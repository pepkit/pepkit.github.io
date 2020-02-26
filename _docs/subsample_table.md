---
title: Subsample table
---

Occasionally, a sample will really have two or more values for a single attribute. This doesn't fit naturally into a tabular data format, but requires the one-to-many relationship that is better handled by a relational database. For these kinds of more complex relationships, PEP offers a second annotation table called `sample_subannotation`, which is added as an attribute in the `metadata` section of your `project_config`. To explain how this works, we'll use the most common example case of needing it: a single sample with multiple input files.

## How do you handle multiple input files for a single sample?

Sometimes you have multiple input files that you want to merge for one sample. For example, perhaps you have technical replicates that you want merged before running a pipeline. Or, a common use case is a single library that was spread across multiple sequencing lanes, yielding multiple input files.

Let's make something very clear: **merging inputs is for files of the same type**. A paired-end sequencing experiment, for example, yields two input files for each sample (read1 and read2). These are *not equivalent*, so **you do not merge read1 and read2**; instead, you would merge all files (perhaps from different lanes) for read1, and then you would merge all files for read2. This is two separate attributes, which each may have multiple files -- it's the multiple files we want to merge, not the separate attributes.

Rather than putting multiple lines in your sample annotation sheet, which causes conceptual and analytical challenges, we introduce two ways to merge these:

1. Use shell expansion characters (like `*` or `[]`) in your `data_source` definition or filename (good for simple merges)
2. Specify a *sample subannotation table* (formerly called a *merge table*), which maps input files to samples for samples with more than one input file (infinitely customizable for more complicated merges).


## Option 1: wildcards

To do the first option, just change your data source specifications, like this:


```{yaml}
data_R1: "${DATA}/{id}_S{nexseq_num}_L00*_R1_001.fastq.gz"
data_R2: "${DATA}/{id}_S{nexseq_num}_L00*_R2_001.fastq.gz"
```

## Option 2: the sample_subannotation table

To do the second option, just provide a sample_subannotation table (formerly called a merge table) in the *metadata* section of your project config:

```{yaml}
metadata:
  sample_annotation: annotation.csv
  sample_subannotation: sample_subannotation.csv
```

Make sure the `sample_name` column of this table matches, and then include any columns you need to point to the data. `PEP` will automatically include all of these files as appropriate. 

Here's a simple example of a PEP that uses merging. If you define the `sample_annotation` like this:

```{csv}
sample_name,library
frog_1,anySampleType
frog_2,anySampleType
```

Then point `sample_subannotation.csv` to the following, which maps `sample_name` to a new column called `file`

```{csv}
sample_name,file
frog_1,data/frog1a_data.txt
frog_1,data/frog1b_data.txt
frog_1,data/frog1c_data.txt
frog_2,data/frog2a_data.txt
frog_2,data/frog2b_data.txt
```

This sets up a simple relational database that maps multiple files to each sample. You can also combine merged columns with derived columns; columns will first be derived and then merged, leading to a very flexible way to point to any files you need to merge. Let's now look at a slightly more complex example that has two attributes we want to merge (such as the case with a paired-end sequencing experiment with multiple files for each R1 and R2):

```{csv}
sample_name	library
frog_1	anySampleType
frog_2	anySampleType
```

```{csv}
sample_name,read1,read2
frog_1,data/frog1a_R1.txt,data/frog1a_R2.txt
frog_1,data/frog1b_R1.txt,data/frog1b_R2.txt
frog_1,data/frog1c_R1.txt,data/frog1c_R2.txt
frog_2,data/frog2a_R1.txt,data/frog2a_R2.txt
frog_2,data/frog2b_R1.txt,data/frog2b_R2.txt
```

You can find more examples of projects that use merging in the [example PEPs list](/docs/example_PEPs).

A few tidbits you may need to consider when using sample_subannotation tables to merge files:

- Sample subannotation tables are intended to handle multiple values *of the same type*. To handle different *classes* of input files, like read1 and read2, these are *not* the same type, and are therefore *not* put into a sample subannotation table. Instead, these should be handled as different columns in the main sample annotation sheet (and therefore different arguments to the pipeline). It is possible that you will want to have read1 and read2, and then each of these could have multiple inputs, which would then be placed in the subannotation table.

- If your project has some samples with subannotations, but others without, then you only need to include samples in the subannotation table if they have subannotations. Other sampels can just be included in the primary annotation table. However, this means you'll need to make sure you provide the correct columns in the primary `sample_annotation` sheet; the simple example above assumes every sample has subannotations, so it doesn't need to define `file` in the `sample_annotation`. If you had unmerged samples, you'd need to specify that column in the primary sheet.

- In practice, we've found that almost every project can be solved using wildcards, and subannotation tables are not necessary. If you start to think about how to use subannotation tables, first double-check that you can't solve the problem using a simple wildcard; that makes it much easier to think about, and it should be possible as long as the files are named systematically.

- Warning: While you may use both wildcards and subannotation tables for different samples within a project; **do not use both wildcards and a subannotation table simultaneously for the same sample**, as it may lead to multiple merges.

