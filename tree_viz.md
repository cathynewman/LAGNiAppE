# Tree Visualization

Here, we will explore some different ways to visualize and edit phylogenetic trees using **FigTree** and **RStudio**.

## Getting started

### Installing R packages

A partial list of the R packages that contain phylogeny-related functionality is available on the website [CRAN phylogenetics task view](https://cran.r-project.org/web/views/Phylogenetics.html).

We will use **phangorn** (Schliep 2011) and **phytools** (Revell 2012).

In RStudio, you can install packages in two ways. The first is by typing the R function `install.packages( )` directly into the command line, with the package name inside the parentheses. Do each package one at a time.

```r
install.packages("phangorn")
install.packages("phytools")
```

The point-and-click way in RStudio is to move over to the lower-right window pane, click the "Packages" tab, and click "Install." Then type in the package name and install it (along with **dependencies**, which are all the other packages required by the one you're installing).

**Load the libraries:** To actually use these installed packages (or libraries) during an R session, you need to **load** them first. Here again, in RStudio you can do it the regular way or the point-and-click way.

```r
library(phangorn)
library(phytools)
```

Or go back to the "Packages" tab in RStudio and click the checkbox next to each package you want to load.

### Working directory

In order to read and write files into/from R, you need to be able to identify your **current working directory**, which is the directory on your hard drive where R is currently working out of.

To see your current working directory, use `getwd()`

In RStudio, you can go to the lower-right window pane and click the "Files" tab, then navigate to the directory where your class files are saved. A shortcut to set your R working directory to that directory is to navigate there first in this Files pane, then go to the "Session" menu at the top of your screen -> "Set working directory" -> *To Files pane location*

So make sure your working dir is the location of the files you'll be working with.

## Trees in R

I think R will also install the package **ape** as a dependency of either phangorn or phytools, but if ape doesn't appear in your list of installed packages now, install it as well and load it.

The most important core package for phylogenies in R is called **ape**, which stands for **A**nalysis of **P**hylogenetics and **E**volution in R.

### Loading a phylogenetic tree

Let's read in a phylogenetic tree of vertebrates from a text string in Newick format. We can do this in a couple of different ways: specify the text string directly or read the tree from a tree file.

Our example text string here will only specify the tree topology, no branch lengths or support values.

```r
## create string with tree text
text.string <- "(((((((cow, pig),whale),(bat,(lemur,human))),(robin,iguana)),coelacanth),goldfish),shark);"

## read tree from string
vert.tree <- read.tree(text = text.string)
```

### Trees in phytools

When we read any phylogeny from file or from a text string (as we did in this case), we create an object in memory. Normally, it won’t be necessary to interact directly with this object’s internal structure. Instead, we usually pass the object unchanged to other functions, just as we did when we plotted our phylogeny in different styles above.

Let's plot our tree using the phytools function `plotTree()`

```r
plotTree(vert.tree)
```

<img src="https://moodle.ulm.edu/draftfile.php/2243332/user/draft/540159805/1_tree-square.png" alt="square tree" width="538" height="355" class="img-responsive atto_image_button_text-bottom"><br>

Let's clean up the tree a little and make it triangle shaped:

```r
plotTree(vert.tree, type = "cladogram", nodes = "centered")
```

<img src="https://moodle.ulm.edu/draftfile.php/2243332/user/draft/540159805/1_cladogram.png" alt="triangle cladogram" width="538" height="355" class="img-responsive atto_image_button_text-bottom"><br>

Let's break this down: First we specified the plotting style with `type = "cladogram"` (slanted cladogram, i.e. a tree where branch lengths are meaningless). Then we cleaned up the image a bit by centering the nodes with `nodes = "centered"`.

Now add a title to the plot:

```r
title(main = "Practice tree")
```

**Saving figures:** In RStudio, you can save these plots, trees, and other figures by clicking "Export" in the Plots window pane. The file will save to your current working directory.

### Tip labels of a tree
One important component of our `phylo` object is the vector `tip.label`:

```r
vert.tree$tip.label
```

```
##  [1] "cow"        "pig"        "whale"      "bat"        "lemur"
##  [6] "human"      "robin"      "iguana"     "coelacanth" "goldfish"
## [11] "shark"
```

By convention the *order* of the tip labels in `tip.label` corresponds to the numerical order of the tip indices (scored from 1 through *N*) in our tree.
