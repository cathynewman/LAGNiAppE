# Tutorial: Gene Trees & Species Trees

LAGNiAppE @ LSU 2025
Cathy Newman (UAB)

**These instructions are for MacOS.** This tutorial and the necessary files are posted on GitHub here: [https://github.com/cathynewman/LAGNiAppE/](https://github.com/cathynewman/LAGNiAppE/)

This tutorial will walk you through how to generate individual gene trees for emydid turtles, compare the gene tree topologies for your gene(s) and the genes your fellow scholars analyzed, and use those gene trees as input to generate a species tree.

Objectives:
* Practice the steps of aligning sequences and generating gene trees and species trees.
* Explore the phylogenetic relationships of turtles in the family Emydidae.
* Understand drivers of gene tree discordance and how species trees differ from gene trees.
* Use FigTree to visualize and edit trees.

> [!NOTE]
> Tree files in Newick format are all plain-text files. It doesn't really matter what the file extension is. Files with extensions `*.newick`, `*.tre`, `*.tree`, `*.treefile`, etc., are all equivalent. Some software tools expect specific filename extensions, but I don't think it actually matters here.

### The dataset: Emydid turtles

We will be using sequence data from Spinks PQ, Thomson RC, McCartney-Melstad E, and Shaffer HB (2016) Phylogeny and temporal diversification of the New World pond turtles (Emydidae). *Molecular Phylogenetics and Evolution*, 103: 85-97. [https://doi.org/10.1016/j.ympev.2016.07.007](https://doi.org/10.1016/j.ympev.2016.07.007)

This dataset includes 41 turtles in the family Emydidae and 1 outgroup sample (the big-headed turtle, *Platysternon megacephalum*). The authors sequenced 30 nuclear genes and 4 mitochondrial genes.

For this tutorial, each group will generate a gene tree for (a) the concatenated mitochondrial genes ("mitochondrial gene tree") and (b) a couple of nuclear genes.

## PART 1: Sequence alignment – AliView

In this section, we will download `FASTA` files of DNA sequences for given genes and generate multisequence alignments in AliView, which uses the MUSCLE alignment algorithm.

STEPS:
1. Download from GitHub `/data` the `FASTA` files for the subset of turtle genes we are using ([1_turtle_sequences.zip](data/1_turtle_sequences.zip)).
2. Open AliView and load the list of sequences (FASTA file) for your gene:  File > *Open File*
3. Align the sequences:  Align > *Realign everything*
4. If alignment ends are uneven (sequences aren't all the same length), AliView automatically fills the spaces with dashes (-). Technically, dashes indicate "real" alignment gaps, but in this case the spaces are simply missing data, not necessarily gaps. So replace gaps (-) with missing data (?) at the ends:  Edit > *Replace terminal gaps into missing char (?)*
5. Scroll through the alignment to make sure it meets the "eye test" (looks good to you).
6. Save alignment as **phylip file (full names & padded)**

## PART 2: Gene trees – IQ-TREE

**Resources:** Here is the [IQ-TREE documentation](http://www.iqtree.org/doc/)

In this section, we will reconstruct a phylogenetic tree for the gene alignment from the previous step. We will run 1,000 "ultrafast" bootstrap pseudoreplicates so that the gene tree has nodal support values to use, along with branch lengths, in the estimation of the species tree in the next section.

To reduce the chance that the tree search will get stuck in a poor local optimum, we will increase the required number of unsuccessful hill-climbing iterations before stopping to 500 from the default of 100.

STEPS:
1. Optional: copy your sequence alignment files into the `IQ-TREE bin/` directory. This makes it easier to specify the file path in step 3 below. Alternatively, just use `/path/to/alignmentfile.phy`
2. In your terminal app, navigate into IQ-TREE bin directory:

```
cd iqtree-3.0.0-macOS/bin
```

3. Run IQ-TREE with the command below, editing to your alignment filename (`-s`). Here, we specify an outgroup (`-o`) as Platysternon_megacephalum, and we will run 1,000 ultrafast bootstrap pseudoreplicates (`-B`). Be sure to use capital `B` for the bootstrap flag, as lowercase `b` will call standard bootstraps, which will take much longer to run.

```
./iqtree3 -s alignmentfile.phy -o "Platysternon_megacephalum" -nstop 500 -B 1000
```

4. Open the output tree file (*.treefile) in FigTree to look at it! See below for some FigTree help and suggestions.
5. Upload your tree file (just the `*.treefile` file) to the Gene Trees folder on Google Drive ([link here](https://drive.google.com/drive/folders/1E0Lfo5crQHkIM26PVa0lGHMKFbczsWfR?usp=sharing))
6. What do your gene trees show as the phylogenetic relationships among the emydid genera? Compare your trees to other groups. What do you think might be causing any observed differences?

## FigTree tips

**Rooting:** Gene trees from IQ-TREE are unrooted trees. You can root with the outgroup in FigTree by clicking on the outgroup name or its branch to highlight it, then click `Reroot` on the top menu.

**Bootstrap values:** When your tree file includes bootstrap or other support values, FigTree will ask upon opening the file what you want to name that set of values (default is "label"). Then, you can add those values to the tree: go to the left sidebar menu, check the box for `Node labels`, and in `Display`, choose whatever you named the bootstrap values. You can change font size, color, etc.

**Branch lengths:** Sometimes trees with branch lengths can be hard to assess in FigTree, especially when IQ-TREE gives a lot of branches lengths of 0. This can make it hard to see the different clades and their interrelationships. In this workshop, we are mainly interested in tree *topology* (phylogenetic relationships) and not so much branch lengths (evolutionary time or extent of divergence), so it's fine to view your trees with branch lengths ignored.

In the left sidebar menu in FigTree, click on `Tree` and check the box next to `Transform branches`. Then choose any one of the options (I prefer "proportional"). Here is an example, with an original tree from IQ-TREE on the left, and the same tree set to proportional branches on the right:

![FigTree trees: Left = with branch lengths, Right = proportional](images/FigTree_tips1.jpg)

**Rotating nodes:** You can also rotate nodes in FigTree. Remember, this doesn't change the tree topology, and it's different from rerooting the tree. You're simply flipping clades around in space visually without changing anything about the phylogenetic relationships depicted in the tree. For example, take the above tree on the right. It really annoys me personally that the outgroup branch is at the bottom while the *Glyptemys* clade is at the top. It looks "wrong" visually to me (this is just a personal preference). So below I show how to rotate the branch that will flip the *Glyptemys* clade down to the bottom. We are flipping the *Glyptemys* clade and the big clade that contains all other ingroup species, so on the left I've highlighted the branch that leads to the MRCA of those two clades:

![How to rotate nodes in FigTree](images/FigTree_tips2.jpg)

## PART 3: Species trees – weighted ASTRAL

**Resources:** Here are the GitHub sites for [ASTER](https://github.com/chaoszhang/ASTER), and specifically [Weighted ASTRAL](https://github.com/chaoszhang/ASTER/blob/master/tutorial/wastral.md)

Now we will take all of the individual gene trees you and your fellow scholars generated in IQ-TREE and use those as input for weighted ASTRAL to estimate the species tree.

STEPS:
1. Download the gene tree files from all groups from Google Drive ([link here](https://drive.google.com/drive/folders/1E0Lfo5crQHkIM26PVa0lGHMKFbczsWfR?usp=sharing))
2. In your terminal app, navigate to the directory containing the gene tree files you downloaded.
3. Concatenate all the gene trees into a single file. This creates a new tree file (with the extension `*.newick`) containing a list of all the gene trees, which we will use as input for ASTRAL.

```
cat *.treefile > gene_trees.newick
```

4. Optional: copy the newick gene tree file into the ASTRAL `bin/` directory.
5. In your terminal app, navigate into ASTRAL bin directory:

```
cd ASTER-MacOS/bin
```

6. Run weighted ASTRAL with the command below, editing to your gene tree filename (`-i`). Here, we specify an outgroup (`--root`) as "Platysternon_megacephalum" and the output filename (`-o`).

```
./wastral -i gene_trees.newick --root "Platysternon_megacephalum" -o turtle_species_tree.tree
```

7. Open the output tree file in FigTree to look at it!
8. How does the species tree compare to the nuclear and mitochondrial gene trees? What does the species tree show as the phylogenetic relationships among the genera? Are all the relationships strongly supported?
