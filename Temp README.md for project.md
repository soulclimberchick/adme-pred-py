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

----------
### Tech Stack
This Project is created with:
* RDKit
* Matplotlib
* Keras
* Python3
* Seaborn
* Sklearn
* Sqlite
* Statsmodels
* Streamlit
* Tensorflow

---------
### Problem Statement

SwissADME includes a Cytochrome P450 inhibition model in their output, their model has outputs for five specific enzymes. 

|CYP450 Enzyme| | 
|---|---|
|cyp2c19||
|cyp2c9||
|cyp1a2||
|cyp2d6||
|cyp3a4||

This superfamily of isoenzymes are responsible for metabolizing drugs 

![](/visuals/fire_size_vs_temp_precip_by_month.png)

--------
### Summary

This project focused on the following 5 Cytochromes in the P450 block: 


----------------
### Data

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






![]()

---------------------------
### Conclusions


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
|2|sbonds |number of single bonds|
|3|dbonds|number of double bonds|
|4|tbonds|number of triple bonds|
|5|abonds|number of aromatic bonds|
|6|GetMolWt|molecular weight|
|7|NumAtoms|number of atoms|
|8|NumHvyAtoms|number of heavy atoms|
|9|AP|aromatic portion|
|10|NumAcceptor|number of H-bond acceptors|
|11|NumDonor|number of H-bond donors|
|12|Num Carbon|number of carbon atoms|
|13|NumHetero|number of heteroatoms|
|14|NumAromatic|number of aromatic atoms|
|15|NumRotors|number of rotatable bonds|
|16|NumRing|number of rings|
|17|TPSA|topological surface area|
|18|MR|molecular refractivityty|
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
|35|logP|OpeBabel log <em>P</em>|
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


