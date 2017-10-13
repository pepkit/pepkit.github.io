---
title: "Python package: pep"
---

<img src="/img/logo_python.svg" alt="" style="float:left; margin:20px"> If you define your project using PEP, you can use our python project models to instantiate an in-memory representation of your project and all of its samples, for any downstream purpose. Here is a brief description of how you would do this. The python `pep` package is being derived from the models module in the `looper` package. This is a work in progress. In the meantime, you can get the same effect by using the `looper` package, where `pep` currently resides.

```
from looper import models

my_project = models.Project("path/to/project_config.yaml")
my_samples = my_project.samples
```

This will eventually be:
```
import pep

my_project = pep.Project("path/to/project_config.yaml")
my_samples = my_project.samples
```

Once you have your project and samples in your Python session, the possibilities are endless. This is the way `looper` reads your project; `looper` uses these objects to loop through each sample and submit pipelines for each. You could just as easily use these objects for other purposes; for example, one way we use these objects is for post-pipeline processing. We can load the project and it sample objects into an analysis session, where we do comparisons across samples.

To read more about using `pep` in python interactively, or to build tools using the `pep` API, follow these resources:

- [pep documentation](http://looper.readthedocs.io/en/latest/models.html)
- [pep API](http://looper.readthedocs.io/en/latest/api.html)
