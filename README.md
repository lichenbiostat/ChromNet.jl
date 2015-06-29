
# ChromNet.jl

Integrates any bed file into the ChromNet group graphical model, the JSON network file produced can then be explored using http://chromnet.cs.washington.edu.

[![Build Status](https://travis-ci.org/slundberg/ChromNet.jl.svg?branch=master)](https://travis-ci.org/slundberg/ChromNet.jl)


## Installation

- Ensure that a recent version of [Julia](http://www.julialang.com) is installed. ChromNet is written in Julia for performance and portability reasons.
- Download the [current data package](http://cloud.google.com/sdfsdf) which contains the ChromNet processed version of all ENCODE ChIP-seq data. Also contained in this package is a Julia script, which when run, will generate a new ChromNet model using the ENCODE data and any user-provided BED files.

Currently the clustering method used is part of an unreleased version of the Clustering.jl package. This means that for the moment you must also checkout the most recent (unreleased) clustering package. This can be done from the command line using:

```bash
julia -e 'Pkg.checkout("Clustering")'
```

The Travis build currently fails because of this dependence on an unreleased package version.


## Usage

Decompress the downloaded data package and run

```bash
julia build_network.jl CONFIG_FILE -o output_file.json
```

from inside the directory. The output JSON file can be dropped into the ChromNet interface at http://chromnet.cs.washington.edu, the config file lists custom BED files to be incorporated into the network. Each line of the config file should conform to the following TAB separated format, where trailing entries can be omitted:

```
BED_FILE_NAME SHORT_TITLE LONG_TITLE CELL_TYPE LAB EXPERIMENT_ID ANTIBODY_ID TREATMENTS ORGANISM LIFE_STAGE LINK
```

The simplest configuration file is just a list of BED file names, and `build_network.jl` supports STDIN and STDOUT streaming. This means a one line invocation of ChromNet on UNIX systems is simply (where '-' denotes STDIN):

```shell
ls ~/my_bed_files/*.bed | julia build_network.jl - > output_file.json
```
