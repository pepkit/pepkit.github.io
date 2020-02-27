---
title: PEP specification
---

Table of contents: 
* The generated Toc will be an unordered list
{:toc}

## Components of a PEP

PEP divides metadata into project-level metadata and sample-level metadata, which are stored in separate files. A complete PEP consists of up to 3 files:

- **Project config file** - REQUIRED. a `yaml` file describing file paths and optional project settings
- **Sample table** - RECOMMENDED. a `csv` file with 1 row per sample
- **Subsample table** - OPTIONAL. A `csv` file with multiple rows for each sample, used to specify sample attributes with multiple values.

This document describes each of these 3 files in detail.

## Project config file specification

The project config file is a `yaml` file, the only required file for PEP, and the primary source of project-level information. The PEP specification for the project config file recognizes six project attributes. Most are optional:

- `pep_format_version` - REQUIRED
- `sample_table`- RECOMMENDED
- `subsample_table`- OPTIONAL
- `sample_modifiers` - OPTIONAL
- `amendments` - OPTIONAL
- `imports` - OPTIONAL

These attributes may appear in any order. The formal schema is described at [schema.databio.org/PEP/pep_v2.yaml](https://schema.databio.org/PEP/pep_v2.yaml)


### Example

```
pep_format_version: 2.0
sample_table: "path/to/sample_table.csv"
subsample_table: "path/to/subsample_table.csv"
sample_modifiers:
  append:
    attribute1: value
    attr2: val2	
  duplicate:
    oldattr: newattr
  imply:
    - from: organism
      values: ["human"]
      to:
        imp_arr: value
    organism:
      human:
        imp_attr: value
      mouse:
        imp_arr: val2
  derive:
    attributes: [read1, read2, other_attr]
    sources:
      key1: "path/to/derived/value/{sample.attribute}/{project.attribute}"
      key2: "path/to/derived/value/{sample.attribute}/{project.attribute}"

amendments:
  // amendment section here

imports:
  - external_pep.yaml
  - http://url.com/pep.yaml
```

### Project attribute: `pep_format_version`

The only required project attribute, which documents the version of the PEP specification this PEP complies with.

### Project attribute: `sample_table`

The `sample_table` is a path (string) to the sample csv file. It can be absolute or relative; relative paths are assumed relative to the location of the `project_config.yaml` file. The target file is expected to comply with the PEP specification for the sample table, described later.

### Project attribute: `subsample_table`

The `subsample_table` is a path (string) to the subsample csv file. Like with the sample_table attribute, relative paths are assumed relative to the location of the `project_config.yaml` file. The target file is expected to comply with the PEP specification for the subsample table.

### Project attribute: `sample_modifiers`

The sample modifiers allows you to modify sample attributes from within the project configuration file. You can use this to add new attributes to samples in a variety of ways, including attributes whose value varies depending on values of existing attributes, or whose values are composed of existing attribute values. This is a key feature of PEP that allows you to make the sample tables more portable. There are 4 subsections corresponding to 4 types of sample modifier: `append`, `duplicate`, `imply`, and `derive`.

#### *sample_modifiers.append*

This section lets you declare additional attributes, for each of which there's a single value across all samples. This is particularly useful when combined with ``derived_attributes`` and/or ``implied_attributes``, especially when there are many samples.

**Example**:

```yaml
sample_modifiers:
  append:
    data_source: src
    read_type: SINGLE
    organism: mouse
```

#### *sample_modifiers.imply*

``implied`` lets you infer additional attributes, which can be useful for pipeline arguments. For instance, it may that one sample attribute implies several more. Rather than encoding these each as separate, non-varying columns in the annotation sheet, you may simply indicate in the `project_config.yaml` that samples of a certain type should automatically inherit additional attributes.

Example:

```yaml
sample_modifiers:
  imply:
    organism:
      human:
        genome: "hg38"
        macs_genome_size: "hs"
```

This example says that any sample with `organism` attribute set to the string "human" should also automatically set additional attributes of `genome` (with value "hg38") and `macs_genome_size` (with value "hs"). These implied attributes may now be used as pipeline arguments or for other analysis.

For more details, see [implied attributes](/docs/implied_attributes).



#### *sample_modifiers.duplicate*

This modifier allows you to duplicate sample attributes to new attributes with other names. This can be useful if you need to tweak a PEP to work under a different tool that specifies a different schema for the same data.

**Example**:

```yaml
sample_modifiers:
  duplicate:
    old_attribute_name: new_attribute_name
```

#### *sample_modifiers.derive*

The sections called `derive` and `derived_sources` provide a flexible way to point to data files on disk. These two sections go together (so they aren't meaningful independently; if you define one you should define the other). 

The `derived_attributes` section provides a way to link short keys to variable-encoded file paths. The variables in the file paths are formatted as `{variable}`, and are populated by sample attributes (columns in the sample annotation sheet). For example, your files may be stored in `/path/to/{sample_name}.fastq`, where `{sample_name}` will be populated individually for each sample in your PEP.

Example:

```yaml
sample_modifiers:
  derive:
    attributes: [read1, read2, data_1]
    sources:
      key1: "/path/to/raw/data/{sample_name}_{sample_type}.bam"
      key2: "/path/from/collaborator/weirdNamingScheme_{external_id}.fastq"
      key3: "${HOME}/{test_id}.fastq"
```

You can also use shell environment variables (like ``${HOME}``).

The `derived_attributes` section simply identifies which column names (or sample attributes) should be populated as data_sources. Corresponding sample attributes will then have as their value derived from the string replacement of sample attributes specified in the config file. This enables you to point to more than one input file for each sample. For more details and a complete example, see [derived attributes](/docs/derived_attributes).


### Project attribute: `amendments`

Amendments are useful to define multiple similar projects within a single project config file. Under the amendments key, you can specify names of subprojects, and then underneath these you can specify any project config variables that you want to overwrite for that particular subproject.

For example:

```yaml
amendments:
  diverse:
    sample_table: psa_rrbs_diverse.csv
  cancer:
    sample_table: psa_rrbs_intracancer.csv
```

This project would specify 2 subprojects that have almost the exact same settings, but change only their ``metadata.sample_annotation`` parameter (so, each subproject points to a different sample annotation sheet). Rather than defining two 99% identical project config files, you can use a subproject. 

For more details, see [subprojects](/docs/subprojects).


### Project attribute: `imports`

The imports key allows the config file to import other PEP config files. The values in the imported files will be overridden by the corresponding entries in the current config file. Imports are recursive, so an imported file that imports another file is allowed; the imports are resolved in cascading order with the most distant imports happening first, so the closest configuration options override the more distant ones.

Exmaple

```yaml
imports:
  - path/to/parent_project_config.yaml
```

## Sample table specification

The `sample_table` is a `.csv` file containing information about all samples (or pieces of data) in a project. **One row corresponds to one sample**. A sample table may contain any number of columns with any column names. Each columns corresponds to an attribute of a sample. For this reason, we sometimes use the word `column` and `attribute` interchangeably. 

The only requirement for the column names is that the table **MUST** include a column named `sample_name`, which should specify a **unique** string identifying each sample. This should be a string without whitespace (space, tabs, etc...). Any additional columns become attributes of your sample and will be part of the project's metadata for the samples. 

**Example:**
```
"sample_name", "protocol", "organism", "flowcell", "lane",  "data_source"
"albt_0h", "RRBS", "albatross", "BSFX0190", "1", "bsf_sample"
"albt_1h", "RRBS", "albatross", "BSFX0190", "1", "bsf_sample"
"albt_2h", "RRBS", "albatross", "BSFX0190", "1", "bsf_sample"
"albt_3h", "RRBS", "albatross", "BSFX0190", "1", "bsf_sample"
"frog_0h", "RRBS", "frog", "", "", "frog_data"
"frog_1h", "RRBS", "frog", "", "", "frog_data"
"frog_2h", "RRBS", "frog", "", "", "frog_data"
"frog_3h", "RRBS", "frog", "", "", "frog_data"
```

## Subsample table specification

The `subsample_table` is a `.csv` file that annotates multi-value sample attributes. Multiple values for an attribute are specified as multiple rows with the same sample name. The subsample table contains a column named `sample_name` that **must** map to the column of the same name in the sample table.

Here's a simple example. If you define the `sample_table` like this:

```{csv}
sample_name,library
frog_1,anySampleType
frog_2,anySampleType
```

Then point `subsample_table` to the following, which maps `sample_name` to a new column called `file`

```{csv}
sample_name,file
frog_1,data/frog1a_data.txt
frog_1,data/frog1b_data.txt
frog_1,data/frog1c_data.txt
frog_2,data/frog2a_data.txt
frog_2,data/frog2b_data.txt
```

This sets up a simple relational database that maps multiple files to each sample. You can also combine a subsample table with derived attributes; attributes will first be derived and then merged, leading to a very flexible way to point to many files of a given type for single sample.