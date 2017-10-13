---
title: "R package: pepr"
---

<img src="/img/logo_R.svg" alt="" style="float:left; margin:20px">

`pepr` is an R package for reading Portable Encapsulated Projects. Portable Encapsulated Projects (PEP) are sample-heavy projects that subscribe to the standard PEP project definition. If you describe your project (configuration and samples) according to this format, you can load all project metadata into R using the pepr package. These projects can also be used for any PEP-compatible pipeline (see this list of compatible pipelines).

pepr is currently in alpha mode and should not be used production projects. It is made available for conceptual and testing purposes only.

The `pepr` project is hosted at Github: [https://github.com/pepkit/pepr](https://github.com/pepkit/pepr)

```{R}
library('pepr')
p = Project(file = "~/code/microtest/config/microtest_config.yaml")

samples(p)
config(p)

p@config
p@samples
```