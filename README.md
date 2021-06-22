# Introduction
This repository contains the course materials for hands-on exercise of the "Variants Annotation and Phenotype Analysis" session in the Quantitative Genomics workshop at Columbia University on June 23-25, 2021.

# Preparation of computing environment
Both the lectures and hands-on exercises will be taught via Zoom video conference online. To ensure the cross-platform compatibility, we will only use Rstudio Cloud to implement tools that are developed in Perl and Python.

Students need to go to [RStudio Cloud](https://rstudio.cloud/) to create your own account before the lectures: 
1. click 'Sign up' on the top-right corner
<img src="https://user-images.githubusercontent.com/16017780/122824588-de42e300-d2ae-11eb-8999-3775f83568ef.png" width="400" height="200">

2. select "Free" and click "Sign up" at the bottom-right corner. 
<img src="https://user-images.githubusercontent.com/16017780/122824608-e8fd7800-d2ae-11eb-9f14-b1ded8d1e03a.png" width="400" height="200">

3. After you input your information, click "Sign up". 
<img src="https://user-images.githubusercontent.com/16017780/122824633-f0248600-d2ae-11eb-9bd1-a72b8f03383b.png" width="200" height="300">

If you logout after signing up, you can go to [RStudio Cloud](https://rstudio.cloud/) and login with your username and password. After you are added to "QuantitativeGenomics2021", you can go to "QuantitativeGenomics" spaces on the left, and then click "New project" to create your projects for practice as shown below. It will take ~1 minute to deploy and prepare the project for you. 
![image](https://user-images.githubusercontent.com/16017780/122825757-58279c00-d2b0-11eb-840a-461f8699e1d9.png)

By default, you will be in "Console" as shown below where you can run R code.
![image](https://user-images.githubusercontent.com/16017780/122825824-6e355c80-d2b0-11eb-8f8e-14af92192188.png)

You can also click "Terminal" so that you can run linux commands.
![image](https://user-images.githubusercontent.com/16017780/122826165-cec49980-d2b0-11eb-9076-5461b0cd4ee2.png)

During the exercise, you can feel free to swith to Terminal when you need to run linux commands and to Console when you need to run R code.

# Basic Linux commands

After login to Rstudio Cloud, you already have terminal access by clicking 'terminal' button and can run all basic Linux commands. If you are not familiar with basic Linux commands, you can follow one simple tutorial to learn several commands: https://maker.pro/linux/tutorial/basic-linux-commands-for-beginners. Some of the  commands that we will use in the exercise include `ls`, `pwd`, `cd`, `mv` and `wget`.

# Functional annotation of genetic variants

### 1. Install ANNOVAR

Typically you will go to the [ANNOVAR website](http://annovar.openbioinformatics.org), fill in a registration form, and download the package there. For this exercise, we already prepared a ZIP file that contains a "compact" version of ANNOVAR and necessary library files, to make it easier for users. To make it easier to manage files and directories, you can create a new directory, then enter this new directory (type `mkdir genomics_exercise` followed by `cd genomics_exercise`). To confirm which directory you are in, you can type `pwd`. You will see the example below.

```
/cloud/project$ mkdir genomics_exercise
/cloud/project$ cd genomics_exercise/
/cloud/project/genomics_exercise$ pwd
/cloud/project/genomics_exercise
```

Next, you can just download the ZIP file for this class by the command `wget https://github.com/WGLab/QuantitativeGenomics2021/releases/download/v1.0.0/exercise1.tar.gz`. The Linux command `wget` essentially downloads a file from a given URL and saves the file to your computer. Because this file contains several annotation databases, its size is around 500Mb and it may take a while to download it. To unzip the file, you can dirctly using `tar -xvf exercise1.tar.gz` to unzip the downladed file. You will see from the messages in screen that several files are extracted from the zip file. Please note that this is a sub-sampled version of ANNOVAR for the purpose of th exercise today (to reduce file size significantly), and it should not be used in any other real data analysis.

```
/cloud/project/genomics_exercise$ tar -xvf exercise1.tar.gz
exercise1/
exercise1/example/
exercise1/example/ex3.avinput
exercise1/example/ex2.vcf
exercise1/example/ex1.avinput
exercise1/example/gene_xref.txt
exercise1/table_annovar.pl
exercise1/convert2annovar.pl
exercise1/retrieve_seq_from_fasta.pl
exercise1/coding_change.pl
exercise1/sarscov2db/
exercise1/sarscov2db/NC_045512v2_avGeneMrna.fa
exercise1/sarscov2db/NC_045512v2_avGene.txt
exercise1/humandb/
exercise1/humandb/hg19_refGeneWithVerMrna.fa
exercise1/humandb/hg19_gnomad211_exome.txt.idx
exercise1/humandb/hg19_cytoBand.txt
exercise1/humandb/hg19_refGeneWithVer.txt
exercise1/humandb/hg19_gnomad211_exome.txt
exercise1/annotate_variation.pl
exercise1/variants_reduction.pl
```

### 2. Run ANNOVAR on a small VCF file

Type `cd exercise1` to enter the `exercise1` directory. The sub-folder `humandb` folder already contains several annotation databases for human genome that we will use in our exercise. (Note that users can find more annotation databases [here](https://doc-openbio.readthedocs.io/projects/annovar/en/latest/user-guide/download/#-for-filter-based-annotation). Run `chmod +x *.pl` to change all perl programs to executable files.

```
perl table_annovar.pl example/ex2.vcf humandb/ -buildver hg19 -out myanno -remove -protocol refGeneWithVer,cytoBand,gnomad211_exome -operation g,r,f -nastring . -vcfinput -polish
```

After that, you will find the result files `myanno.hg19_multianno.txt` and `myanno.hg19_multianno.vcf`.

In the command above, we specified three protocols, including RefGene annotation (a gene-based annotation), cytogenetic band annotation (a region-based annotation), and allele frequency in gnoMAD version 2.1.1 (a filter-based annotation). These three types of annotation are indicated as `g`, `r` and `f` in the -operation argument, respectively. We also specify that dots ('.') be used to indicate lack of information, so variants that are not observed in the database will have '.' as the annotation. The `-vcfinput` argument specifies that the input file is in VCF format.

If you have Excel installed, you can open the `hg19_multianno.txt` file by Excel and examine the various tab-delimited fields.

### 3. Run ANNOVAR on a human exome

Next, we want to download a VCF file and then run ANNOVAR on this file.

In the `exercise1` folder:

```
wget http://molecularcasestudies.cshlp.org/content/suppl/2016/10/11/mcs.a001131.DC1/Supp_File_2_KBG_family_Utah_VCF_files.zip -O Supp_File_2_KBG_family_Utah_VCF_files.zip
```

Next unzip the ZIP file (`unzip`):

```
unzip Supp_File_2_KBG_family_Utah_VCF_files.zip
mv './File 2_KBG family Utah_VCF files/' VCF_files
```

Run ANNOVAR on the VCF file:
```
perl table_annovar.pl VCF_files/proband.vcf -buildver hg19 humandb -out proband.annovar -remove -protocol refGeneWithVer,gnomad211_exome -operation g,f -nastring . -vcfinput
```

The `proband.annovar.hg19_multianno.txt` file contains annotations for this exome.


### 4. Results visualization

In the `exercise1` folder, run `pwd` to obtain the working directory and copy this path. 

Paste this path to R console or R studio interface to setup the working directory using function `setwd()`. For example:

```
> setwd("/cloud/project/genomics_exercise/exercise1")
```

Then, run R code in `Results_Visualization.R` to visualize the results.

Check variant distribution across chromesomes:
```{r}
#load data
res <- read.table("proband.annovar.hg19_multianno.txt", fill=T, header=T, sep="\t", na.strings = ".")

#visualize variant frequency
attach(mtcars)
par(mar=c(5.1, 4.1, 4.1, 2.1),mfrow=c(1,1))
table <- table(res$Chr)
chrlist <- paste0("chr",1:22)
chrlist <- c(chrlist, "chrX")
barplot(table[chrlist], ylab="Number of Variant", las=2)
````

At this point, you should see a barplot similar to the one below:

![image](https://user-images.githubusercontent.com/11565618/123002115-87590e80-d37f-11eb-8b90-bf2901e3a8c8.png)

Check allele frequency distribution between non-synonymous, synonymous and intronic variants:
```{r}
#visualize allele frequency
par(mar=c(5.1, 4.1, 4.1, 2.1),mfrow=c(3,1))
af_syn<-res[res$ExonicFunc.refGeneWithVer=="synonymous SNV",]$AF
hist(as.numeric(af_syn),  ylab="Frequency", xlab="Allele frequency", main="Synonymous SNV", breaks=seq(0,1,.001))

af_nonsyn<-res[res$ExonicFunc.refGeneWithVer=="nonsynonymous SNV",]$AF
hist(as.numeric(af_nonsyn), ylab="Frequency", xlab="Allele frequency", main="nonSynonymous SNV",breaks=seq(0,1,.001))

af_intron<-res[res$Func.refGeneWithVer =="intronic",]$AF
hist(as.numeric(af_intron), ylab="Frequency", xlab="Allele frequency", main="Intronic", breaks=seq(0,1,.001))
```

You should see a figure similar to the one below:

![image](https://user-images.githubusercontent.com/11565618/123002219-a9eb2780-d37f-11eb-9452-01cf20f5eedc.png)



Check allele frequency distribution across race:
```{r}
#visualize allele frequency across race
par(mar = c(12, 5, 2, 2),mfrow=c(1,1)) 
res_sub <- res[res$Gene.refGeneWithVer=="NRXN2",]
variantname <- res_sub$Otherinfo6[1]
res_sub <- as.matrix(res_sub[1,16:23])
colnames(res_sub) <- c("African/African-American", "South Asian", 
                       "Latino/Admixed American", "East Asian", "Non-Finnish European", "Finnish",
                       "Ashkenazi Jewish", "Other")
barplot(res_sub, ylab="Allele frequency", main=variantname, las=2)
```

You should see a figure similar to the one below:

![image](https://user-images.githubusercontent.com/11565618/123002260-ba030700-d37f-11eb-811b-54eb98fb0c73.png)


### 5. Run ANNOVAR to analyze a new strain of SARS-CoV-2

Suppose we sequenced a patient with COVID-19 and performed alignment, filtering, read assembly, and generate variant calls (the calls are generated by comparing to reference genome NC045512.2). We want to evaluate what effects that the virus mutations in this patient may have.

You can prepare a simple text file called `ex3.avinput` with the following mutations: 

```
NC_045512v2     29095   29095   C       T
NC_045512v2     26144   26144   G       T
NC_045512v2     28144   28144   T       C
```

This format is referred to as the avinput format, and it is a simple tab or space delimited file, with each variant per line. There can be many columns per line, but the first five columns in each line represent chr, start, end, reference allele and alternative allele, respectively.

Then you can just run `perl table_annovar.pl example/ex3.avinput sarscov2db -build NC_045512v2 -protocol avGene -operation g -out ex3` to annotate these mutations and the results will be stored in ex3.NC_045512v2_multianno.txt. 

You can use the Linux command `cat` to show the content of this file, since it is a relatively small file.

![image](https://user-images.githubusercontent.com/5926328/122944485-dd10c500-d345-11eb-8cce-4a96dbb61eb3.png)

Additional exercise: The [B.1.167.2](https://en.wikipedia.org/wiki/SARS-CoV-2_Delta_variant) variant (The WHO named it the Delta variant on 31 May 2021) is a variant of concern, because it showed evidence of higher transmissibility and reduced neutralisation. The variant is thought to be partly responsible for India's second wave of the pandemic beginning in February 2021. As of June 22, 2021, the distribution of delta variant from GISAID is shown below.

![image](https://user-images.githubusercontent.com/5926328/122996121-7953bf80-d378-11eb-9196-deb89f19f466.png)

The first sequence on B.1.167.2 was sequenced on 3/23/2021 from Australia (identifier "NSW-R0167/2021|EPI_ISL_1315070|2021-03-18"). Note that there are already 66,276 sequences for B.1.167.2 in GISAID database as of June 22, 2021, but this one is the first reported one. We analyzed this sequence and identified a total of 34 mutations listed below. Now please use the procedure learned above, annotate these mutations to see how they affect proteins in SARS-CoV-2.

One thing that becomes immediately clear from your analysis is that B.1.167.2 contains the famous L452R mutation and the [D614G mutation](https://www.nature.com/articles/s41586-020-2895-3). However, it does NOT contain the E484Q mutation. Therefore, the statements made in Wikipedia page is wrong as of June 22, 2021 ("The Delta variant has mutations in the gene encoding the SARS-CoV-2 spike protein[5] causing the substitutions E484Q and L452R.[7]".

```
NC_045512v2  210 210 G   T   1
NC_045512v2  241 241 C   T   1
NC_045512v2  1191    1191    C   T   1
NC_045512v2  1267    1267    C   T   1
NC_045512v2  1825    1825    C   T   1
NC_045512v2  3037    3037    C   T   1
NC_045512v2  5184    5184    C   T   1
NC_045512v2  9203    9203    G   A   1
NC_045512v2  9678    9678    T   C   1
NC_045512v2  11005   11005   C   A   1
NC_045512v2  14408   14408   C   T   1
NC_045512v2  17496   17496   A   G   1
NC_045512v2  20396   20396   A   G   1
NC_045512v2  21618   21618   C   G   1
NC_045512v2  21792   21792   A   C   1
NC_045512v2  21987   21987   G   A   1
NC_045512v2  22029   22035   GAGTTCA G   1
NC_045512v2  22917   22917   T   G   1
NC_045512v2  22995   22995   C   A   1
NC_045512v2  23403   23403   A   G   1
NC_045512v2  23604   23604   C   G   1
NC_045512v2  24410   24410   G   A   1
NC_045512v2  25469   25469   C   T   1
NC_045512v2  26767   26767   T   C   1
NC_045512v2  27638   27638   T   C   1
NC_045512v2  27752   27752   C   T   1
NC_045512v2  28040   28040   A   G   1
NC_045512v2  28253   28253   C   T   1
NC_045512v2  28271   28272   TA  T   1
NC_045512v2  28461   28461   A   G   1
NC_045512v2  28881   28881   G   T   1
NC_045512v2  29402   29402   G   T   1
NC_045512v2  29742   29742   G   T   1
```




# Phenotype-driven prioritization of human disease genes (Phen2Gene, ClinPhen, AMELIE, etc)

Phen2Gene is a phenotype-based gene prioritization tool from HPO IDs or clinical notes on patients. In the next exercise, we will first use Phen2Gene to prioritize genes based on clinical phenotypes of patients with Mendelian diseases.

We will do the exercise in a directory called `exercise2`. So first use `cd ..` to go to the upper directory, then create this new directory, then go into this directory.

```
/cloud/project/genomics_exercise/exercise1$ cd ..
/cloud/project/genomics_exercise$ ls
exercise1  exercise1.tar.gz
/cloud/project/genomics_exercise$ mkdir exercise2
/cloud/project/genomics_exercise$ cd exercise2/
/cloud/project/genomics_exercise/exercise2$ 
```

There are three ways to run Phen2Gene: download and run it locally (need a few GB of space), using API and using Phen2Gene website. Today, we will perform the latter 2 ways of running Phen2Gene. Of course, if you are interested downloading Phen2Gene and run it locally in a different computer server, you can follow instructions here: https://github.com/WGLab/Phen2Gene.

The benefit of running Phen2Gene is if you do not have any idea of a candidate gene for your disease, you can use it in one of three scenarios:

1. Ideally, you have a list of physician-curated HPO terms describing a patient phenotype and a list of potential candidate disease variants that overlap gene space and you want to narrow down the list of variants by prioritizing candidate disease genes, often in tandem with variant prioritization software, which cannot as of yet score STR expansions or SVs unlike Phen2Gene which is variant agnostic.
2. You do not have variants, but you have HPO terms and would like to get some candidate genes for your disease that you may want to target sequence, as it is much cheaper than whole-genome or whole-exome sequencing.
3. If you have clinical notes, you can use tools like EHR-Phenolyzer or Doc2HPO for processing clinical notes into HPO terms using natural language processing (NLP) techniques, then apply scenario 1 or 2 as relevant.

Other tools listed below (ClinPhen, AMELIE, GADO) require a gene list, and Phen2Gene does not require any variant data or prior gene lists to provide high quality candidate genes.  One would most likely use Phen2Gene for obtaining the genetic etiology of an undiagnosed rare disease with no obvious genetic cause.


### 1. Using Phen2Gene API.
1. Go to Terminal, make sure you are in the `exercicse2` directory first, and run `curl -i -H "Accept: application/json" -H "Content-Type: application/json" "https://phen2gene.wglab.org/api?HPO_list=HP:0002459" | tail -n 1 > output.txt`
where you generate JSON output in `output.txt`

2. Go To Console, remember that we should first set `exercise2` as the working directory.

```
setwd("../exercise2")
```

3. and we can run
```
# install JSON in R
install.packages("rjson")

# Load JSON in R
library("rjson")

# Read JSON results
result <- fromJSON(file = "output.txt")

# Convert them to array and name columns.
marray <- t(array( unlist(result$results), dim=c(5, length(result$results)) ) );
colnames(marray) <- names(result$results[[1]]);

# View the results in 2-D array. The second column is the rank of genes.
View (marray);
```
![image](https://user-images.githubusercontent.com/16017780/122829197-b2c2f700-d2b4-11eb-9fe7-a4079bfe286d.png)

You can see that the top ranked genes are ALK, ATP7A, TTR, etc.

### 2. Using the Phen2Gene Website to assess the ANKRD11 case

Go to https://phen2gene.wglab.org.  Click on the tab `Patient notes`:

![image1](https://user-images.githubusercontent.com/6568964/84083556-df5ce700-a9af-11ea-87c5-d02cc742b8c4.png)

and paste this paragraph of clinical notes in the text box:

```
The proband had an abnormally large fontanelle, which resolved without treatment. The proband does not appear to have a sacral dimple. Other than the presence of flat arches, there are no obvious signs of foot abnormalities. The proband does not look like his other siblings, although there was a resemblance between him and his sister when they were the same age. Features of proband’s face can be seen, including bushy eyebrows, broad nasal tip, short philtrum, full lips and cupid bow of upper lip. Video of proband’s behavior. Proband is non-verbal, and hyperactive. He repetitively spins his toy. While playing, he gets up from his chair, walks a few steps, stomps his feet, and sits back down. Additional interview in August 2015, after the parents received the proband’s diagnosis of KBG syndrome. The proband develops new mannerisms every four to six months, the most recent being short, hard breaths through the nose and head turning. The proband has had a substantial decrease in the number of seizures after starting an Epidiolex (cannabidiol) treatment (70-80% decrease as described by the parents). The frequency of seizures increased after the proband fell and fractured his jaw.  The mother describes the proband’s macrodontia. Although the mother and several siblings have large teeth, the macrodontia in the proband does not appear in any other member of the family.  The proband’s features are compared to other characteristics usually found in other KBG patients. Unlike most KBG patients, the proband has full lips. Like most KBG patients, the proband has curved pinkies (diagnosed as clinodactyly), which are often found in KBG patients.  Although the proband has relatively short toes, this trait may have been inherited from the father. The proband also has curved toenails, which commonly appear in autistic children.
```

![image2](https://user-images.githubusercontent.com/6568964/84083597-f26fb700-a9af-11ea-8fff-ead80c87d479.png)

Then click Submit.

![image3](https://user-images.githubusercontent.com/6568964/84083616-fac7f200-a9af-11ea-97cd-68585539d7fe.png)

You should see that ANKRD11 is in the top 3.  The reason it is not 2 as in the previous example is that we manually curated HPO terms from the patient notes for each patient in our Phen2Gene paper and this is raw notes with no curation done and all HPO terms are being automatically extracted by Doc2HPO.  Despite this, the discrepancy in our results is minimal.

![image4](https://user-images.githubusercontent.com/6568964/84083649-0fa48580-a9b0-11ea-81ed-7d999f40eade.png)

Alternatively, you can also submit the HPO terms from `example/ANKRD11.txt` manually using the tab `HPO IDs` (they are already the default terms in the website so you can just click `Submit` on that tab).

![image5](https://user-images.githubusercontent.com/6568964/84083556-df5ce700-a9af-11ea-87c5-d02cc742b8c4.png)

And you should see that ANKRD11 is still number 2.

![image6](https://user-images.githubusercontent.com/6568964/84211809-3afba300-aa8a-11ea-8674-89518b0f8576.png)

### 3. Run ClinPhen

ClinPhen is another tool to analyze clinical notes. To run this tool, 
1. Go to Terminal and run the commands below to install it.
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

python get-pip.py

export PATH="/home/rstudio-user/.local/bin":$PATH

pip install clinphen
```

2. Save clinical notes 
In Terminal, type "vi m_notes.txt",  then press "Insert" botton or "i", then copy the notes below, and click right mouse button followed by "paste"
```
The proband had an abnormally large fontanelle, which resolved without treatment. The proband does not appear to have a sacral dimple. Other than the presence of flat arches, there are no obvious signs of foot abnormalities. The proband does not look like his other siblings, although there was a resemblance between him and his sister when they were the same age. Features of proband’s face can be seen, including bushy eyebrows, broad nasal tip, short philtrum, full lips and cupid bow of upper lip. Video of proband’s behavior. Proband is non-verbal, and hyperactive. He repetitively spins his toy. While playing, he gets up from his chair, walks a few steps, stomps his feet, and sits back down. Additional interview in August 2015, after the parents received the proband’s diagnosis of KBG syndrome. The proband develops new mannerisms every four to six months, the most recent being short, hard breaths through the nose and head turning. The proband has had a substantial decrease in the number of seizures after starting an Epidiolex (cannabidiol) treatment (70-80% decrease as described by the parents). The frequency of seizures increased after the proband fell and fractured his jaw.  The mother describes the proband’s macrodontia. Although the mother and several siblings have large teeth, the macrodontia in the proband does not appear in any other member of the family.  The proband’s features are compared to other characteristics usually found in other KBG patients. Unlike most KBG patients, the proband has full lips. Like most KBG patients, the proband has curved pinkies (diagnosed as clinodactyly), which are often found in KBG patients.  Although the proband has relatively short toes, this trait may have been inherited from the father. The proband also has curved toenails, which commonly appear in autistic children.
```

After that, press "ESC", and then shift+":wq" to exit the vi editor.

To make things easier for users who do not know how to use vi, we also prepared this file in the `exercise1/example` directory as `m_notes.txt`. So you can also just directly use that file instead.

3. Run ClinPhen
```
clinphen m_notes.txt
```

The expected results are below:

```
/cloud/project/genomics_exercise/exercise2$ clinphen m_notes.txt
HPO ID  Phenotype name  No. occurrences Earliness (lower = earlier)     Example sentence
HP:0012471      Thick vermilion border  2       12      unlike most kbg patients the proband has full lips 
HP:0000574      Thick eyebrow   1       9       features of proband s face can be seen including bushy eyebrows broad nasal tip short philtrum full lips and cupid bow of upper lip 
HP:0000455      Broad nasal tip 1       10      features of proband s face can be seen including bushy eyebrows broad nasal tip short philtrum full lips and cupid bow of upper lip 
HP:0000322      Short philtrum  1       11      features of proband s face can be seen including bushy eyebrows broad nasal tip short philtrum full lips and cupid bow of upper lip 
HP:0002263      Exaggerated cupid's bow 1       13      features of proband s face can be seen including bushy eyebrows broad nasal tip short philtrum full lips and cupid bow of upper lip 
HP:0001250      Seizures        1       32      the frequency of seizures increased after the proband fell and fractured his jaw 
HP:0030084      Clinodactyly    1       42      like most kbg patients the proband has curved pinkies diagnosed as clinodactyly which are often found in kbg patients 
```

You will get the results from ClinPhen. You can also type `clinphen --help` if you are interested in more options.

### 4. Run AMELIE

[AMELIE](https://amelie.stanford.edu/) is yet another tool for analyzing clinical notes, however it requires a candidate gene list.  We put the real causal gene in a algorithm-allowed maximum 1000 gene list of random exome capture genes.

First copy [this list of HPO terms](https://raw.githubusercontent.com/WGLab/Phen2Gene/master/example/HPO_sample.txt) to your clipboard. Then go to the [AMELIE website](https://amelie.stanford.edu/) and click "Submit Case."

![image](https://user-images.githubusercontent.com/6568964/122857269-3812cf80-d2e6-11eb-9649-4a30f0d8d94d.png)

Now, paste the HPO term list into "Case phenotypes."

![image](https://user-images.githubusercontent.com/6568964/122857504-93dd5880-d2e6-11eb-94b8-d4ce749df014.png)

Then, copy [this list of genes](https://raw.githubusercontent.com/WGLab/Phen2Gene/master/example/1000genetest.txt) to your clipboard.  

Next, paste the gene list into "Case genotype" under the tab "Gene List" then click Submit.

![image](https://user-images.githubusercontent.com/6568964/122857542-a2c40b00-d2e6-11eb-82ca-ac10ea712772.png)

You'll get a screen that looks like this:

![image](https://user-images.githubusercontent.com/6568964/122857711-ea4a9700-d2e6-11eb-8c03-761257d6ff8b.png)

And a wheel that tells you to wait a _while_.

After the wheel is done spinning your results should look like the following:

![image](https://user-images.githubusercontent.com/6568964/122857763-051d0b80-d2e7-11eb-8e14-2941636be222.png)

The number 1 gene, TAF1 is the correct causal gene.  But Phen2Gene gets the same result.

### 5. Summary of the phenotype analysis exercises

In summary, a number of computational tools such as Phen2Gene, AMELIE and GADO can perform phenotype-driven gene prioritization. Phen2Gene provides webserver or API, or you can install and run locally (which is important to deploy it in batch processing mode), and it does not require a list of random genes to run either.

# Running PhenCards (optional exercises)

Click [this link](PhenCards.md) to learn how you can use PhenCards.

# CITATIONS:

- [ANNOVAR](https://academic.oup.com/nar/article/38/16/e164/1749458): Wang K, Li M, Hakonarson H. ANNOVAR: Functional annotation of genetic variants from next-generation sequencing data. Nucleic Acids Research, 38:e164, 2010
- [Phen2Gene](https://academic.oup.com/nargab/article/2/2/lqaa032/5843800): Zhao, M. et al. Phen2Gene: rapid phenotype-driven gene prioritization for rare diseases. NAR Genom Bioinform, 2:lqaa032 (2020).
- [AMELIE](https://stm.sciencemag.org/content/12/544/eaau9113.abstract): Birgmeier, J. et al. AMELIE speeds Mendelian diagnosis by matching patient phenotype and genotype to primary literature. Sci. Transl. Med. 12:eaau9113, (2020).
- [GADO](https://www.nature.com/articles/s41467-019-10649-4): Deelen, P. et al. Improving the diagnostic yield of exome- sequencing by predicting gene-phenotype associations using large-scale gene expression analysis. Nat. Commun. 10:2837 (2019).
- [ClinPhen](https://www.nature.com/articles/s41436-018-0381-1): Deisseroth, C. A. et al. ClinPhen extracts and prioritizes patient phenotypes directly from medical records to expedite genetic disease diagnosis. Genet. Med. 21:1585–1593 (2019).
