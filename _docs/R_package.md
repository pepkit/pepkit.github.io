---
title: "R package: pepr"
---

<img src="/img/logo_R.svg" alt="" style="float:left; margin:20px">

`pepr` is an R package for reading Portable Encapsulated Projects. It will read PEP projects, loading all project and sample metadata into R with a single line of code.

Install `pepr` with `devtools::install_github("pepkit/pepr")`, then load up your project like this:


```{R}
p = pepr::Project(file="project_config.yaml")
```

Now you can retrieve all your configuration information or sample metadata with easy getter functions:
```{R}
samples(p)
config(p)

p@config
p@samples
```

`pepr` is currently in alpha mode and should not be used production projects. It is made available for conceptual and testing purposes only.

The complete documentation will be hosted at http://code.databio.org/pepr (pending).

The `pepr` source is hosted at Github: [https://github.com/pepkit/pepr](https://github.com/pepkit/pepr)