---
title: Getting started
permalink: /docs/home/
redirect_from: /docs/index.html
---

*Portable Encapsulated Projects* provides you with a structure and toolkit for organizing large-scale research projects. Let's break that down into two components that work together to form this concept: 

1. the standardized structure for organizing a project, which we call *PEP*
2. a toolkit with several pieces of software that are built around that standard

**Creating a PEP**. The first component is documented here in the section called *Creating a PEP*, which contains simple examples to get you started as well as detailed documentation for reference. This is the place to start if you're interested in using PEP structure to organize your projects and the primary source of documentation describing universal features of PEP structure. Start with [a simple example of a PEP](/docs/simple_example/).

**Using PEP tools**. The second component is outlined in the section called *Reading a PEP*. This page provides a brief introduction to each of the tools under development that can make use of your project. Each tool is its own separate project and has comprehensive documentation elsewhere, which you can find linked from the individual pages here. This will give you an overview of what is already available that can get you started analyzing your PEP or building a new PEP-compatible tool, but you should consult the detailed documentation for a particular tool for further information.


## How is this useful?
<img src="/img/data-munging.svg" alt="" style="float:right; margin-left:20px" width="350px">

In a data analysis project, we frequently want to try a series of different tools all on the same data. Too often, running several different tools requires a unique structure for each analysis. This makes it difficult to do things like run several different analyses on one dataset, or plug several different datasets into one analysis, because each connection structure must be defined manually.

To alleviate this challenge of linking data to tools, Portable Encapsulated Projects (PEP) aims to standardize the description of data collections, enabling both data providers and data users to communicate through the common interface of a standard format. Practically, this means individuals who describe their projects using this format will immediately inherit both greater portability for analysis as well as greater access to external complementary data. This link operates around a simple, standard, extensible definition of a project. 
