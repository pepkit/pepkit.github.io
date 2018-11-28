---
title: Introduction
permalink: /docs/home/
redirect_from: /docs/index.html
---

*Portable Encapsulated Projects* (*PEP* for short) is a standardized way to organize project metadata. **A *project* is a collection of data with associated metadata.** PEPs are typically data-intensive bioinformatics projects with many samples (which could be individual experiments, organisms, cell lines, *etc.*). However, the concepts are generic and could be applied to any project that represents metadata in tabular form.

## How is this useful?

<img src="/img/data-munging.svg" alt="" style="float:right; margin-left:20px" width="350px">

In a data analysis project, we frequently want to run many different tools on the same input data. Too often, this requires structuring the data uniquely for each tool. This makes it difficult to test multiple tools because each connection structure must be defined manually.

To alleviate this challenge of linking data to tools, Portable Encapsulated Projects (PEP) standardizes the description of data collections, enabling both data providers and data users to communicate through a common interface. This link operates around a simple, standard, extensible definition of a <i>project</i>, plus a suite of software tools that operate on that standard.

PEP makes it easy to:

1. describe all your projects the same way, so you only have to learn one metadata structure
2. work collaboratively, since the organization is standardized
3. share your project with others, who are already familiar with the standard
4. employ a variety of tools or pipelines without restructuring the metadata
5. analyze attributes of your project in the data analysis environment of your choice (R/python).


To use PEP, you'll first have to either create or download a PEP. Then, you will be able to process that metadata using tools in *pepkit*.

**Creating a PEP**. This web page contains simple examples and detailed documentation for creating a PEP. Start with [a simple example of a PEP](/docs/simple_example/).

**Using PEP tools**. With a PEP in hand, you'll then be able to use any PEP-compatible tool to process your project. This web page provides a brief introduction and link to each of the tools in `pepkit` that can read your project. There is also a [list of PEP-compatible external tools](/docs/software/) and [projects that use PEP](/docs/projects/).
