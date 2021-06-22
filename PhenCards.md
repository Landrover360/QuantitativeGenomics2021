# What is PhenCards?

PhenCards (https://phencards.org) is a database and web server intended as a one-stop shop for previously disconnected biomedical knowledge related to human clinical phenotypes. Users can query human phenotype terms or clinical notes. PhenCards obtains relevant disease/phenotype prevalence and co-occurrence, drug, procedural, pathway, literature, grant, and collaborator data. PhenCards recommends the most probable genetic diseases and candidate genes based on phenotype terms from clinical notes. PhenCards facilitates exploration of phenotype, e.g., which drugs cause or are prescribed for patient symptoms, which genes likely cause specific symptoms, and which comorbidities co-occur with phenotypes.


# Trying to use PhenCards? Why not watch the video?

[![PhenCards tutorial](https://res.cloudinary.com/marcomontalbano/image/upload/v1624340098/video_to_markdown/images/youtube--9FT4pFgeA08-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=9FT4pFgeA08&ab_channel=JimHavrilla "PhenCards tutorial")

## If you don't feel like watching the video, here's an easy, semi-practical example.

Let's try the example with patient notes we use in the PhenCards paper.  So you can find real clinical free text from deidentified patients looking through journals like AJHG and Cold Spring Harb Mol Case Stud.  This example comes from the latter.  Go to [the paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4990810/) and copy and paste the text from the *_entire_* section "Clinical Presentation and Family History" (minus Figures, of course):

![image](https://user-images.githubusercontent.com/6568964/122875077-cb0d3300-d301-11eb-9d5f-d60a66e2c63a.png)

![image](https://user-images.githubusercontent.com/6568964/122875538-66060d00-d302-11eb-9672-e6a2c13d692e.png)

![image](https://user-images.githubusercontent.com/6568964/122875599-79b17380-d302-11eb-8523-25284d9b5b9a.png)

Go to [PhenCards](https://phencards.org) and click on the tab "Patient Notes."  This will utilize [Doc2HPO](https://impact2.dbmi.columbia.edu/doc2hpo/):

![image](https://user-images.githubusercontent.com/6568964/122875653-89c95300-d302-11eb-905c-b7b3dd9871ad.png)

Now click "Search" and you will see something like this:

![image](https://user-images.githubusercontent.com/6568964/122875757-ab2a3f00-d302-11eb-99d6-f182ca8d045e.png)

The highlighted terms are Human Phenotype Ontology [(HPO)](https://hpo.jax.org/) terms extracted by Doc2Hpo's Aho-Corasick algorithm implementation. Just like with the [Phen2Gene](https://phen2gene.wglab.org/) exercise from earlier, we can use this list of HPO terms to determine candidate genes.  In fact, Phen2Gene is integrated into PhenCards under the "Genes" section:

![image](https://user-images.githubusercontent.com/6568964/122875940-e7f63600-d302-11eb-9d6d-3c6a54c37480.png)

In this case, the causal gene is FGD1 which is the third ranked gene by Phen2Gene.  Further evidence on this page can lead you to this being the correct gene without any genetic information like variant data.  For example, we predict that the disease is most likely Aarskog-Scott syndrome (spoiler: it is):

![image](https://user-images.githubusercontent.com/6568964/122876121-1e33b580-d303-11eb-9846-e13733297264.png)

This information comes from HPO-linked disease databases [OMIM](https://omim.org/) and [Orphanet](https://www.orpha.net).  The higher the matching by Elasticsearch composite score to these linked diseases, the higher the score.  We can further explore these sites by clicking on the "Aarskog-Scott syndrome" link which takes us to the entry in the HPO website:

![image](https://user-images.githubusercontent.com/6568964/122876333-62bf5100-d303-11eb-93d4-175e31da53cd.png)

Here we find our friendly databases OMIM and Orphanet both have information on this. Let's click on the "OMIM" outlink (have to click the ID on the last page and "OMIM" on this page) and follow it:

![image](https://user-images.githubusercontent.com/6568964/122876439-85ea0080-d303-11eb-8339-e66525b41875.png)

![image](https://user-images.githubusercontent.com/6568964/122876548-a1550b80-d303-11eb-83f7-0d5654696da3.png)

Here we can see FGD1 is clearly the causal gene in OMIM.  Let's go back and double click on the two links to Orphanet now...

![image](https://user-images.githubusercontent.com/6568964/122876681-cba6c900-d303-11eb-98a3-bcdf27552791.png)

![image](https://user-images.githubusercontent.com/6568964/122876722-d5c8c780-d303-11eb-9630-4dd2596ca119.png)

Scroll down the Orphanet page and click on the "Genes" section:

![image](https://user-images.githubusercontent.com/6568964/122876803-ebd68800-d303-11eb-9631-744ada3f6526.png)

![image](https://user-images.githubusercontent.com/6568964/122876865-fee95800-d303-11eb-85fe-d6c3f06eda43.png)

Now we found further evidence FGD1 is the causal gene.

### So what did we learn?

With just clinical free text or patient notes on an unidentified disease, PhenCards gives us several pieces of evidence to look through, candidate genes ranked by Phen2Gene, a candidate disease if not already known, and databases to look through to support a potential genetic etiology.  Furthermore, one can click on any of the term symptoms and search a wealth of information about them on PhenCards using its "Phenotype search" tab which is its primary function:

![image](https://user-images.githubusercontent.com/6568964/122877196-58ea1d80-d304-11eb-8ad1-0a4ad8b24925.png)

You will get a wealth of data including candidate genes for that term, drugs that could be prescribed for it and cause it, pathways it could be involved in (the symptoms of Aarskog-Scott syndrome can be used to find its pathways as well), collaborator, grant, and nonprofit information.  

![image](https://user-images.githubusercontent.com/6568964/122877430-98186e80-d304-11eb-8e83-5e48e8dd6d82.png)

If you wish to know more about it, watch the [video](https://www.youtube.com/watch?v=9FT4pFgeA08&ab_channel=JimHavrilla).

#### CITATIONS:

[PhenCards](https://genomemedicine.biomedcentral.com/articles/10.1186/s13073-021-00909-8): Havrilla, J. M., Liu, C., Dong, X., Weng, C. & Wang, K. PhenCards: a data resource linking human phenotype information to biomedical knowledge. Genome Med. 13, 91 (2021).
[Doc2Hpo](https://academic.oup.com/nar/article/47/W1/W566/5491745): Liu, C. et al. Doc2Hpo: a web application for efficient and accurate HPO concept curation. Nucleic Acids Res. 47, W566–W570 (2019).
[Phen2Gene](https://academic.oup.com/nargab/article/2/2/lqaa032/5843800): Zhao, M. et al. Phen2Gene: rapid phenotype-driven gene prioritization for rare diseases. NAR Genom Bioinform 2, lqaa032 (2020).
