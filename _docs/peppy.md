---
title: "Python package: peppy"
---

<img src="/img/logo_python.svg" alt="" style="float:left; margin:20px">`peppy` is a python package that loads PEPs. It instantiates an in-memory representation of metadata for your project and all of its samples, for any downstream purpose. `peppy` is useful for software developers or data analysts who use python.

### Code and documentation

* [User documentation and vignettes](http://peppy.readthedocs.io/)
* [peppy API](http://peppy.readthedocs.io/en/latest/api.html)
* [Source code at Github](https://github.com/pepkit/peppy)

### Quick start 

Peppy is on pypi. Install with:
```
pip install peppy
```

Or use `pip install --user --upgrade peppy` to install a local copy. Then you can load your project into Python with this code:

```
import peppy

my_project = peppy.Project("path/to/project_config.yaml")
my_samples = my_project.samples
```

Once you have your project and sample metadata in your Python session, the possibilities are endless.