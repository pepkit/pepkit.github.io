---
title: Looper
---

`looper` is a command-line job submitting engine. If you have a pipeline and a bunch of samples you want to run, `looper` can help you organize the inputs and outputs.

It’s scalable: by default, it runs your jobs sequentially on the local computer, but with a small configuration change, it will create and submit jobs to any cluster resource manager (like SLURM, SGE, or LFS).

The basics: You describe your project using **PEP format**. Pass your *project configuration file* as input to `looper`, which parses it, reads your sample list, maps each sample to the appropriate pipeline, and creates and runs (or submits) job scripts. Easy.

Run your PEP project through looper on the command line with this command:

```{bash}
looper run project_config.yaml
```

`looper` is modular and totally configurable, so it scales as your needs grow. We provide sensible defaults for ease-of-use, but you can configure just about anything. You have complete control. Looper handles the mundane project organization tasks that you don’t want to worry about.

Do not confuse `looper` with a pipeline workflow engine, which is used to build pipelines. `looper` assumes you already have pipelines built, and it helps you map samples to those pipelines.

You can find further details at [http://looper.readthedocs.io](http://looper.readthedocs.io).