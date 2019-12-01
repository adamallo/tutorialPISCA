#Tutorial PISCA

##IMPORTANT: How to follow this tutorial, Virtual Machine or Native installation?
You can follow this tutorial using your own system or a virtual machine image I will provide for the ACE Bootcamp. A virtual machine is as emulation of a computer system, sort of a simulated computer within your computer. If you use the virtual machine image provided (with a Linux distribution, specifically, Ubuntu 18.04.3 LTS), you will not have to install any software in your computer, apart from a virtualization product (e.g., [VirtualBox](https://www.virtualbox.org/wiki/Downloads)) and all software needed to follow the tutorial will be included in the machine. The execution of PISCA will be slower, but you will be able to follow the tutorial. **I recommend using the virtual machine to users that do not plan to use PISCA or BEAST in the near future, and do not plan to analyze data of their own in the afternoon session**. Otherwise, I think it is worth installing the software in your own machine for speed.

**License disclaimer: I will provided the virtual machine for its usage during the ACE Bootcamp only. Do not distribute it.**

##Background on software used in this tutorial
##BEAST

##PISCA

##Tracer

https://github.com/beast-dev/tracer/releases/tag/v1.7.1


##FigTree

##RWTY
install.packages("rwty",dependencies=TRUE)
##Installation

###Download Java
Java is a cross-platform general-purpose programming language. Java applications are usually distributed compiled in bytecode that runs on a Java virtual machine in any system. BEAST, and therefore PISCA, are programmed in Java. **You need to java a Java SE Runtime Environment >=1.6 to follow this tutorial**. You can check the version of your Java SE runtime environment executing `java -version` in the command line of your computer (i.e., Windows: cmd or PowerShell, Mac/Linux: terminal):

<pre><code>\> java -version
java version "1.8.0_60"
Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)</pre></code>



######Linux
<pre><code>sudo apt install openjdk-8-jre-headless</pre></code>
######Windows/MAC
Download the Java JRE from `https://www.oracle.com/technetwork/java/javase/downloads/index.html` and install following the normal procedure of your system.

###R?
######Linux
<pre><code>sudo apt-get install libopenblas-base r-base</pre></code>
######Windows

######Mac

###Download BEAST 1.8.4
You can find the [official BEAST 1.8.4 release on github](https://github.com/beast-dev/beast-mcmc/releases/tag/v1.8.4). Make sure to download the version that is appropriate for your Operative system (Mac: .dmg, Linux: .tgz, Windows: .zip).

######Linux
`tar xvzf BEASTv1.8.4.tgz`

 
###Download PISCA
You can find the [official PISCA 1.0.2 release on github] (https://github.com/adamallo/PISCA/releases/tag/v1.0.2). Download the .tgz file.

######Linux
`tar xvzf PISCAv1.0.2.tgz`

