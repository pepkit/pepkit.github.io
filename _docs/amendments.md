---
title: Amendments
---

The `amendments` section of the config file allows you to include variations of the project. When a PEP is parsed, one or more amendments can be included to amend the values in the processed PEP. This is a powerful function that can be used for many purposes, such as *on the fly* tweaks or embedding multiple subprojects within a parent project.

## Example

```
sample_table: annotation.csv
amendments:
  my_project2:
    sample_table: annotation2.csv
  my_project3:
    sample_table: annotation3.csv
````


## Rationale

At times you will want to create two projects that are very similar, but differ just in one or two attributes. For example, you may define a project with one set of samples, and then want an identical project but using a different sample annotation sheet. Or, you may define a project to run on a particular reference genome, and want to define a second project that is identical, but uses a different reference genome.

You could simply define 2 complete PEPs, but this would duplicate information and make it harder to maintain. Instead, you can use *subprojects*, which allow you to encode additional similar projects all within the original `project_config.yaml` file. Subprojects are like mini embedded `project_config.yaml` files that can be *activated* by software. 

For example, consider this `project_config.yaml`: 

```
metadata:
  sample_annotation: annotation.csv

subprojects:
  my_project2:
	metadata:
	  sample_annotation: annotation2.csv
  my_project3:
	metadata:
	  sample_annotation: annotation3.csv
```

If you load this configuration file, it will by default use the `annotation.csv` file specified in the primary `metadata` section, as you would expect. If you don't directly specify that you want to use a subproject, then they are ignored. But if you choose, you may activate one of the two subprojects, which are called `my_project2` and `my_project3`. If you activate `my_project2`, then the `metadata.sample_annotation` attribute would now have a value of `annotation2.csv`; everything else about the project would be the same as the primary project, because these existing values are not overridden.

Practically what happens under the scenes is that the primary project is first loaded, and then, if a subproject is activated, it overrides any attributes with those specified in the subproject.

## How do you activate a subproject?

Activating a subproject depends on what software you're using to load your PEP. You can tell looper to load a particular subproject by passing `--sp subproject-name` on the command line. You can activate a subproject in `peppy` or `pepr` by passing an argument, `subproject=my_project2`, when you construct the `Project` object.
