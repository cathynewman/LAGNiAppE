# Tree Visualization in R

Here, we will explore some ways to visualize and edit phylogenetic trees using **RStudio**.

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

We can also read in a tree from file, such as one of our emydid turtle gene trees.

```r
# Create string with tree text
text.string <- "(((((((cow, pig),whale),(bat,(lemur,human))),(robin,iguana)),coelacanth),goldfish),shark);"

# Read tree from text string
vert.tree <- read.tree(text = text.string)

# Read a tree from file
turtles.tree <- read.tree("Emydidae_Mitochondrial.treefile")
```

### Plot the tree

Let's first plot the basic vertebrate tree. Calling `plot()` with a phylo object actually calls the ape function `plot.phylo()`. So to see all the options for this function (styles, color, etc.), use `help("plot.phylo")`. But here we can just use `plot()` for short:

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

Let's look at an emydid turtle gene tree now. This is the mitochondrial gene tree that I generated in IQ-TREE.

Because this tree has more tips, I'll make the text size smaller with `cex = 0.5` and make the tree fill the plot window by eliminating the margins with `no.margin = T`.

```r
plot(turtles.tree, cex = 0.5, no.margin = T)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/3_turtle_tree.jpg" width="75%">

We can see that this tree is unrooted.

```r
turtles.tree
```

```
## Phylogenetic tree with 42 tips and 40 internal nodes.
##
## Tip labels:
##   Platysternon_megacephalum, Terrapene_ornata_luteola_1, Terrapene_ornata_luteola_2, Terrapene_carolina_triunguis_1, Terrapene_carolina_triunguis_2, Terrapene_carolina_1, ...
## Node labels:
##   , 73, 62, 73, 100, 100, ...
##
## Unrooted; includes branch length(s).
```

We can root the tree with the outgroup, Platysternon_megacephalum.

It's hard to see all the relationships here because the branch near the root of the tree was given a length of 0. For our purposes here, we don't really care about branch lengths anyway (we are only interested in tree topology), so let's also add an option to ignore the branch lengths in the plot by setting `use.edge.length` to `FALSE`:

```
# Root tree
turtles.rooted <- root(turtles.tree, outgroup = "Platysternon_megacephalum", resolve.root = T)

# Plot rooted tree
plot(turtles.rooted, use.edge.length = F, cex = 0.5, no.margin = T)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/4_turtle_tree_rooted_noBL.jpg" width="75%">

### Rotating nodes

This is a personal preference, but I like the outgroup to be at the bottom of the tree. We can rotate specific nodes on the tree by specifying the node number. First we need to see how the nodes are numbered, so let's add the node number annotation to our tree:

```
nodelabels()
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/5_turtle_nodelabels.jpg" width="75%">

Rotating node number 43 will flip the outgroup, Platysternon_megacephalum, down to the bottom:

```
# Rotate tree
turtles.rooted.rotated <- rotate(turtles.rooted, node = 43)

# Plot rotated tree
plot(turtles.rooted.rotated, use.edge.length = F, cex = 0.5, no.margin = T)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/6_turtle_rotated.jpg" width="75%">

### Adding support values

The bootstrap support values are stored in the element "Node labels." To see what this element is actually named, use `names()` to show the names of all elements in the phylo object:

```
names(turtles.rooted.rotated)
```

```
## [1] "edge"  "edge.length"  "Nnode"  "node.label"  "tip.label"
```

Now, before we plot the node labels, let's see exactly what they are. In R, you can call a specific element of an object with the dollar sign `$`

```
turtles.rooted.rotated$node.label
```

```
##  [1] "Root" "73"   "62"   "73"   "100"  "100"  "78"   "100"  "98"   "88"   "100"  "100"  "100"
## [14] "100"  "67"   "98"   "92"   "100"  "100"  "78"   "100"  "100"  "100"  "100"  "100"  "61"  
## [27] "60"   "100"  "100"  "100"  "73"   "56"   "69"   "100"  "100"  "100"  "100"  "100"  "99"  
## [40] "100"  ""
```

First, notice that rerooting the tree gave the root node the name `Root`. If you don't want this label to appear on your tree, you can simply rename the root to an empty value. It's the first node label in the list, so select it with `[1]`:

```
turtles.rooted.rotated$node.label[1] <- ""
```

You can also see in this example that the last node in the list (the node for the ingroup) has an empty node label, denoted as `""`. This isn't a problem for this example with an IQ-TREE tree, but ASTRAL in particular leaves the branch length and support value of terminal branches empty. This is a problem for some tree-viewing packages (such as `phytools`), which is the main reason why I'm just showing you some basic tree-viz tools in `ape` instead, which can handle these NA values. Discussion in the [weighted ASTRAL documentation](https://github.com/chaoszhang/ASTER/blob/master/tutorial/wastral.md).

Now let's plot the tree and add node labels. I'll also adjust down the font size for the node labels:

```
plot(speciestree)
nodelabels(text = turtles.rooted.rotated$node.label, cex = 0.5)
```

<img src="https://github.com/cathynewman/LAGNiAppE/blob/main/images/7_turtle_bootstrap.jpg" width="75%">

There are other options you can add to the `nodelabels()` call to customize the labels, such as changing the background color with `bg = "color"` or the text color with `col = "color"`. See all the options with `help("nodelabels")`.
