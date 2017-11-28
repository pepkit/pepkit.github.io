---
title: Merge inputs
---


How do you handle multiple input files for a single sample?

Sometimes you have multiple input files that you want to merge for one sample. For example, a common use case is a single library that was spread across multiple sequencing lanes, yielding multiple input files that need to be merged, and then run through the pipeline as one. Or, perhaps you have technical replicates that you want merged before running a pipeline. Rather than putting multiple lines in your sample annotation sheet, which causes conceptual and analytical challenges, we introduce two ways to merge these:

1. Use shell expansion characters (like `*` or `[]`) in your `data_source` definition or filename (good for simple merges)
2. Specify a *merge table* which maps input files to samples for samples with more than one input file (infinitely customizable for more complicated merges).

## Option 1: wildcards

To do the first option, just change your data source specifications, like this:

.. code-block:: yaml

      data_R1: "${DATA}/{id}_S{nexseq_num}_L00*_R1_001.fastq.gz"
      data_R2: "${DATA}/{id}_S{nexseq_num}_L00*_R2_001.fastq.gz"

# Option 2: the merge table

To do the second option, just provide a merge table in the *metadata* section of your project config:

```
metadata:
  merge_table: mergetable.csv
```

Make sure the `sample_name` column of this table matches, and then include any columns you need to point to the data. `PEP` will automatically include all of these files as appropriate. You can also combine merged columns with derived columns; files will first be derived and then merged, leading to a very flexible way to point to any files you need to merge.

Warning: do not use both of these options simultaneously for the same sample, it will lead to multiple merges.

Note: to handle different *classes* of input files, like read1 and read2, these are *not* merged and should be handled as different columns in the main sample annotation sheet (and therefore different arguments to the pipeline).
