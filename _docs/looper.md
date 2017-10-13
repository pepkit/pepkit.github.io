---
title: Looper
---

`looper` is a command-line job submitting engine. It's the **glue that connects your PEP project to command-line pipelines**. It can work with any pipeline that takes command-line arguments. Once your PEP project and your pipelines are configured, you run your samples through `looper` on the command line with this command:

```{bash}
looper run project_config.yaml
```

That command will parse your PEP samples, and build jobs for each one. It’s scalable: by default, it runs your jobs sequentially on the local computer, but with a small configuration change, it will create and submit jobs to any cluster resource manager (like SLURM, SGE, or LFS).

To check on your running jobs, you run:

```{bash}
looper check project_config.yaml
```

To summarize the statistics of all the pipeline runs, you type:

```{bash}
looper summarize project_config.yaml
```

`looper` is modular and totally configurable, so it scales as your needs grow. It is a cog in pepkit that makes it easy to run any kind of command-line tool serially across all the samples in your project. We provide a suite of already-built pipelines for ease-of-use, but you can configure it to work with just about anything. You have complete control.

`looper` is not a pipeline workflow engine, which is used to build pipelines. `looper` assumes you already have pipelines built, and it helps you map samples to those pipelines.

You can find further details at [http://looper.readthedocs.io](http://looper.readthedocs.io).