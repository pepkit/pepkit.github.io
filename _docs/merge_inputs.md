---
title: Merge inputs
---


How do you handle multiple input files for a single sample?

Sometimes you have multiple input files that you want to merge for one sample. For example, a common use case is a single library that was spread across multiple sequencing lanes, yielding multiple input files that need to be merged, and then run through the pipeline as one. Or, perhaps you have technical replicates that you want merged before running a pipeline. Rather than putting multiple lines in your sample annotation sheet, which causes conceptual and analytical challenges, we introduce two ways to merge these:

1. Use shell expansion characters (like `*` or `[]`) in your `data_source` definition or filename (good for simple merges)
2. Specify a *merge table* which maps input files to samples for samples with more than one input file (infinitely customizable for more complicated merges).

## Option 1: wildcards

To do the first option, just change your data source specifications, like this:


```{yaml}
data_R1: "${DATA}/{id}_S{nexseq_num}_L00*_R1_001.fastq.gz"
data_R2: "${DATA}/{id}_S{nexseq_num}_L00*_R2_001.fastq.gz"
```

## Option 2: the merge table

To do the second option, just provide a merge table in the *metadata* section of your project config:

```{yaml}
metadata:
  sample_annotation: annotation.csv
  merge_table: merge_table.csv
```

Make sure the `sample_name` column of this table matches, and then include any columns you need to point to the data. `PEP` will automatically include all of these files as appropriate. 

Here's a simple example of a PEP that uses merging. If you define the `sample_annotation` like this:

```{csv}
sample_name,library
frog_1,anySampleType
frog_2,anySampleType
```

Then point `merge_table.csv` to the following, which maps `sample_name` to a new column called `file`

```{csv}
sample_name,file
frog_1,data/frog1a_data.txt
frog_1,data/frog1b_data.txt
frog_1,data/frog1c_data.txt
frog_2,data/frog2a_data.txt
frog_2,data/frog2b_data.txt
```

This sets up a simple relational database that maps multiple files to each sample. You can also combine merged columns with derived columns; columns will first be derived and then merged, leading to a very flexible way to point to any files you need to merge. 

Warning: do not use both wildcards and a merge table simultaneously for the same sample, as it may lead to multiple merges.

Note: to handle different *classes* of input files, like read1 and read2, these are *not* merged and should be handled as different columns in the main sample annotation sheet (and therefore different arguments to the pipeline).
