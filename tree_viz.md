# Tree Visualization

Here, we will explore some different ways to visualize and edit phylogenetic trees using **FigTree** and **RStudio**.

## Getting started

### Installing R packages

A partial list of the R packages that contain phylogeny-related functionality is available on the website [CRAN phylogenetics task view](https://cran.r-project.org/web/views/Phylogenetics.html).

We will use **ape**, which stands for **A**nalysis of **P**hylogenetics and **E**volution in R.

In RStudio, you can install packages in two ways. The first is by typing the R function `install.packages( )` directly into the command line, with the package name inside the parentheses.

```r
install.packages("ape")
```

The point-and-click way in RStudio is to move over to the lower-right window pane, click the "Packages" tab, and click "Install." Then type in the package name and install it (along with **dependencies**, which are all the other packages required by the one you're installing).

**Load the libraries:** To actually use these installed packages (or libraries) during an R session, you need to **load** them first. Here again, in RStudio you can do it the regular way or the point-and-click way.

```r
library(ape)
```

Or go back to the "Packages" tab in RStudio and click the checkbox next to each package you want to load.

### Working directory

In order to read and write files into/from R, you need to be able to identify your **current working directory**, which is the directory on your hard drive where R is currently working out of.

To see your current working directory, use `getwd()`

In RStudio, you can go to the lower-right window pane and click the "Files" tab, then navigate to the directory where your class files are saved. A shortcut to set your R working directory to that directory is to navigate there first in this Files pane, then go to the "Session" menu at the top of your screen -> "Set working directory" -> *To Files pane location*

So make sure your working dir is the location of the files you'll be working with.

## Trees in R

### Load a phylogenetic tree

Let's read in a phylogenetic tree of vertebrates from a text string in Newick format. We can do this in a couple of different ways: specify the text string directly or read the tree from a tree file.

Our vertebrate tree example text string here will only specify the tree topology, no branch lengths or support values.

We can also read in a tree from file, such as one of our primate gene trees.

```r
# Create string with tree text
text.string <- "(((((((cow, pig),whale),(bat,(lemur,human))),(robin,iguana)),coelacanth),goldfish),shark);"

# Read tree from text string
vert.tree <- read.tree(text = text.string)

# Read a tree from file
Acsm1.tree <- read.tree("Acsm1.tree")
```

### Plot the tree

Let's first plot the basic vertebrate tree:

```r
plot(vert.tree)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/1_vert_tree.jpg" width="50%">

We can make it triangle shaped by specifying `type = "cladogram"`. (The default type is "phylogram")

```r
plot(vert.tree, type = "cladogram")
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/2_vert_tree_clado.jpg" width="50%">

```r
title(main = "Vertebrate tree")
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

### Rooting a tree

Let's look at the primate gene tree now.

```r
plot(Acsm1.tree)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/3_Acsm1_tree.jpg" width="50%">

We can see that this tree is unrooted.

```r
Acsm1.tree
```

```
## Phylogenetic tree with 5 tips and 3 internal nodes.
##
## Tip labels:
##   Rhesus, Orangutan, Chimp, Gorilla, Human
## Node labels:
##   , 98, 46
##
## Unrooted; includes branch length(s).
```

We can root the tree with the outgroup, Rhesus:

```
# Root tree
Acsm1.tree.rooted <- root(Acsm1.tree, outgroup = "Rhesus", resolve.root = T)

# Plot rooted tree
plot(Acsm1.tree.rooted)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/4_Acsm1_tree_rooted.jpg" width="50%">

It's hard to see all the relationships here because the branch near the root of the tree was given a length of 0. For our purposes here, we don't really care about branch lengths anyway (we are only interested in tree topology), so let's add an option to ignore the branch lengths in the plot by setting `use.edge.length` to `FALSE`:

```
plot(Acsm1.tree.rooted, use.edge.length = F)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/5_Acsm1_tree_rooted_noBL.jpg" width="50%">

### Rotating nodes

This is a personal preference, but I like the outgroup to be at the bottom of the tree. We can rotate specific nodes on the tree by specifying the node number. First we need to see how the nodes are numbered, so let's add the node number annotation to our tree:

```
nodelabels()
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/6_Acsm1_nodelabels.jpg" width="50%">

Rotating node number 6 will flip the outgroup, Rhesus, down to the bottom:

```
# Rotate tree
Acsm1.tree.rooted.rotated <- rotate(Acsm1.tree.rooted, node = 6)

# Plot rotated tree
plot(Acsm1.tree.rooted.rotated, use.edge.length = F)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/7_Acsm1_rotated.jpg" width="50%">

(You could then also rotate node 7 to flip human down to make a more visually appealing and "balanced" tree, if you want to. I won't show it here.)

### Adding support values

Let's look at the primate species tree from ASTRAL now. This is a rooted tree that includes branch lengths and support values (for internal nodes; see below). The support values are in the object element "Node labels."

```
speciestree <- read.tree("speciestree.tre")
speciestree
```

```
## Phylogenetic tree with 6 tips and 5 internal nodes.
##
## Tip labels:
##   Bonobo, Chimp, Human, Gorilla, Orangutan, Rhesus
## Node labels:
##   , , 0.996591, 0.832483, 0.970552
##
## Rooted; includes branch length(s).
```

Plot the tree:

```
plot(speciestree)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/8_speciestree.jpg" width="50%">

The bootstrap support values are stored in the element "Node labels." To see what this element is actually named, use `names()` to show the names of all elements in the phylo object:

```
names(speciestree)
```

```
## [1] "edge"  "edge.length"  "Nnode"  "node.label"  "tip.label"
```

Now, before we plot the node labels, let's see exactly what they are. In R, you can call a specific element of an object with the dollar sign `$`

```
speciestree$node.label
```

```
[1] ""         ""         "0.996591" "0.832483" "0.970552"
```

First, you can see in this example that two of the nodes have an empty node label, denoted as `""`. This is because ASTRAL leaves the branch length and support value of terminal branches empty. This is a problem for some tree-viewing packages (such as `phytools`), which is the main reason why I'm just showing you some basic tree-viz tools in `ape` instead, which can handle these NA values. Discussion in the [weighted ASTRAL documentation](https://github.com/chaoszhang/ASTER/blob/master/tutorial/wastral.md).

Second, we don't need all those decimal places in the numbers. If you want to round the numbers, the quickest way is to just re-set those support values manually. We can't use the `round()` function directly because the node labels are stored in the object as text strings, not numbers.

```
speciestree$node.label[3] <- "1.0"
speciestree$node.label[4] <- "0.83"
speciestree$node.label[5] <- "0.97"
```

Now let's plot the tree and add node labels:

```
plot(speciestree)
nodelabels(text = speciestree$node.label)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/9_speciestree_support.jpg" width="50%">

There are other options you can add to the `nodelabels()` call to customize the labels, such as changing the background color with `bg = "color"` or the text color with `col = "color"`. See all the options with `help("nodelabels")`.
