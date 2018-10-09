---
title: "geofetch"
---
`geofetch` is a command-line utility that converts GEO or SRA accessions into PEP projects. You provide an accession (or a spreadsheet with a list of accessions), and `geofetch` with produce the two components of a PEP: the project config file and the sample annotation file. `geofetch` will also download the data from SRA, so your project will be ready for direct input into any PEP-compatible tool.

### Code and documentation

* User documentation and vignettes pending
* [Source code at Github](https://github.com/pepkit/geofetch)

### Quick start

`geofetch` is a python script that can be downloaded from github:

```
git clone git@github.com:pepkit/geofetch.git

```

To see the command-line options, run it with `-h`:

```
python geofetch.py -h
```

For example, you can run `geofetch` to build a PEP for your list of accessions like this:

```
python geofetch.py -i GSE##### -m path/to/metadata/folder -n PROJECT_NAME
```

