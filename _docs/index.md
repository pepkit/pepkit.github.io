---
title: Introduction
permalink: /docs/home/
redirect_from: /docs/index.html
---

Organizing and annotating sample data is an important task in data-intensive bioinformatics, but each project is typically organized uniquely. Furthermore, data processing tools typically expect a unique format for sample annotation. There is no standardized way to represent sample-heavy project metadata that spans projects and tools.

*Portable Encapsulated Projects* (*PEP* for short) seeks to make projects more portable and self-contained by formally specifying a structure for project metadata. **A *project* is a collection of data with associated metadata.** PEPs are typically data-intensive bioinformatics projects with many samples (which could be individual experiments, organisms, cell lines, *etc.*). However, the concepts are generic and could be applied to any project that represents metadata in tabular form. In addition to providing a sample metadata format, the PEP specification also provides features that make metadata more portable, across both computing environments and processing tools. 

<img src="/img/data-munging.svg" alt="" style="float:right; margin-left:10px" width="300px">

In a data analysis project, we frequently want to run many different tools on the same input data. Too often, this requires structuring the data uniquely for each tool. This makes it difficult to test multiple tools because each connection structure must be defined manually.

To alleviate this challenge of linking data to tools, Portable Encapsulated Projects (PEP) standardizes the description of data collections, enabling both data providers and data users to communicate through a common interface. This link operates around a simple, standard, extensible definition of a <i>project</i>.

PEP makes it easy to:

1. use one metadata structure for all your projects
2. work collaboratively
3. share your project with others
4. use multiple tools without restructuring metadata
5. analyze data in both R *and* Python


This web page outlines the PEP specification. Once you have a PEP, you will be able to process that metadata using tools in *pepkit*. You can find tools to use on your PEP in the [list of PEP-compatible external tools](/docs/software/) and [projects that use PEP](/docs/projects/).
