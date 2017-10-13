---
title: A simple example
#permalink: /docs/example/
---

To use the PEP toolkit, you must define your project using PEP structure. The format is simple and modular, so you only need to define the components you plan to use. You need to supply 2 files:

1. **Project config file** - a ``yaml`` file describing file paths and optional project settings
2. **Sample annotation sheet** - a ``csv`` file with 1 row per sample


In the simplest case, ``project_config.yaml`` is just a few lines of ``yaml``, a simple hierarchical markup language used to store key-value pairs. You can <a href="http://www.yaml.org/start.html">read more about yaml here</a>. Here's a minimal example `project_config.yaml`:


```{yaml}
metadata:
 output_dir: /path/to/output/folder
 sample_annotation: /path/to/sample_annotation.csv
```

The `output_dir` key specifies where to save results. The `sample_annotation` key points to another file, which is a comma-separated value (``csv``) file describing samples in the project. Here's a small example of `sample_annotation.csv`:

```{csv}
"sample_name", "library", "file"
"frog_1", "RNA-seq", "frog1.fq.gz"
"frog_2", "RNA-seq", "frog2.fq.gz"
"frog_3", "RNA-seq", "frog3.fq.gz"
"frog_4", "RNA-seq", "frog4.fq.gz"
```

With those two simple files, you are ready to use the pepkit tools! Now, with a single line of code, you could load your metadata into R using <a href="/docs/R_package/">pepr</a>, into python using <a href="/docs/python_package/">pep</a>, or run each sample through an arbitrary command-line pipeline using <a href="/docs/looper/">looper</a>. 

If you make a habit of describing all your projects like this, you'll never parse another sample annotation sheet again. You'll never write another pipeline submission loop.

This is the beginning. In practice, you may want to use some of the more advanced features of PEP structure by adding additional information to your configuration ``yaml`` file and your sample annotation ``csv`` file. The next sections will go through the more advanced details of both annotation sheets and project config files.
