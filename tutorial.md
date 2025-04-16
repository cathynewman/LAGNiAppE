# Tutorial: Gene Trees & Species Trees

LAGNiAppE @ LSU 2025
Cathy Newman (UAB)

**These instructions are for MacOS.** This tutorial and the necessary files are posted on GitHub here: [https://github.com/cathynewman/LAGNiAppE/](https://github.com/cathynewman/LAGNiAppE/)

This tutorial will walk you through how to generate individual gene trees for primates, compare the gene tree topologies for your gene(s) and the genes your fellow scholars analyzed, and use those gene trees as input to generate a species tree.

Objectives:
* Practice the steps of aligning sequences and generating gene trees and species trees.
* Explore the phylogenetic relationships of turtles in the family Emydidae.
* Understand drivers of gene tree discordance and how species trees differ from gene trees.
* Use FigTree and RStudio to visualize and edit trees.

> [!NOTE]
> Tree files in Newick format are all plain-text files. It doesn't really matter what the file extension is. Files with extensions `*.newick`, `*.tre`, `*.tree`, `*.treefile`, etc., are all equivalent. Some software tools expect specific filename extensions, but I don't think it actually matters here.

## PART 1: Sequence alignment – AliView

In this section, we will download a `*.fasta` file of DNA sequences for a given gene and generate a multisequence alignment in AliView, which uses the MUSCLE alignment algorithm.

STEPS:
1. Download the `*.fasta` file for your genes from GitHub.
2. Open AliView and load your list of sequences:  File > *Open File*
3. Align the sequences:  Align > *Realign everything*
4. If alignment ends are uneven (sequences aren't all the same length), AliView automatically fills the spaces with dashes (-). Technically, dashes indicate "real" alignment gaps, but in this case the spaces are simply missing data, not necessarily gaps. So replace gaps (-) with missing data (?) at the ends:  Edit > *Replace terminal gaps into missing char (?)*
5. Scroll through the alignment to make sure it meets the "eye test" (looks good to you).
6. Save alignment as **phylip file (full names & padded)**

## PART 2: Gene trees – IQ-TREE

In this section, we will reconstruct a phylogenetic tree for the gene alignment from the previous step. We will run 1,000 "ultrafast" bootstrap pseudoreplicates so that the gene tree has nodal support values to use, along with branch lengths, in the estimation of the species tree in the next section.

STEPS:
1. Optional: copy your sequence alignment file into the `IQ-TREE bin/` directory. This makes it easier to specify the file path in step 3 below. Alternatively, just use `/path/to/alignmentfile.phy`
2. In your terminal app, navigate into IQ-TREE bin directory:

```
cd iqtree-3.0.0-macOS/bin
```

3. Run IQ-TREE with the command below, editing to your alignment filename (-s). Here, we specify an outgroup (-o) as Platysternon_megacephalum, and we will run 1,000 ultrafast bootstrap pseudoreplicates (-B).

```
./iqtree3 -s alignmentfile.phy -o "Platysternon_megacephalum" -B 1000
```

4. Open the output tree file (*.treefile) in FigTree to look at it!
5. Upload your tree file (just the *.treefile file) to the Google Classroom workshop directory [ADD LINK HERE]
6. What do your gene trees show as the phylogenetic relationships among the emydid genera? Compare your trees to other groups. What do you think might be causing any observed differences?

## PART 3: Species trees – weighted ASTRAL

Now we will take all of the individual gene trees you and your fellow scholars generated in IQ-TREE and use those as input for weighted ASTRAL to estimate the species tree.

STEPS:
1. Download the gene tree files from all groups from the Google Classroom workshop directory.
2. In your terminal app, navigate to the directory containing the gene tree files you downloaded.
3. Concatenate all the gene trees into a single file. This creates a new tree file (with the extension *.newick) containing a list of all the gene trees, which we will use as input for ASTRAL.

```
cat *.treefile > gene_trees.newick
```

4. Optional: copy the newick gene tree file into the ASTRAL bin/ directory.
5. In your terminal app, navigate into ASTRAL bin directory:

```
cd ASTER-MacOS/bin
```

6. Run weighted ASTRAL with the command below, editing to your gene tree filename (-i). Here, we specify an outgroup (--root) as Platysternon_megacephalum and the output filename (-o).

```
./wastral -i gene_trees.newick --root "Platysternon_megacephalum" -o turtle_species_tree.tree
```

7. Open the output tree file in FigTree to look at it!
8. How does the species tree compare to your gene trees? What does the species tree show as the phylogenetic relationships among the genera? Are all the relationships strongly supported?
