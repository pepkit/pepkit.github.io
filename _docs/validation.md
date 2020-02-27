---
title: "PEP Validation"
---

## Validating a generic PEP

You can validate a PEP for the generic PEP format like this:

```
peppy validate path/to/project_config.yaml -s https://schema.databio.org/PEP/pep_v2.yaml
```

## Validating a PEP for a specific tool

Even more useful than this is to validate your PEP against a more strict schema, like one for a particular pipeline or workflow. Now, most tools will require more information than is provided by a generic, minimal PEP. For example, a particular tool may need to have a specific attribute set for each sample, such as `genome`. 

## Writing 

We recommend that workflow authors write a PEP schema that describes what sample and project attributes are required for their tool to work.

Here, we should describe how to go about writing a schema for your tool.


