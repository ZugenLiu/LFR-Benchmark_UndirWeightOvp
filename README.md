# LFR-Benchmark_UndirWeightOvp
Extended version of the Lancichinetti-Fortunato-Radicchi Benchmark for Undirected Weighted Overlapping networks to evaluate clustering algorithms

## Description
This program is an implementation of the algorithm described in the paper "Directed, weighted and overlapping benchmark graphs for community detection algorithms", written by Andrea Lancichinetti and Santo Fortunato. In particular, this program is to produce undirected weighted networks with overlapping nodes.  
Each feedback is very welcome. If you have found a bug or have problems, or want to give advises, please contact us:

andrea.lancichinetti@isi.it  
fortunato@isi.it

Turin, 29 October 2009

Original sources:

* [Benchmark graphs for testing community detection algorithms](https://sites.google.com/site/santofortunato/inthepress2)
* [Benchmarks](https://sites.google.com/site/andrealancichinetti/files)

**Reference:** A. Lancichinetti, S. Fortunato, and F. Radicchi.(2008) [Benchmark graphs for testing community detection algorithms.](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.78.046110) Physical Review E, 78.

> This is an extended version of the original LFR framework (see the [changelog](#changelog)) by Artem Lutov <artem@exascale.info>

## Content
- [Compilation](#compilation)
- [Usage](#usage)
  - [Random Seed](#random-seed)
  - [Accessory Options](#accessory-options)
- [Examples](#examples)
- [Output](#output)
- [Acknowledgement](#acknowledgement)
- [Changelog](#changelog)
- [Related Projects](#related-projects)

## Compilation 

In order to compile, type: `$ make`

## Usage

To run the program, type:  
```
./benchmark [FLAG] [P]

[FLAG]		[P]
-N		number of nodes
-k		average degree
-maxk	maximum degree
-mut	mixing parameter for the topology
-muw	mixing parameter for the weights
-beta	exponent for the weight distribution
-t1		minus exponent for the degree sequence
-t2		minus exponent for the community size distribution
-minc	minimum for the community sizes
-maxc	maximum for the community sizes
-on		number of overlapping nodes
-om		number of memberships of the overlapping nodes
-C		[average clustering coefficient]
```

In this program you can assign the number of overlapping nodes (option -on) and assign the number of memberships for them (option `-om`). The other nodes will have only one membership. For instance, typing 
```
./benchmark [flags...] -N 1000 -on 20 -om 2
```
will produce a network with 1000 nodes, 980 with only one membership and 20 nodes with two memberships.

It is also possible to set the parameters writing flags and relative numbers in a file (look at flags.dat to see an example). To specify the file, use the option: `-f	[filename]`

You can set the parameters both writing some of them in the file, and using flags from the command line for others (if you set a parameter twice, the latter one will be taken).

`-N, -k, -maxk, -muw` have to be specified. For the others, the program can use default values: `t1=2, 	t2=1, 	on=0,	om=0,	beta=1.5, mut=muw, minc` and `maxc` will be chosen close to the degree sequence extremes.  

The flag `-C` is not mandatory. If you use it, the program will perform a number of rewiring steps to increase the average cluster coefficient up to the wished value.
Since other constraints must be fulfilled, if the wished value will not be reached after a certain time, the program will stop (displaying a warning).

### Random Seed

In the file time_seed.dat you can edit the seed which generates the random numbers. After reading, the program will increase this number by 1 (this is done to generate different networks running the program again and again). If the file is erased, it will be produced by the program again.

### Accessory Options

To have a random network use: `-rand`  
Using this option will set` mut=0`, `muw=0` and `minc=maxc=N`, i.e. there will be one only community.

Use option: `-sup (-inf)`

if you want to produce a benchmark whose distribution of the ratio of external degree/total degree is superiorly (inferiorly) bounded by the mixing parameter (only for the topology). In other words, if you use one of these options, the mixing parameter is not the average ratio of external degree/total degree (as it used to be) but the maximum (or the minimum) of that distribution. When using one of these options, what the program essentially does is to approximate the external degree always by excess (or by defect) and if necessary to modify the degree distribution. Nevertheless, this last possibility occurs for a few nodes and numerical simulations show that it does not affect the degree distribution appreciably.


## Examples
Example1:  `./benchmark -N 1000 -k 15 -maxk 50 -muw 0.1 -minc 20 -maxc 50`  
Example2:  `./benchmark -f flags.dat -t1 3`

If you want to produce a kind of Girvan-Newman benchmark, you can type:  
`./benchmark -N 128 -k 16 -maxk 16 -muw 0.1 -minc 32 -maxc 32 -beta 1`

## Output
Please note that the community size distribution can be modified by the program to satisfy several constraints (a warning will be displayed).

The program will produce three files:

1. Network.dat contains the list of edges (nodes are labelled from 1 to the number of nodes; the edges are ordered and repeated twice, i.e. source-target and target-source), with the relative weight.
1. Community.dat contains a list of the nodes and their membership (memberships are labelled by integer numbers >=1).
1. Statistics.dat contains the degree distribution (in logarithmic bins), the community size distribution, the distribution of the mixing parameter for the topology and the weights, and the internal and external weight distribution.

## Acknowledgement
Thanks to:
- Peter Ronhovde and Conrad Lee, for many usuful advises
- Rodrigo Rocha Gomes e Souza, for reporting bugs (and also fixing them)
- Filippo Radicchi and Jos Ramasco for testing the program

## Changelog
Additionally implemented features on top of the original LFR Benchmark are the following:
- Parameter `-cnl` (ommunity nodes list) added to output communities (clusters) as lists of
nodes to be compatible with NMI evaluation input format (.cnl)
- Parameter `-name` added to give custom name for the output files
- `maxk` is automatically decreased if required for the network generation with the specified `k`
(instead of the generation process termination)

## Related Projects
- [PyCABeM](https://github.com/eXascaleInfolab/PyCABeM) - Python Benchmarking Framework for the Clustering Algorithms Evaluation. Uses extrinsic (NMIs) and intrinsic (Q) measures for the clusters quality evaluation considering overlaps (nodes membership by multiple clusters).
- [PyNetConvert](https://raw.githubusercontent.com/eXascaleInfolab/PyNetConvert) - Network (graph, dataset) converter from Pajek, Metis and .nsl formats (including *.ncol*, Stanford SNAP and Edge/Arcs Graph) to *.nsl* (*.nse/a* that are more common than *.snap* and *.ncol*) and *.rcg* (Readable Compact Graph, former *.hig*; used by DAOC / HiReCS libs) formats.

**Note:** Please, [star this project](https://github.com/eXascaleInfolab/LFR-Benchmark_UndirWeightOvp) if you use it.
