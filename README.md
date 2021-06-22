# Introduction
This repository contains the course materials for hands-on exercise of the "Variants Annotation and Phenotype Analysis" session in the Quantitative Genomics workshop at Columbia University on June 23-25, 2021.

# Preparation of computing environment
Both the lectures and hands-on exercises will be taught via Zoom video conference online. To ensure the cross-platform compatibility, we will only use Rstudio Cloud to implement tools that are developed in Perl and Python.

## 0. Sign up
Students need to go to [RStudio Cloud](https://rstudio.cloud/) to create your own account before the lectures: 
1. click 'Sign up' on the top-right corner
<img src="https://user-images.githubusercontent.com/16017780/122824588-de42e300-d2ae-11eb-8999-3775f83568ef.png" width="400" height="200">

2. select "Free" and click "Sign up" at the bottom-right corner. 
<img src="https://user-images.githubusercontent.com/16017780/122824608-e8fd7800-d2ae-11eb-9f14-b1ded8d1e03a.png" width="400" height="200">

3. After you input your information, click "Sign up". 
<img src="https://user-images.githubusercontent.com/16017780/122824633-f0248600-d2ae-11eb-9bd1-a72b8f03383b.png" width="200" height="300">

## 1. Be familiar with RStudio
If you logout after signing up, you can go to [RStudio Cloud](https://rstudio.cloud/) and login with your username and password. After you are added to "QuantitativeGenomics2021", you can go to "QuantitativeGenomics" spaces on the left, and then click "New project" to create your projects for practice as shown below. It will take ~1 minute to deploy and prepare the project for you. 
![image](https://user-images.githubusercontent.com/16017780/122825757-58279c00-d2b0-11eb-840a-461f8699e1d9.png)

By default, you will be in "Console" as shown below where you can run R code.
![image](https://user-images.githubusercontent.com/16017780/122825824-6e355c80-d2b0-11eb-8f8e-14af92192188.png)

You can also click "Terminal" so that you can run linux commands.
![image](https://user-images.githubusercontent.com/16017780/122826165-cec49980-d2b0-11eb-9076-5461b0cd4ee2.png)

During the exercise, you can feel free to swith to Terminal when you need to run linux commands and to Console when you need to run R code.

# Basic Linux commands

After login to Rstudio Cloud, you already have terminal access by clicking 'terminal' button and can run all basic Linux commands. If you are not familiar with basic Linux commands, you can follow one simple tutorial to learn several commands: https://maker.pro/linux/tutorial/basic-linux-commands-for-beginners. Some of the  commands that we will use in the exercise include `ls`, `pwd`, `cd`, `mv` and `wget`.

# Installation of software tools and data sets

## Install ANNOVAR

### 1. Install ANNOVAR

Typically you will go to the [ANNOVAR website](http://annovar.openbioinformatics.org), fill in a registration form, and download the package there. For this exercise, we already prepared a ZIP file that contains a "compact" version of ANNOVAR and necessary library files, to make it easier for users. To make it easier to manage files and directories, you can create a new directory, then enter this new directory (type `mkdir genomics_exercise` followed by `cd genomics_exercise`). To confirm which directory you are in, you can type `pwd`.

Next, you can just download the ZIP file for this class by the command `wget https://github.com/WGLab/Workshop_Annotation/releases/download/v1.0.1/exercise1.tar.gz`. The Linux command `wget` essentially downloads a file from a given URL and saves the file to your computer. Because this file contains several annotation databases, its size is around 500Mb and it may take a while to download it. To unzip the file, you can dirctly using `tar -xvf exercise1.tar.gz` to unzip the downladed file. You will see from the messages in screen that several files are extracted from the zip file.

### 2. Run ANNOVAR on a small VCF file

Type `cd exercise1` to enter the `exercise1` directory. The sub-folder `humandb` folder already contains several annotation databases for human genome that we will use in our exercise. (Note that users can find more annotation databases [here](https://doc-openbio.readthedocs.io/projects/annovar/en/latest/user-guide/download/#-for-filter-based-annotation). Run `chmod +x *.pl` to change all perl programs to executable files.

```
perl table_annovar.pl example/ex2.vcf humandb/ -buildver hg19 -out myanno -remove -protocol refGeneWithVer,cytoBand,gnomad211_exome -operation g,r,f -nastring . -vcfinput -polish
```

After that, you will find the result files `myanno.hg19_multianno.txt` and `myanno.hg19_multianno.vcf`.

In the command above, we specified three protocols, including RefGene annotation (a gene-based annotation), cytogenetic band annotation (a region-based annotation), and allele frequency in gnoMAD version 2.1.1 (a filter-based annotation). These three types of annotation are indicated as `g`, `r` and `f` in the -operation argument, respectively. We also specify that dots ('.') be used to indicate lack of information, so variants that are not observed in the database will have '.' as the annotation. The `-vcfinput` argument specifies that the input file is in VCF format.

If you have Excel installed, you can open the `hg19_multianno.txt` file by Excel and examine the various tab-delimited fields.

### 2. Run ANNOVAR on a human exome

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


### 3. Results visualization

In the `exercise1` folder, run `pwd Results_Visualization.R` to obtain the path location of `Results_Visualization.R` file and copy this path. Open `Results_Visualization.R` in Rstudio by clicking open file, paste the path of to file name and select `Results_Visualization.R`.

<img src="https://user-images.githubusercontent.com/11565618/122840830-dee87300-d2c8-11eb-9796-997e00daa0b9.JPG">

Click session to setup the work directory to source file location.

<img src="https://user-images.githubusercontent.com/11565618/122842022-8023f900-d2ca-11eb-92ff-aeee031a44d0.JPG">

Then, run R code in `Results_Visualization.R` to visualize the results.

Check variant distribution across chromesomes:
```{r}
#load data
res <- read.table("proband.annovar.hg19_multianno.txt", fill=T, header=T, sep="\t")

#visualize variant frequency
attach(mtcars)
par(mar=c(5.1, 4.1, 4.1, 2.1),mfrow=c(1,1))
table <- prop.table(table(res$Chr))
chrlist <- paste0("chr",1:22)
chrlist <- c(chrlist, "chrX")
barplot(table[chrlist], ylab="Variant frequency", las=2)
````

Check allele frequency distribution between non-synonymous, synonymous and intronic variants:
```{r}
#visualize allele frequency
par(mar=c(5.1, 4.1, 4.1, 2.1),mfrow=c(3,1))
af_syn<-res[res$ExonicFunc.refGeneWithVer=="synonymous SNV",]$AF
hist(as.numeric(af_syn), na.rm=T, ylab="Frequency", xlab="Allele frequency", main="Synonymous SNV")

af_nonsyn<-res[res$ExonicFunc.refGeneWithVer=="nonsynonymous SNV",]$AF
hist(as.numeric(af_nonsyn), na.rm=T, ylab="Frequency", xlab="Allele frequency", main="nonSynonymous SNV")

af_intron<-res[res$Func.refGeneWithVer =="intronic",]$AF
hist(as.numeric(af_intron), na.rm=T, ylab="Frequency", xlab="Allele frequency", main="Intronic")
```

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

### 4. Run ANNOVAR to analyze a new strain of SARS-CoV-2

Suppose we sequenced a patient with COVID-19 and performed alignment, filtering, read assembly, and generate variant calls (the calls are generated by comparing to reference genome NC045512.2). We want to evaluate what effects that the virus mutations in this patient may have.

You can prepare a simple text file called `ex3.avinput` with the following mutations: 

```
NC_045512v2     29095   29095   C       T
NC_045512v2     26144   26144   G       T
NC_045512v2     28144   28144   T       C
```

This format is referred to as the avinput format, and it is a simple tab or space delimited file, with each variant per line. There can be many columns per line, but the first five columns in each line represent chr, start, end, reference allele and alternative allele, respectively.

Then you can just run `perl table_annovar.pl example/ex3.avinput sarscov2db -build NC_045512v2 -protocol avGene -operation g -out ex3` to annotate these mutations and the results will be stored in ex3.NC_045512v2_multianno.txt. 

Alternatively, since you are using only one single type of annotation (gene-based annotation), you may also do `perl annotate_variation.pl example/ex3.avinput sarscov2db/ -build NC_045512v2 -dbtype avGene -out ex3` to annotate these mutations. Examine the output file `ex3.exonic_variant_function` to see what proteins these mutations affect, and what amino acid changes that they cause.


## Phen2Gene: phenotype-based gene prioritization tool from HPO IDs or clinical notes on patients

In the next exercise, we will use a software tool called Phen2Gene to prioritize genes based on clinical phenotypes of patients with Mendelian diseases.

There are three ways to run Phen2Gene: download and run it locally (need more space), using API and using Phen2Gene website. Below, the last 2 ways of running Phen2Gene are showns.

### Using Phen2Gene API.
1. Go to Terminal, and run `curl -i -H "Accept: application/json" -H "Content-Type: application/json" "https://phen2gene.wglab.org/api?HPO_list=HP:0002459" | tail -n 1 > output.txt`
where you generate JSON output in `output.txt`

2. Go To Console, and run
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



### Using the Phen2Gene Website to assess the ANKRD11 case

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

### Run ClinPhen

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

3. Run ClinPhen
```
clinphen m_notes.txt
```
You will get the results from ClinPhen. You can also type `clinphen --help` if you are interested in more options.

### Run AMELIE

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

### Phen2Gene vs AMELIE and GADO

In all honesty though, we recommend Phen2Gene because it is faster:

| Tool              | Phen2Gene (API) | Phen2Gene (CLI) | Phenolyzer (ver. 0.2.0, CLI) | AMELIE 2 (API) | GADO (API) | GADO (CLI) |
|-------------------|-----------------|-----------------|------------------------------|----------------|------------|------------|
| Median time (s)   | 0.94            | 0.96            | 504.54                       | 519.97         | 1.52       | 5.89       |
| Minimum time (s)  | 0.17            | 0.51            | 187.97                       | 198.35         | 0.74       | 3.43       |
| Maximum time (s)  | 2.96            | 1.92            | 1021.54                      | 852.70         | 4.15       | 10.3       |

and more accurate:

![image](https://user-images.githubusercontent.com/6568964/122856907-a1dea980-d2e5-11eb-96a9-d11990d75aeb.png)

### Running PhenCards

Click [this link](PhenCards.md) to learn how you can use PhenCards.
