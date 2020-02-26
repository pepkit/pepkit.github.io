---
title: PEP specification basics
---

PEP divides project metadata into two types: project-level metadata and sample-level metadata. These two types are stored in two different files, so a complete PEP consists of (at least) 2 files:

1. **Project config file** - a `yaml` file describing file paths and optional project settings
2. **Sample table** - a `csv` file with 1 row per sample


## Example

```
config_version: 2.0
sample_table: path/to/sample_table.csv
sample_modifiers:
  constant:
    attribute1: value
    attr2: val2	
  implied:
    organism:
      human:
        imp_attr: value
      mouse:
        imp_arr: val2
  duplicated:
    oldattr: newattr
  derived: [read1, read2, other_attr]
  derived_sources:
    key1: "path/to/derived/value/{sample.attribute}/{project.attribute}"
    key2: "path/to/derived/value/{sample.attribute}/{project.attribute}"

amendments:
  etc.

imports:
  - external_pep.yaml
  - http://url.com/pep.yaml
```