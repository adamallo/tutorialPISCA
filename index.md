# Tutorial PISCA

## READ FIRST
### Virtual Machine or Native installation?
You can follow this tutorial using your own system or a virtual machine image I will provide for the ACE Bootcamp. A virtual machine is as emulation of a computer system, sort of a simulated computer within your computer. If you use the virtual machine image provided (with a Linux distribution, specifically, Ubuntu 18.04.3 LTS), you will not have to install any software in your computer, apart from a virtualization product (e.g., [VirtualBox](https://www.virtualbox.org/wiki/Downloads){:target="_blank"}) and all software needed to follow the tutorial will be included in the machine. The execution of PISCA will be slower, but you will be able to follow the tutorial. **I recommend using the virtual machine to users that do not plan to use PISCA or BEAST in the near future, and do not plan to analyze data of their own today**. Otherwise, I think this may be a good opportunity for you to install the software for future usage.

**License disclaimer: I will provided the virtual machine for its usage during the ACE Bootcamp only. Do not distribute it.**

### Hyperlinks
I have included a lot of supplementary information and reading materials linked along the text. Please, feel free to wander away, either during the tutorial session (making sure you are able to finish the assignment) or definitely afterwards. 
Other resources of interest:
- [BEAST2 book](https://www.beast2.org/book/){:target="_blank"}
- [Taming the BEAST tutorials](https://taming-the-beast.org/tutorials/)

## Exercise 1: Start your first PISCA analysis
- Execute (double-click) the BEAST program in your desktop
- Find the input file *** in Desktop/tutorial/datasets
- Start the analysis
- This analysis will take from ** to *** minutes. Please, use this time to get familiarized with the software we will be using along this tutorial (below)

## Software relevant for this tutorial
## [BEAST](http://beast.community/index.html){:target="_blank"}
Platform for Bayesian analysis of molecular sequences using the Markov Chain Monte Carlo algorithm. At its core, it estimates rooted chronograms evolving under strict or relaxed clock models. It can be *easily* expanded with more models and data structures in order not to reinvent the wheel. 

Among others, BEAST has models for:

- Tree models
	- Population size estimation (growth)
- Clock models (strict, relaxed, epoch...)
- Data types
	- DNA
	- Codon
	- AminoAcid
	- SSR
- Discrete phylogeography
- Continuous phylogeography
- Species-tree reconstruction

### Input
BEAST uses a single input file that contains the input data, models, and MCMC parameters (priors, operators, chain options) in [XML](https://en.wikipedia.org/wiki/XML){:target="_blank"} format. This file is usually long and relatively difficult to read for a human being.

### Output
BEAST generates two main output files, a .log file and a .trees file. The first contain the posterior sample of the model parameter values, the second, the posterior sample of the trees (topologies, branch lengths, and some other branch-specific parameters).

***NOTE***: There are currently two main implementations of BEAST, [BEAST1.X](http://beast.community/index.html) and [BEAST2.X](http://www.beast2.org/). They have a pretty recent common ancestor but are currently evolving independently. In this tutorial we will be using BEAST1.8 

## BEAUti
Graphical user interface distributed with BEAST that makes it easier to generate XML files for the most common scenarios. **Unfortunately, PISCA is not yet compatible with BEAUti**. For this reason, along most of this tutorial you will be working with pre-generated XML files, although you may have to edit small sections of it. PISCA is distributed with scripts that help users generate XML files for the most common PISCA analyses.

## [PISCA](https://github.com/adamallo/PISCA){:target="_blank"}
BEAST1.8 plugin that expands BEAST with models to reconstruct the evolution of homogeneous somatic samples using allele-specific integer copy number states.
It adds:

- 3-parameter (gain, loss, and conversion) substitution model for SCAs
- Ascertainment bias correction for this data type
- Tree model with last universal normal cell
- Adapts strict and relaxed clock models to this framework

## [BEAGLE](https://beast.community/beagle){:target="_blank"}
High performance computing library that BEAST can use to speed up some calculations (parallelization using multiple CPU and/or GPU cores among others). **PISCA IS NOT COMPATIBLE WITH BEAGLE YET.** I have set up BEAST/PISCA in the VM so that by default BEAGLE is desactivated. Please, do not activate it, and **make sure it is not active if you are running BEAST/PISCA in your own system**.

## [TreeAnnotator](https://beast.community/treeannotator){:target="_blank"}
BEAST component that summarizes a posterior samples of trees in a single estimate. Usually, this is the maximum clade credibility tree topology (i.e., tree that maximizes the product of the posterior probability of the clades it contains) with common ancestor height branch lengths (i.e., the mean of the MRCA of all pairs of nodes in that clade). However, other options are available. 

## [FigTree](http://tree.bio.ed.ac.uk/software/figtree/){:target="_blank"}
Program to inspect and format phylogenetic trees. 

**WARNING:** Do not open a .trees file directly using FigTree. This files contains the whole posterior sample (i.e., thousands to millions of trees) and may freeze your computer. Make sure to only open with it a small number of trees (e.g., single estimates generated using TreeAnnotator).

## [Tracer](http://beast.community/tracer){:target="_blank"}
Program to analyze the posterior sample of MCMC data. Among other features, it is used to [obtain posterior point estimates of parameters](https://beast.community/analysing_beast_output) and [check proper convergence](https://beast.community/tracer_convergence) and mixing.

## [RWTY](https://cran.r-project.org/web/packages/rwty/vignettes/rwty.html){:target="_blank"}
More comprehensive R package to diagnose convergence and mixing of MCMC chains, including intra and inter-chain diagnostics.

# Exercise 1 (cont).


<!---
## Installation

### Download Java
Java is a cross-platform general-purpose programming language. Java applications are usually distributed compiled in bytecode that runs on a Java virtual machine in any system. BEAST, and therefore PISCA, are programmed in Java. **You need to java a Java SE Runtime Environment >=1.6 to follow this tutorial**. You can check the version of your Java SE runtime environment executing `java -version` in the command line of your computer (i.e., Windows: cmd or PowerShell, Mac/Linux: terminal):

<pre><code>\> java -version
java version "1.8.0_60"
Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)</code></pre>



###### Linux
<pre><code>sudo apt install openjdk-8-jre-headless</code></pre>
###### Windows/MAC
Download the Java JRE from `https://www.oracle.com/technetwork/java/javase/downloads/index.html` and install following the normal procedure of your system.

### R?
###### Linux
<pre><code>sudo apt-get install libopenblas-base r-base</code></pre>
###### Windows

###### Mac

### Download BEAST 1.8.4
You can find the [official BEAST 1.8.4 release on github](https://github.com/beast-dev/beast-mcmc/releases/tag/v1.8.4){:target="_blank"}. Make sure to download the version that is appropriate for your Operative system (Mac: .dmg, Linux: .tgz, Windows: .zip).

###### Linux
`tar xvzf BEASTv1.8.4.tgz`

 
### Download PISCA
You can find the [official PISCA 1.0.2 release on github] (https://github.com/adamallo/PISCA/releases/tag/v1.0.2){:target="_blank"}. Download the .tgz file.

###### Linux
`tar xvzf PISCAv1.0.2.tgz`
--->
