
# Cytochrome P450 inhibition Machine Learning Modeling

## Table of contents
* [Tech Stack](###Tech-Stack)
* [Problem Statement](###Problem-Statment)
* [Summary](###Summary)
* [Data](###Data)
* [Modeling](###Modeling)
* [Conclusions](###Conclusions)
* [Recommendations](###Recommendations)
* [Next Steps](###Next-Steps)
 * [Resources:](###Resources:)

----------
### Tech Stack
This Project is created with:

* Anaconda package management
* Jupyter Notebooks
* Keras
* RDKit
* Matplotlib
* MolVS
* Numpy
* Pandas
* Python3
* Seaborn
* Sklearn
* Statsmodels
* Tensorflow
* tqdm

---------
### Problem Statement

The 5 Cytochrome P450 block of enzymes we are talking about today are a part of a larger block of more than 50 different enzymes. These 5 are responsible for breaking down about 30% to 90% of drugs depending on the source of information. Genetics affect the way they metabolize proteins.
Because of this, there are HUGE implications for prescribing drugs that are ineffective or possibly toxic for the individual patient.

The "superblock" can contain 5-6 enzymes depending on  the source. We will be working with the following 5 enzymes based on SwissADMEs dataset and process with hopes of adding additional important cytochromes in the future:


|CYP450 Enzyme| | 
|---|---|
|cyp2c19||
|cyp2c9||
|cyp1a2||
|cyp2d6||
|cyp3a4||

![](/visuals/)

--------
### Summary

Each person has unique DNA characteristics tha affect how these enzymes behave. A person can have duplicates and variations in that particular area of their genome. Because of this there are multiple categories each enzyme can fall under for each individual:

* Super - metabolizes quicker than average
* Intermediate -  metabolizes somewhat  quicker than average
* Normal -  metabolizes at an average rate
* Slow -  metabolizes slower than average

The drugs and foods we consume also have a considerable impact on these enzymes' abilities to process compounds. 
Compounds can inhibit the enzymes and blunt their ability to do their job, and other compounds can induce these enzymes, meaning they increase the rate of hepatic metabolism. Lastly, many drugs are just considered substrates of (a) particular molecule(s), which essentially means that it bonds to the enzyme and creates the chemical reaction for which the drug is broken down into its constituent chemical compounds to be broken down by the body (and become useful for the drugs purpose). 

This is important for numerous reasons, one being - if an individual is on a cocktail of multiple drugs, each one affecting these enzymes  in one way or the other, on top of the enzymes potentially having variable levels of metabolism rates on the genetic level. This is a lot to take in so let's start with an example from drugguide.com. They do a decent quick example:

"For example, several antidepressants (paroxetine [Paxil] and fluoxetine [Prozac]) are inhibitors of metabolism when given with drugs metabolized through the CYP2D6 enzyme, such as haloperidol (Haldol), metoprolol (Lopressor), and hydrocodone. Thus, the therapeutic response can be accentuated. Medications that inhibit the CYP3A4 enzyme, such as amiodarone and antifungals, can affect the therapeutic response of fentanyl, alprazolam (Xanax), and numerous statins; as a result, the effect of these drugs can be enhanced leading to potential toxic levels." <sup> https://www.drugguide.com/ddo/view/Davis-Drug-Guide/109519/all/The_Cytochrome_P450_System:_What_Is_It_and_Why_Should_I_Care_ </sup>

So we can see that a combination of medications can become complicated really quickly, and have the potential to become dangerous. By using machine learning models to predict inhibition action of an ezyme on particualr molecular structures   of drugs, treatment can be optimized for safety and effecacy for each patient. Just think of what can be done if we combine inhibition prediction with substrates, and enzyme activity levels. The possibilities are endless!

SwissADME includes a Cytochrome P450 inhibition model in their output, their model has outputs for five specific enzymes. 

|CYP450 Enzyme| | 
|---|---|
|cyp2c19||
|cyp2c9||
|cyp1a2||
|cyp2d6||
|cyp3a4||

This is a great resource as a quick reference for determining if a known drug is a Inhibitor, Inducer and/or Substrate:
https://www.d.umn.edu/~jfitzake/Lectures/DMED/TAA/Q_A/CYP450InteractionTable.htm

The FDA also has great information on this:
https://www.fda.gov/drugs/drug-interactions-labeling/drug-development-and-drug-interactions-table-substrates-inhibitors-and-inducers

----------------
### Data
The data used in this project was curated by the SwissADME team and is available on PubChem, Metrabase and other repositories: https://pubchem.ncbi.nlm.nih.gov/bioassay/1851

This dataset includes 17,143 tested substances, 16,560 tested compounds agaist the 5 enzymes noted in the summary.
The data that we used had to undergo many transformations to get it to a modeling-friendly form:

PUBCHEM_SID will need to be converted from SID > SMILES   

SMILES data is The simplified molecular-input line-entry systemThe simplified molecular-input line-entry system (SMILES) which is is a specification in the form of a line notation for describing the structure of chemical species using short ASCII strings.

SMILES data is not very readable for modeling, though so we will need to convert it further to a Morgan Fingerprint Hash.  

Fingerprints in data are a fingerprinting algorithm is a procedure that maps an arbitrarily large data item (such as a computer file) to a much shorter bit string, its fingerprint, that uniquely identifies the original data for all practical purposes just as human fingerprints uniquely identify people for practical purposes. In the case of molecular data, molecules have unique "ring" fingerprints.

This family of fingerprints, better known as circular fingerprints, is built by applying the Morgan algorithm to a set of user-supplied atom invariants. The Morgan algorithm converts a single molecule into a fixed number of binary bits (e.g. 2048). Each bit corresponds to the presence/absence of chemical substructures up to a specified radius (e.g. 2).

Rdkit casts fingerprints as a unique type of object, which is not readable by our models. To get around this, we convert the Hashed Morgan Fingerprint to a Bit Vector. Bit Vectors are just arrays of bits. 

These bits are converted two more times - once more to a Bit String which are much easier to work with. Then each bit is cast as a feature (meaning they each get their own column in the dataframe). In the end, each element has each bit cast as a feature of that molecule for modeling.

![](data/visuals/)

---------
### Modeling

In the interest of time, train_test_split used to create training, testing, and valiation datasets. 

The models chosen are: a Markov Chain Classifier Neural Netowork employed using Keras and SciKitLearn, and a SciKitLearn implementation of SVC SVM model utilizing gridsearching for parameter optimization around both accuracy and recall. Both models performed well, though we will likely stick with the SVM model as that is the model used for SwissADME. 

The models were deployed on various datasets:
Each cytochrome enzyme with fingerprint data, and again with engineered feature data. The models were also deployed on each of the full merged datasets for fingerprint data and engineered feature data.


![]()

---------------------------
### Conclusions

Our models can predict 

 Currently both models we are using are showing promising accuracy scores on par with or better than SwissADMEs model scores, though there is more work to be done on the feature set to make parity, and a worry of overfitting. 



--------------------------------
**Classes and model improvements:**


-----------------------
### Recommendations

-----------------------
### Data Dictionary 

**Molecular Substrate Features:**
Source: https://static-content.springer.com/esm/art%3A10.1038%2Fsrep42717/MediaObjects/41598_2017_BFsrep42717_MOESM91_ESM.pdf  


|Index|Variable|Description|
|---|---|---|
|1|nF |number of fluorine atoms|
|2|single_bond *n_bonds|number of single bonds|
|3|dbonds *n_bonds|number of double bonds|
|4|tbonds *n_bonds|number of triple bonds|
|5|abonds *n_bonds|number of aromatic bonds|
|6|mol_weight|molecular weight|
|7|n_atoms|number of atoms|
|8|n_heavy_atoms|number of heavy atoms|
|10|h_bond_acceptors|number of H-bond acceptors|
|11|h_bond_donors|number of H-bond donors|
|12|n_carbons|number of carbon atoms|
|13|n_heteroatoms|number of heteroatoms|
|14|n_aromatic_atom|number of aromatic atoms|
|15|n_rot_bonds|number of rotatable bonds|
|16|n_rings|number of rings|
|17|tpsa|topological surface area|
|18|molar_refractivity|molecular refractivityty|
|35|logp|OpeBabel log <em>P</em>|

## Additional features added for functionality to get other features:
|Index|Variable|Description|
|---|---|---|
|#|conformer |The class to store 2D or 3D conformation of a molecule|
|#| stereo| Returns the stereo configuration of the bond as a BondStereo|
|#| is_conjugated| Returns whether or not the bond is considered to be conjugated.|
|#| bond_type| Returns the type of the bond as a BondType|

## To be added 
|Index|Variable|Description|
|---|---|---|
|9|AP|aromatic portion|
|19|MlogpCX|weighted sum of carbon and halogen atoms|
|20|MlogpNO|total number of nitrogen and oxygen atoms|
|21|MLogpUB|number of undsaturated bonds|
|22|MLogpRNG|presence of ring structures|
|23|MLogpNO2|number of nitro groups|
|24|MLogpNCS|presence of thiocyanate or isothiocynanate|
|25|MLogpQN|presence of quarternary nitrogen or N-oxide|
|26|MLogpALK|presence of alane, alkene cyclocalckane or cycloalkene|
|27|MLogpHB|presence of intramolecular H-bond|
|28|MLogpPOL|number af aromatic substituents|
|29|MLogpBLM|presence of beta-lactam|
|30|MLogpAMP|presence of amphoteric property|
|31|MLogpPRX|proximity effect of nitrogen and oxygen atoms|
|32|mlogp|MLOGP|
|33|mhlogp|NC+NHET log <em>P</em>|
|34|alogp|WLOGP|
|36|NumSpiro|number of spiro groups|
|37|NumBridge|number of ringbridging atoms|
|38|NumStero|number of stereocenters|
|39|NumMacrocycle|number of macrocycles|
|40|sizePenalty|size penalty|
|41|macrocyclePenalty|macrocycle penalty|
|42|stereoComplexity|stereo complexity|
|43|ringComplexity|ring complexity|
|44|complexityPenalty|complexity penalty|
|45|ilogp|IGLOP|
|46|xlogp3|XLOGP3|
|47|silicos_logP|Filter-IT log <em>P</em>|
|48|logSwE|ESOL log <em>S</em>|
|49|logSwA|Ali log <em>S</em>|
|50|silicos_logS|Filter-IT log <em>S</em>|









<br>

**Cytochrome P450 isoenzymes**

### Resources:

#### Modern treatment diversity issues:

https://www.sciencedaily.com/releases/2008/10/081015132108.htm
Plataforma SINC. (2008, October 17). Medical Textbooks Use White, Heterosexual Men As A 'Universal Model'. _ScienceDaily_. Retrieved February 25, 2021 from 

https://press.uchicago.edu/Misc/Chicago/213095.html
Steven Epstein  
Inclusion: The Politics of Difference in Medical Research
©2007, 416 pages  
Cloth $29.00 ISBN: 978-0-226-21309-5 (ISBN-10: 0-226-21309-9)

https://www.ncbi.nlm.nih.gov/books/NBK220337/
Institute of Medicine (US) Committee on Understanding and Eliminating Racial and Ethnic Disparities in Health Care; Smedley BD, Stith AY, Nelson AR, editors. Unequal Treatment: Confronting Racial and Ethnic Disparities in Health Care. Washington (DC): National Academies Press (US); 2003. RACIAL AND ETHNIC DISPARITIES IN DIAGNOSIS AND TREATMENT: A REVIEW OF THE EVIDENCE AND A CONSIDERATION OF CAUSES. 

https://www.theguardian.com/lifeandstyle/2019/nov/13/the-female-problem-male-bias-in-medical-trials
_This is an edited extract from  [Pain and Prejudice](https://guardianbookshop.com/pain-and-prejudice-9780349424552.html)  by Gabrielle Jackson, published by Little, Brown (£14.99 rrp). To order a copy for £11.24 with free UK p&p, go to guardianbookshop.com or call 020-3176 3837_
  

#### General resources on CYP450 gene expression/affect on drug metabolism

https://www.aafp.org/afp/2007/0801/p391.html
TOM LYNCH, PharmD, AMY PRICE, MD, Eastern Virginia Medical School, Norfolk, Virginia
Am Fam Physician._ 2007 Aug 1;76(3):391-396.

https://www.nature.com/articles/6500462
Tomalik-Scharte, D., Lazar, A., Fuhr, U. _et al._ The clinical role of genetic polymorphisms in drug-metabolizing enzymes. _Pharmacogenomics J_  **8,** 4–15 (2008). https://doi.org/10.1038/sj.tpj.6500462

https://www.nature.com/articles/6500285
Ingelman-Sundberg, M. Genetic polymorphisms of cytochrome _P_450 2D6 (CYP2D6): clinical consequences, evolutionary aspects and functional diversity._Pharmacogenomics J_  **5,** 6–13 (2005). https://doi.org/10.1038/sj.tpj.6500285

https://www.nature.com/articles/gim2007123
Thakur, M., Grossman, I., McCrory, D. _et al._ Review of evidence for genetic testing for CYP450 polymorphisms in management of patients with nonpsychotic depression with selective serotonin reuptake inhibitors. _Genet Med_  **9,** 826–835 (2007). 
https://doi.org/10.1097/GIM.0b013e31815bf98f

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1312247/
Ogu, C C, and J L Maxa. “Drug interactions due to cytochrome P450.” _Proceedings (Baylor University. Medical Center)_ vol. 13,4 (2000): 421-3. doi:10.1080/08998280.2000.11927719

https://www.cancernetwork.com/view/cytochrome-p450-microenzyme-system-and-targeted-therapies
February 24, 2009
Gail M. Wilkes, MS, RNC
Oncology Nurse Edition, ONCOLOGY Nurse Edition Vol 23 No 2, Volume 23, Issue 2

FDA:
https://www.fda.gov/drugs/drug-interactions-labeling/drug-development-and-drug-interactions-table-substrates-inhibitors-and-inducers

https://www.d.umn.edu/~jfitzake/Lectures/DMED/TAA/Q_A/CYP450InteractionTable.htm
  

#### SwissADME - Data gathering:

http://www.swissadme.ch

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2912882/
Mishra NK, Agarwal S, Raghava GP. Prediction of cytochrome P450 isoform responsible for metabolizing a drug molecule. _BMC Pharmacol_. 2010;10:8. Published 2010 Jul 16. doi:10.1186/1471-2210-10-8

https://www.nature.com/articles/nbt.1581
Veith, H., Southall, N., Huang, R. _et al._ Comprehensive characterization of cytochrome P450 isozyme selectivity across chemical libraries. _Nat Biotechnol_  **27,** 1050–1055 (2009). https://doi.org/10.1038/nbt.1581

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4477067/
J Cheminform. 2015; 7: 31. Published online 2015 Jun 23. doi: [10.1186/s13321-015-0083-5](https://dx.doi.org/10.1186%2Fs13321-015-0083-5)
PMCID: PMC4477067
PMID:   26106450

https://pubmed.ncbi.nlm.nih.gov/26400175/
Kim S, Thiessen PA, Bolton EE, et al. PubChem Substance and Compound databases. _Nucleic Acids Res_. 2016;44(D1):D1202-D1213. doi:10.1093/nar/gkv951

#### Data detangling:

https://depth-first.com/articles/2019/01/11/extended-connectivity-fingerprints

https://www.rdkit.org/docs/RDKit_Book.html/

https://www.researchgate.net/publication/51509126_Transfer_learning_for_cytochrome_P450_isozyme_selectivity_prediction
August 2011 Journal of Bioinformatics and Computational Biology 9(4):521-40
DOI:  [10.1142/S0219720011005434](https://www.researchgate.net/deref/http%3A%2F%2Fdx.doi.org%2F10.1142%2FS0219720011005434)


https://www.cheminformania.com/fetch-the-sherif-i-found-a-fingerprint/
Esben Jannik Bjerrum  November 23, 2015/  [Blog](https://www.cheminformania.com/category/blog/), [Cheminformatics](https://www.cheminformania.com/category/cheminformatics/), [RDkit](https://www.cheminformania.com/category/rdkit/)
  
  
#### Toxicity Research

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7431904/
Toxicol Res. 2021 Jan; 37(1): 1–23. PMCID: PMC7431904
Published online 2020 Aug 18. doi: [10.1007/s43188-020-00056-z](https://dx.doi.org/10.1007%2Fs43188-020-00056-z) PMID: [32837681](https://www.ncbi.nlm.nih.gov/pubmed/32837681)

https://academic.oup.com/jat/article/29/7/590/731303
Ming Jin, Susan B. Gock, Paul J. Jannetto, Jeffrey M. Jentzen, Steven H. Wong, Pharmacogenomics as Molecular Autopsy for Forensic Toxicology: Genotyping Cytochrome P450 _3A4*1B_ and _3A5*3_ for 25 Fentanyl Cases, _Journal of Analytical Toxicology_, Volume 29, Issue 7, October 2005, Pages 590–598, [https://doi.org/10.1093/jat/29.7.590](https://doi.org/10.1093/jat/29.7.590)

https://www.hindawi.com/journals/bmri/2015/971982/
Debendranath Dey, Sunetra Chaskar, Nitin Athavale, Deepa Chitre,  "Acute and Chronic Toxicity, Cytochrome P450 Enzyme Inhibition, and hERG Channel Blockade Studies with a Polyherbal, Ayurvedic Formulation for Inflammation",  _BioMed Research International_,  vol. 2015,  Article ID 971982,  9  pages,  2015.https://doi.org/10.1155/2015/971982

https://www.sciencedirect.com/science/article/pii/S1752928X16300051
Selma J.M. Eikelenboom-Schieveld, Yolande Lucire, James C. Fogleman, The relevance of cytochrome P450 polymorphism in forensic medicine and akathisia-related violence and suicide,
Journal of Forensic and Legal Medicine, Volume 41, 2016, Pages 65-71, ISSN 1752-928X, https://doi.org/10.1016/j.jflm.2016.04.003.

#### Modeling 

https://arxiv.org/pdf/1805.00784.pdf
arXiv:1805.00784v1 [cs.LG] 2 May 2018
Maren Awiszus, Bodo Rosenhahn Institut fu ̈r Informationsverarbeitung Leibniz Universita ̈t Hannover www.tnt.uni-hannover.de


https://doi.org/10.1007/3-211-27389-1_125
https://link.springer.com/chapter/10.1007%2F3-211-27389-1_125
Koutník J., Šnorek M. (2005) Neural Network Generating Hidden Markov Chain. In: Ribeiro B., Albrecht R.F., Dobnikar A., Pearson D.W., Steele N.C. (eds) Adaptive and Natural Computing Algorithms. Springer, Vienna. 


  

#### Design and images for presentation slides:

https://pubchem.ncbi.nlm.nih.gov/gene/CYP2C19/human(to get to rcsb for each molecule)

https://www.rcsb.org/3d-view/2HI4/1 (for 3d molecule images)

https://www.flaticon.com/

https://www.freepik.com/

https://slidesgo.com