---
layout: page
title: A Bioinfo Workflow in the Browser
permalink: assignments/bioinfo_workflow
parent: Assignments
nav_order: 1
---

# The research question in hand
{:.no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Intro

We're going to get started with a workflow we can run entirely in the browser.

The initial step to all research is defining a question. What is it you want to know more about?

For our work in bioinformatics, this question is often exploratory. An exploratory analysis can also be a hypothesis-generative analysis. In other words, a question such as "What environmental samples contain Candida albicans?" may lead to the specific hypothesis of "Candida albicans may be increasingly identified in samples throughout time due to its opportunistic pathogenic nature and the increase use of sequencing technology"

![c_albicans_world_map_animation](https://hackmd.io/_uploads/rJv6uTB4xg.gif)

<details>
Initial inspiration brought about by [this](https://stackoverflow.com/questions/30706124/plotting-the-world-map-in-r)

```r
# read in the csv file containing the metadata
calbicans <- read.csv("path/to/SRAmetadata")

# use existing libraries in R to create a world map
WorldData <- map_data('world') %>% filter(region != "Antarctica") %>% fortify

# rename metadata to match mapdata, convert metadata date to the R Date class and only keep the year, group and summarize the data by region and year
year_cont_df <- calbicans %>%
  mutate(region = recode(geo_loc_name_country_calc,
                         "United States" = "USA",
                         "United States of America" = "USA",
                         "America" = "USA",
                         "United Kingdom" = "UK",
                         .default = geo_loc_name_country_calc)) %>%
  mutate(year = format(as.Date(collection_date_sam), "%Y")) %>%
  group_by(region, year) %>%
  summarize(n = n(), .groups = "drop")

# plot a map with year and region information
p <- ggplot() +
  geom_map(data = WorldData, map = WorldData,
           aes(x = long, y = lat, group = group, map_id=region),
           fill = "white", colour = "#7f7f7f", size=0.5) + 
  geom_map(data = year_cont_df, map=WorldData,
           aes(fill=n, map_id=region),
           colour="#7f7f7f", size=0.5) +
  coord_map("rectangular", lat0=0, xlim=c(-180,180), ylim=c(-60, 90)) +
  scale_fill_continuous(low="thistle2", high="darkred", guide="colorbar") +
  scale_y_continuous(breaks=c()) +
  scale_x_continuous(breaks=c()) +
  labs(fill="legend", title="Title", x="", y="") +
  theme_bw()
p 

# animate the plot to dynamically show the change
p_anim <- ggplot() +
  geom_map(data = WorldData, map = WorldData,
           aes(x = long, y = lat, group = group, map_id = region),
           fill = "white", colour = "#7f7f7f", size = 0.5) +
  geom_map(data = year_cont_df, map = WorldData,
           aes(fill = n, map_id = region, frame = year),
           colour = "#7f7f7f", size = 0.5) +
  coord_map("rectangular", lat0 = 0, xlim = c(-180, 180), ylim = c(-60, 90)) +
  scale_fill_continuous(low = "darkblue", high = "darkred", guide = "colorbar") +
  labs(fill = "legend", x = "", y = "") +
  theme_bw() +
  transition_states(year, transition_length = 2, state_length = 1) +
  labs(title = "C. albicans in env. sample - Year: {closest_state}")

# Play the animation
animate(p_anim, fps = 5, width = 800, height = 400)
# Or save the animation as a gif
anim_save("c_albicans_world_map_animation.gif", p_anim, fps = 5, width = 600, height = 300)
```
</details>

{: .note }
These steps may take some time because the web-based infrastructure takes a while to run. Prepare some lecture for students while waiting...

<br>

## Let's start by looking at where an organism of interest is found in public data sets
---

- Pick a species of interest to experiment with from the list below or from your own interest:

    - Escherichia coli
    - Staphylococcus aureus
    - Pseudomonas aeruginosa
    - Mycobacterium tuberculosis
    - Helicobacter pylori
    - Bacillus subtilis
    - Klebsiella pneumoniae
    - Streptococcus pneumoniae
    - Listeria monocytogenes
    - Haemophilus influenzae
    - Salmonella enterica
    - Chlamydia trachomatis
    - Enterococcus faecalis
    - Acinetobacter baumannii
    - Staphylococcus epidermidis
    - Streptococcus mutans
    - Neisseria gonorrhoeae
    - Vibrio cholerae
    - Bacillus cereus
    - Mycobacterium avium
        > [This list was borrowed from "These are the 20 most-studied bacteria - the majority have been ignored"](https://www.nature.com/articles/d41586-025-00038-x)

- Use the NCBI Web site (instructions below) to download a genome for your selected species :

    - [The National Center of Biotechnological Information (NCBI)](https://www.ncbi.nlm.nih.gov/datasets/genome/)

        > A GenBank (GCA) genome assembly contains assembled genome sequences submitted by investigators or sequencing centers to GenBank or another member of the International Nucleotide Sequence Database Collaboration (INSDC). The GenBank (GCA) assembly is an archival record that is owned by the submitter and may or may not include annotation. A RefSeq (GCF) genome assembly represents an NCBI-derived copy of a submitted GenBank (GCA) assembly maintained by NCBI and includes annotation.

        - Navigate to https://www.ncbi.nlm.nih.gov/datasets/genome/ 
        ![image](https://hackmd.io/_uploads/H151d2B4xg.png)
        - Type and select the species you are interested in exploring:
        ![image](https://hackmd.io/_uploads/HymnOhSNlx.png)
        - When downloading a single genome from the NCBI, start with the species reference genome (the green check mark):
        ![image](https://hackmd.io/_uploads/B1ICO3SNgg.png)
        - Select the three vertical dots and click `Download`:
        ![image](https://hackmd.io/_uploads/Byqx5hHEel.png)
        - Choose the RefSeq Genome Sequence to Download:
        ![image](https://hackmd.io/_uploads/H1iro2HElg.png)

    - ALTERNATE place to grab genomes: [The Genome Taxonomic DataBase (GTDB)](https://gtdb.ecogenomic.org/)
        > Importantly and increasingly, this dataset includes draft genomes of uncultured microorganisms obtained from metagenomes and single cells, ensuring improved genomic representation of the microbial world. All genomes are independently quality controlled using CheckM before inclusion in GTDB

        - Navigate to the link above:
        ![image](https://hackmd.io/_uploads/B1xXa2H4ee.png)
        - Search for the species you are interested in exploring and select one of the `Accession` links:
        ![image](https://hackmd.io/_uploads/HJANp2HVge.png)
        - Select the `GTDB Representative of Species`:
        ![image](https://hackmd.io/_uploads/SkkiphHNex.png)
        - Scroll down to `NCBI Metadata`:
        ![image](https://hackmd.io/_uploads/Hk0JChSEgl.png)
        - Click `Download`:
        ![image](https://hackmd.io/_uploads/rJfQA3SNle.png)
        - Choose the RefSeq Genome Sequence to Download:
        ![image](https://hackmd.io/_uploads/BJ0H0nSNgx.png)


- With a FASTA file downloaded, search all environmental DNA samples available in the public collection of the Sequence Read Archive (SRA) using [the branchwater tool](https://branchwater.jgi.doe.gov/) (this tool is from Titus/Colton lab) -

    - Upload the FASTA file from the previous section and click `Submit`:
    ![image](https://hackmd.io/_uploads/HyOGkTHNxl.png)
    - Look through the results and click `Download CSV`:
    ![image](https://hackmd.io/_uploads/rJmUy6S4xe.png)

At this point, we now have all the samples containing the genome you select to a 90% containment Average Nucleotide Identity (cANI). This means that our tool estimates that approximately 90% of the genome is present in the sample.

- With the accessions available from our SRA search, we can inquire to the make-up of the environmental DNA using [chill-filter](https://chill-filter.sourmash.bio/):
    {: .note }
    To download from the browser, the file must be < 5GB. We suggest starting with a file that is smaller, since it will need to download to your computer!

    - Use the Branchwater results to identify a sample you are interested in knowing the other genomes that make it up.
    - Download that sample by one of two ways:
    1. - Navigate to the [SRA](https://www.ncbi.nlm.nih.gov/sra):
       - Search for the sample by its accession number:
       ![image](https://hackmd.io/_uploads/rkiYpABElx.png)
       - Click on the `Run` link:
       ![image](https://hackmd.io/_uploads/SkMx0CBVgl.png)
       - Go to `FASTA/FASTQ download`
       ![image](https://hackmd.io/_uploads/rkUIARSVel.png)
    2. - Navigate to the [SRA Run Browser](https://www.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&display=metadata)
       - Search for the sample accession of your choice:
       ![image](https://hackmd.io/_uploads/S1MSRCHElx.png)
       - Go to `FASTA/FASTQ download`
       ![image](https://hackmd.io/_uploads/rkUIARSVel.png)

To submit this to the chill-filter Web site:

   - go to https://chill-filter.sourmash.bio/
   - Browse your computer to find the FASTAQ file you just downloaded:
    ![image](https://hackmd.io/_uploads/ryNT1k84gx.png)
   - Hit `Submit` to see the results.
    ![image](https://hackmd.io/_uploads/ryFq30BEgl.png)
   - Click the table links to see a further breakdown of the sample:
    ![image](https://hackmd.io/_uploads/BkSUZJL4ex.png)
    
And see what other organisms are there.

We can now ask questions like, what is correlated with our first search organism?

<br>

## Limitations
---

Why is this approach limited?
 - Because it is a point and click adventure (GUI and Browser)
 - Because you are at the whims of the developers (like us!), and those people often don't (can't) know anything about your specific scientific question!
 - For example, the map gif at the beginning of this document was derived from the same information the developer used to create their map on the branchwater results page.

To truly unlock your computational potential we must incorporate:
- a terminal
- scripts
- workflows

<br>

## a placeholder for conceptual information behind branchwater and chillfilter
---

![Scientific Fields Arranged By Purity](https://imgs.xkcd.com/comics/purity.png)

<br>

## Installation Festival
---

Currently, we have the intention of introducing you to using R/Python.

[Git](https://ucdavisdatalab.github.io/adventures_in_data_science/chapters/required-software.html) - a version control software that stores all the changes to the files within a directory. It also allows users to link any local repositories to a remote server (GitHub) for easy collaboration and more. This should also allow Windows users to have a bash emulator terminal called [Git Bash](https://git-scm.com/downloads)

[R and RStudio](https://ucdavisdatalab.github.io/workshop_r_basics/chapters/01_getting-started.html#prerequisites) - a statistical programming language that has a massive ecosystem, activate community, and wide user base in bioinformatics.

[Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/install#quickstart-install-instructions) - A customizable environment manager software that is packaged with the lastest version of Python. It allows users to create environments of various software combinations through `conda install`.

{: .warning }
Optional, [Pixi](https://ucdavisdatalab.github.io/workshop_installing_software/chapters/01_installing-software.html) the 'new kid' environment manager that utilizes the conda architecture to simplify the management of environments.

<br>

### Verification for Sanity
---

A standard way of verifying an installation is to use the `--version` keyword argument for the software installed.

```bash
conda --version
```

If the command returned something like `conda 24.11.3`, you have installed conda! Now to configure.

<br>

### Configuration for Easy-of-use
---

```bash
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```

This command adds the channel defaults to the `~/.condarc`, a configuration file stored in a "dotfile"

<br>

### Conda creation environment for downloading large files from the SRA
---

If you wanted to download a sample that was > 5GB, it's best to use the [SRA-tools](https://github.com/ncbi/sra-tools) and [awscli](https://aws.amazon.com/cli/)

These two software can be install through conda with a command similar to:

```bash
conda create -n <environment-name> -y <software-list...>
```

To find the conda package name for a software, search for it at the [anaconda website](https://anaconda.org/search?q=):

1. https://anaconda.org/bioconda/sra-tools

For an environment to download files from the SRA:

```bash
conda create -n sra -y sra-tools awscli
```

Enter the environment that contains the software:

```bash
conda activate sra
```

To download a large file from the SRA:

```bash
mkdir -p sra/ && aws s3 cp --quiet --no-sign-request s3://sra-pub-run-odp/sra/<sample-id>/<sample-id>  sra/<sample-id>.sra || prefetch --quiet <sample-id> -o sra/<sample-id>.sra
```

To parse the `sra` file into a usable format:

```bash
fasterq-dump sra/<sample-id>.sra --skip-technical --split-files --progress --threads 4 --bufsize 1000MB --curcache 10000MB --mem 16GB
```

{: .note }
This would be a great stopping point for the tangent of workflows and big data processing!!!

<details>
```py
import pandas as pd
from os.path import exists

# Create a dataframe of runs for each project
metadata = pd.read_csv(<file-path-to-list-of-accessions>, usecols=['Run'])

# Create a list from the "Run" column
SAMPLES = metadata['Run'].tolist()

# This pseudo rule designates the "stopping" point of the workflow
rule all:
    input:
         expand("sigs/{sample}.zip", sample=SAMPLES),

# These target rules perform a discrete action along an algorithm 
# Download an SRA file
# Convert that SRA file to a FASTA file

rule download_sra:
    output:
        temp("sra/{sample}.sra")
    shell:
       """
           aws s3 cp --quiet --no-sign-request \
           s3://sra-pub-run-odp/sra/{wildcards.sample}/{wildcards.sample} \
           sra/{wildcards.sample}.sra \
           || prefetch --quiet {wildcards.sample} -o sra/{wildcards.sample}.sra
       """

rule dump_and_fasta:
    input:
        sra_file = "sra/{sample}.sra"
    output:
        "sigs/{sample}.zip",
    shell:
        """
        fasterq-dump --stdout --skip-technical --fasta-unsorted \
         --threads 4 --bufsize 1000MB --curcache 10000MB --mem 8GB
        """
```
</details>

<br>

## Examples to scripted results
---

With the CSV containing the metadata of the environmental samples containing your selected genome:


{: .important }
The various scripts included in this document use the following R packages or libraries.

```r
library(tidyverse) # A collection of libraries for data analysis and visualization. Including ggplot2, dplyr, tidyr, readr, purrr, tibble, stringr, forcats, lubridate. 

#library(dplyr) # A data manipulation package.
#library(lubridate) # A package to extend R's ability to parse and manage Date and Time data classes.
#library(ggplot2) # A data visualization package

library(stringdist) # Matching, distance, and fuzzy finding strings in R at speed!
library(text2vec) # Text and NLP processing in R
library(pheatmap) # Make pretty heatmaps in R
library(gganimate) # Animate the plots in R
library(clipr) # Easily read and write to the clipboard
library(maps) # Basic maps in R
```

<details>
- https://www.tidyverse.org/packages/
- https://github.com/markvanderloo/stringdist/tree/master
- https://text2vec.org/
- https://github.com/raivokolde/pheatmap
- https://gganimate.com/
- https://github.com/mdlincoln/clipr
- https://github.com/adeckmyn/maps
</details>

To install these packages, use the `install.packages()` function with the "combine" function that creates a vector. I.e.
```r
install.packages(
  c('tidyverse', 'stringdist', 'text2vec', 'pheatmap', 'clipr', 'maps', 'gganimate')
  )
```

<br>

### What is the association of `Country` to `Organism`?
---

![image](https://hackmd.io/_uploads/rJBgf1L4lx.png)

<details>
```
DT::datatable(
  as.data.frame(
    table(calbicans$geo_loc_name_country_calc, calbicans$organism)
    ),
  options = list(order = list(list(3, 'desc'))),
  colnames = c("Country", "Organism", "Count")
)
```
</details>

A table like above is a fine tool to search and see how the metadata needs to be updated. For example, the `Country` "NP" is most likely "Not Provided" by the user on submission. Also, notice the many similar `Organism` values that should be collapsed into a single value.

![calbicans-country-organism](https://hackmd.io/_uploads/H1ie5kUVex.png)

<details>
```
assoc_df <- calbicans %>%
  count(geo_loc_name_country_calc, organism)

p <- ggplot(assoc_df, aes(x = organism, y = geo_loc_name_country_calc, fill = n)) +
  geom_tile() +
  scale_fill_viridis_c() +
  labs(
    title = "Candida Albicans Country vs Organism Association",
    x = "Organism",
    y = "Country",
    fill = "Count") +
  theme_minimal() +
  theme(
    axis.text.x = element_blank())

ggplotly(p)
```
</details>

<br>

### What is the association of `cANI` to `Containment`?
---

![calbicans-cANI-contain-count](https://hackmd.io/_uploads/HJF2m1INle.png)

<details>
```
DT::datatable(
  as.data.frame(table(calbicans$cANI, calbicans$containment)) %>%
    dplyr::filter(Freq > 0),
  options = list(
    pageLength = 100,
    order = list(
      list(1, 'desc'),
      list(2, 'desc')
    )
  ),
  colnames = c("cANI", "Containment", "Count")
)
```
</details>

A more succinct format would look like:

![succinct-cani-contain](https://hackmd.io/_uploads/S1YhIyLElx.png)

<details>
```
ani_containment_df <- calbicans %>%
  group_by(cANI) %>%
  summarize(
    containment_min = min(containment, na.rm = TRUE),
    containment_max = max(containment, na.rm = TRUE),
    count = n()
  ) %>%
  mutate(containment_range = paste0(containment_max, " - ", containment_min)) %>%
  select(cANI, containment_range, count) %>%
  arrange(desc(count))

DT::datatable(
  ani_containment_df,
  options = list(
    pageLength = 25,
    order = list(list(2, 'desc'))  # sort by count descending
  ),
  colnames = c("cANI", "Containment Range", "Count")
)
```
</details>

This simplified table highlights that there is a valley of sample counts between 0.9 and 1. To illustrate this observation a visualization can also be used to describe it through another medium.

![calbicans-cani-line](https://hackmd.io/_uploads/Bkr9v1IEle.svg)

<details>
```
ggplot(ani_containment_df, aes(x=cANI, y=count)) +
  geom_line() +
  labs(title = "Candida Albicans cANI across samples")
```
</details>

### What samples contain the most of my genome sequence?

![sample-contain](https://hackmd.io/_uploads/rk9F41IVgg.png)

<details>
```
DT::datatable(
  calbicans[, 1:3],
  options = list(
    pageLength = 100,
    order = list(
      list(2, 'desc'),
      list(3, 'desc')
    )
  ),
  colnames = c("Accession", "cANI", "Containment")
)
```
</details>

### How can I arrange the samples by highest cANI and Containment and copy to my clipboard?

```
sorted <- calbicans[order(calbicans[[2]], calbicans[[3]], decreasing = TRUE), 1:3]
write_clip(head(sorted$acc, n = 10), breaks = ", ")
```

Output:

```
SRR085109, SRR5439749, SRR2190408, SRR29449332, SRR13236832, SRR12907767, SRR20794593, SRR20794596, SRR19732280, SRR17780415
```

If we take this list and paste it in the search bar of the [SRA Run Selector](https://www.ncbi.nlm.nih.gov/Traces/study/)...

![image](https://hackmd.io/_uploads/Hk3EBk8Vex.png)

We can get access to all the metadata of these samples!

![image](https://hackmd.io/_uploads/S1QuHkLNxx.png)

### What are the BioProjects with samples of highest containment and what are their `Organism`

![all-projectids-cani-organism](https://hackmd.io/_uploads/SyjXYJUNll.png)

<details>
```
ani_org_df <- calbicans %>%
  group_by(cANI, organism) %>%
  summarize(
    count = n(),
    bioprojects = paste(unique(bioproject), collapse = ", ")
  ) %>%
  mutate(
    BioProject = paste0(
      "<span title='", htmltools::htmlEscape(bioprojects), "'>",
      sub(",.*", ", ...", bioprojects),
      "</span>"
    ))

DT::datatable(
  ani_org_df[-4],
  options = list(
    pageLength = 100,
    order = list(
      list(1, 'desc'),
      list(3, 'desc')
    )
  ),
  colnames = c("cANI", "Organism", "Count", "BioProject"),
  escape = FALSE
)
```
</details>

### How can I sort through the metadata that seems erroneous?

![similarity_heatmap](https://hackmd.io/_uploads/BJD_nJLExe.svg)

<details>
Inspired through searching the web and finding [this](https://stackoverflow.com/a/62101516)

```
calbicans_organism <- unique(calbicans[c("organism")])

prep_fun = function(x) {
  # make text lower case
  x = str_to_lower(x)
  # remove non-alphanumeric symbols
  x = str_replace_all(x, "[^[:alnum:]]", " ")
  # collapse multiple spaces
  str_replace_all(x, "\\s+", " ")
}

map_metadata <- function(df, column) {
  map_dfr(pull(df, {{ column }}), ~ {
    vals <- pull(df, {{ column }})
    i <- which(stringdist(., vals, method = "jw") < 0.40)
    tibble(index = i, metadata = vals[i])
  }, .id = "group") %>%
    distinct(index, .keep_all = TRUE) %>%
    mutate(group = as.integer(group))
}

calbicans_organism$organism_clean = prep_fun(calbicans_organism$organism)

org_meta <- map_metadata(calbicans_organism, organism_clean)

it <- itoken(org_meta$metadata)
vocab <- create_vocabulary(it)
dtm <- create_dtm(it, vocab_vectorizer(vocab))
sim_matrix <- sim2(dtm, method = "cosine", norm = "l2")

rownames(sim_matrix) <- colnames(sim_matrix) <- org_meta$metadata

p_h <- pheatmap(sim_matrix,
         cluster_rows = FALSE,
         cluster_cols = FALSE,
         display_numbers = FALSE,
         color = colorRampPalette(c("blue", "white", "red"))(100),
         main = "Cosine Similarity Heatmap")
```
</details>

![similarity_heatmap-20-topics](https://hackmd.io/_uploads/SkKW0J8Vxl.svg)

<details>
```
lsa = LSA$new(n_topics = 20)
dtm_tfidf_lsa = fit_transform(dtm_tfidf, lsa)
sim_matrix3 = sim2(x = dtm_tfidf_lsa, method = "cosine", norm = "l2")

rownames(sim_matrix3) <- colnames(sim_matrix3) <- org_meta$metadata

pheatmap(sim_matrix3,
         cluster_rows = TRUE,
         cluster_cols = TRUE,
         display_numbers = FALSE,
         color = colorRampPalette(c("blue", "white", "red"))(100),
         main = "Cosine Similarity Heatmap")
```
</details>
