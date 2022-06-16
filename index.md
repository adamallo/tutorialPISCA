# Tutorial PISCA

## READ FIRST
You can follow this tutorial using [this package with input, intermediate, and result files](https://drive.google.com/file/d/1-ETx6gnISUczkS06xRL7d9DnagIYol8k/view?usp=sharing) or install BEAST1.8.X and PISCA and run the exercises in your system. I recommend the first option for most people since you will obtain the same learning experience without the hassle of having to install software in your computer.
 
### Hyperlinks
I have included a lot of supplementary information and reading materials linked in the text. Please, feel free to wander away, either during the interactive learning session (making sure you are able to finish the assignment) or definitely afterwards. 
Other resources of interest:
- [BEAST2 book](https://www.beast2.org/book/){:target="_blank"}
- [Taming the BEAST tutorials](https://taming-the-beast.org/tutorials/)

### Collapsed boxes
You will see collapsed boxes along the tutorial document. Please, try to answer the questions on your own before clicking on them. Otherwise, this tutorial will be of little use for you!

<details><summary>Understood?</summary>
<p>
YES!
</p>
</details>

## Exercise 1: Using PISCA to reconstruct a somatic phylogeny
### General description
We will be analyzing a rather small toy dataset. Part of the activity will be for you to get a better understanding of its characteristics by inspecting the input data. For now, it is enough to know that we are analyzing allele-specific copy number states from a number of homogeneous samples taken at two time points (dated in years from the date of birth of the patient).

In this activity you will learn:

- How to run PISCA
- Determine if the Markov Chain Monte Carlo (MCMC) chain converged and mixed properly
- Tweak the MCMC chain
- Identify if your results are driven by your priors
- Summarize the results
- Obtain a visual representation of the resulting evolutionary hypothesis (tree and parameters)

### Running PISCA using a given XML input file ***(optional)***
- Input file: `strict\_test\_short.xml`
- Running command: `beast --beagle-off --seed 1020 strict\_test\_short.xml
- This analysis will take from 1 to 5 minutes.

### Inspect the information that BEAST logs in the console
- For this, open the file `strict_test_short.log`

<details><summary>Could you tell how many MCMC iteractions (states in the Markov Chain) you are running the algorithm for?</summary>
<p>
The first column indicates the step sampled in that row
</p>
</details>

<details><summary>Why is the posterior probability not growing monotonically?</summary>
<p>
Remember, MCMC is a Markov chain that samples the posterior probability distribution (remember the robot). It samples more often the regions of the distribution with higher density, instead of maximizing any value. 
</p>
</details>

### Running a second XML input file ***(optional)***
- Before going forward, repeat the same procedure with the `strict\_test.xml` file

### Get familiarized with the software we will be using to analyze our results

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
BEAST generates two main output files:
- **Parameter sample (.log)**. Posterior sample of the model parameter values
- **Tree sample (.trees)**. Posterior sample of the trees (topologies, branch lengths, and some other branch-specific parameters).

Depending on the configuration of the XML you may find additional output files, for example
- **Operator summary (.ops)**. Summary of the activity of the different operators along the MCMC chain.

***NOTE***: There are currently two main implementations of BEAST, [BEAST1.X](http://beast.community/index.html) and [BEAST2.X](http://www.beast2.org/). They have a pretty recent common ancestor but are currently evolving independently. In this tutorial we are using BEAST1.8 

## BEAUti
Graphical user interface distributed with BEAST that makes it easier to generate XML files for the most common scenarios. **Unfortunately, PISCA is not yet compatible with BEAUti**. For this reason, along most of this tutorial you will be working with pre-generated XML files, although you may have to edit small sections of it. PISCA is distributed with scripts that help users generate XML files for the most common PISCA analyses.

## [PISCA](https://github.com/adamallo/PISCA){:target="_blank"}
BEAST1.8 plugin that expands BEAST with models to reconstruct the evolution of homogeneous somatic samples using allele-specific integer copy number states.
It adds:

- 3-parameter (gain, loss, and conversion) substitution model for allele-specific SCAs
- 2-parameter (gain, loss) substitution model for absolute SCAs
- 1-parameter (relative demethylation rate) substitution model for bi-allelic binary data (methylation)
- Ascertainment bias correction for these data types
- Tree model with last universal normal cell
- Adapts strict and relaxed clock models to this framework

## [BEAGLE](https://beast.community/beagle){:target="_blank"}
High performance computing library that BEAST can use to speed up some calculations (parallelization using multiple CPU and/or GPU cores among others). **PISCA IS NOT COMPATIBLE WITH BEAGLE YET.**.

## [TreeAnnotator](https://beast.community/treeannotator){:target="_blank"}
BEAST component that summarizes a posterior samples of trees in a single estimate. Usually, this is the maximum clade credibility tree topology (i.e., tree that maximizes the product of the posterior probability of the clades it contains) with common ancestor height branch lengths (i.e., the mean of the MRCA of all pairs of nodes in that clade). However, other options are available. 

## [FigTree](http://tree.bio.ed.ac.uk/software/figtree/){:target="_blank"}
Program to inspect and format phylogenetic trees. Its web is down since recently. 

**WARNING:** Do not open a .trees file directly using FigTree. These files contain the whole posterior sample (i.e., thousands to millions of trees) and may freeze your computer. Make sure to only open with it a small number of trees (e.g., single estimates generated using TreeAnnotator).

## [Tracer](http://beast.community/tracer){:target="_blank"}
Program to analyze the posterior sample of MCMC data. Among other features, it is used to [obtain posterior point estimates of parameters](https://beast.community/analysing_beast_output) and [check proper convergence](https://beast.community/tracer_convergence) and mixing.

## [RWTY](https://cran.r-project.org/web/packages/rwty/vignettes/rwty.html){:target="_blank"}
More comprehensive R package to diagnose convergence and mixing of MCMC chains, including intra and inter-chain diagnostics of trees.

# Exercise 1 (cont.)

### Analyzing the MCMC chain of the *strict\_test\_short.xml* run
- Open the program *Tracer*
- Find the file with the posterior sample of the evolutionary parameters of this run `strict_test_short.log`
<details><summary>Posterior sample??</summary>
<p>
The values that the MCMC took in each sampled state for evolutionary parameters other than the phylogenetic tree.
</p>
</details>
- Inspect the file with a text editor to understand what you are working with (a 1 minute glance)
- Open it on Tracer (you can just drag and drop)
<details><summary>Do you think we have sampled the posterior probability properly?</summary>
<p>
NO. ESSs are very low. Remember, we would like to have >200 ESS in all parameters of interest, and any parameter with very low ESS indicate a convergence (or mixing) problem
</p>
</details>
<details><summary>Do all parameters mix equally?</summary>
<p>
NO. You will see that some parameters are mixing properly (fuzzy caterpillar), while others show mixing problems.
</p>
</details>
<details><summary>What mixing problems can you identify?</summary>
<p>
The chain gets stuck estimating substitution rates. You will also see directionality in some parameters (probably in the posterior).
</p>
</details>
<details><summary>Can you think of ways to solving these problems?</summary>
<p>
- The mixing of stuck parameters can be usually improved by decreasing the size of the step (editing the operator that proposes new values for this parameter).
- The mixing of parameters that show trends can be usually improved by increasing the size of the step.
- A lot of mixing problems can be solved by increasing the sampling frequency. In this case, this run had an extremely low sampling frequency (20) just to force this issue on purpose. However, generally speaking, we cannot increase the sampling frequency forever; this is an equivalent to *brute force*.
</p>
</details>

### Let's now consider the results of  `strict\_test.xml`
<details><summary>Take a careful look at the last table printed on the console when the analysis finishes. Do you remember what operators are?</summary>
<p>
The operators are the functions used in the MCMC to propose new states. The operator name follows a simple scheme function(parameter).
</p>
</details>
<details><summary>What do you think the column count may mean? Why do you think that not all operators have the same count? Do you think that tunning this may improve your MCMC chain?</summary>
<p>
Some parameters are easier to estimate than others, and some are more interesting than others. Tweaking the relative weight of the different operators in the xml may help you improve the sampling efficiency of your MCMC.
</p>
</details>
- Now, drag and drop the new posterior sample `strict\_test.log` into tracer. In the top panel, you will be able to see the two files (samples). You can select a specific parameter, and switch between one and another file. More importantly, you can also select both files (press ctrl when selecting the second), and compare the two posterior distributions
- Make sure to compare most/all parameters in the different views (estimates, marginal density, traits...)
- Now, select two parameters at the same time to see their joint-marginal.
- It is time to compare it with another posterior sample I previously generated. You can find it at `strict_test1.log`
<details><summary>Do you think the MCMC converged?</summary>
<p>
These MCMC chains are fairly short due to time constraints. However, if you compare the two long chains you'd probably find that they seem to have converged. Remember, this is a toy dataset and therefore has less noise than real data.
</p>
</details>
- Finally, I encourage you to dive into the .xml file. Can you locate the data? Models? Operators? MCMC options? At the end of the day, when you do real Bayesian inference you always end up having to modify the .xml directly, you'd better get used to it!

### Sampling from the prior
As we talked earlier, the posterior probability (our tree score) is proportional to the product of the phylogenetic likelihood and the prior probability of the model. If the data has very low phylogenetic signal and the priors are very informative, you could get results dominated by the prior (i.e., your data is not giving you additional information).

<details><summary>How would you sample from the prior?</summary>
<p>
Running the same exact analysis without data. In order to do so, we can substitute all characters by gaps (-) or missing data (?).
</p>
</details>

- Add the posterior sample `strict_prior.log` into Tracer. As the name indicates, I run the MCMC only sampling from the prior.
- Compare the posterior distributions of the two long chains with the one only sampling the prior.

<details><summary>Do you think the MCMC was driven strongly by the prior?</summary>
<p>
Not really, even in this very small dataset. However, you can see how the data is informing certain parameters more strongly than others.
</p>
</details>

### Summarizing the results
It is now time to summarize our posterior sample of trees in one plot. There are several criteria on how to choose the best tree. [Read about it here.](https://www.beast2.org/summarizing-posterior-trees/)

You can find the results of this step in `preruntrees/strict_test_sumtree.tree`

- Load the posterior sample of trees `strict_test.trees` into TreeAnnotator.
- Select the number of trees you want to discard, **burnin**
<details><summary>What number do I use?</summary>
<p>
It is up to you. 10% of the samples is the standard, but really it is up to you. Make sure to keep a good sample size!
</p>
</details>
- Indicate where you want to save the resulting tree, and run the program.

### Exploring the tree
- Load (you can drag and drop) the tree you have just reconstructed (`preruntrees/strict_test_sumtree.tree`) in FigTree.
- Make sure to add the posterior probability of the nodes as node labels.
- You can play with the different display options, useful for publication!

## Exercise 2 (Optional)
I included the posterior samples (3 chains) of the Barrett's esophagus study we published using PISCA. You can find them at `traces_BE`.
Replicate what we did up to now using this dataset.

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
