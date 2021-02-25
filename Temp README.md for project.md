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
* Amazon CLI
* Boto3
* Matplotlib
* Mpl_toolkits
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

Our project focused on the following 11 States in the US: 
|States| | 
|---|---|
|Arizona|Montana |
|California|New Mexico |
|Colorado|Nevada|
|Idaho|Utah |
|Oregon|Washington |
|Wyoming||

2020 was the most active fire season in the Western United States’s recorded history. California had the single worst fire season in it’s history, while Arizona had the worst in a decade. Oregon had its most destructive fire season meanwhile Washington and Colorado had several of their all time largest wildfires. Overall 10.2 million acres of land went up in flame and 46 people lost their lives. 13,887 buildings were destroyed and the total cost is upwards of 19.88 billion USD. It is evident that fire is a clear and present danger in the western united states. 

The global atmospheric monitoring satellite Copernicus has recorded CO2 emissions from the 2020 fires and noted that “The fires are also emitting lots of smoke and pollution into the atmosphere; those in California and Oregon have already emitted far more carbon in 2020 than in any other year since CAMS records begin in 2003” - [CAMS monitors smoke release from devastating US wildfires | Copernicus](https://atmosphere.copernicus.eu/cams-monitors-smoke-release-devastating-us-wildfires). We decided to investigate the relationship between weather data (precipitation, temperatures, and drought) and the occurrence of fires, and to attempt building a model which would predict the destructive sizes of wildfire to help prevent the associated damage for our communities.

Following National Wildfire Coordinating Group's convention which groups fires into ranges of fire size based on the number of acres within the final fire perimeter, we chose to set up our model as a a multi-classification where class "A" corresponds to fires smaller than 0.01 acres, "B" - 0.225 acres, "C" - 10 acres, "D" - 100 acres , "E" - 300 acres , "F" - 1000 acres, and "G" - fires larger than that.

----------------
### Data

PUBCHEM_SID will need to be converted from SID > SMILES   

SMILES data is The simplified molecular-input line-entry systemThe simplified molecular-input line-entry system (SMILES) which is is a specification in the form of a line notation for describing the structure of chemical species using short ASCII strings.

SMILES data is not very readable for modeling, though so we will need to convert it further to a Morgan Fingerprint Hash.  

Fingerprints in data are a fingerprinting algorithm is a procedure that maps an arbitrarily large data item (such as a computer file) to a much shorter bit string, its fingerprint, that uniquely identifies the original data for all practical purposes just as human fingerprints uniquely identify people for practical purposes. In the case of molecular data, molecules have unique "ring" fingerprints.

This family of fingerprints, better known as circular fingerprints, is built by applying the Morgan algorithm to a set of user-supplied atom invariants. The Morgan algorithm converts a single molecule into a fixed number of binary bits (e.g. 2048). Each bit corresponds to the presence/absence of chemical substructures up to a specified radius (e.g. 2).

Rdkit casts fingerprints as a unique type of object, which is not readable by our models. To get around this, we convert the Hashed Morgan Fingerprint to a Bit Vector. Bit Vectors are just arrays of bits. 

These bits are converted two more times - once more to a Bit String which are much easier to work with. Then each bit is cast as a feature (meaning they each get their own column in the dataframe). In the end, each element has each bit cast as a feature of that molecule for modeling.

![](/visuals/fire_size_vs_temp_precip_by_month.png)

---------
### Modeling

The project ultimately uses two main models. Neural network for predictive power and Random Forest Classifier for feature importance. We optimized the neural network on recall score focusing on true positive rate and capturing large fires over small fires. Large fires being more destructive and being more in line with the scope of the project at the expense of smaller fires. The final chosen neural network model topology optimizes recall over accuracy. When we focused on accuracy, we were predicting the majority class (small fires) over the minority (large fires) which missed the most destructive wildfires. 

To improve our models, we employed the modeling technique of bootstrapping which gave us a more normal distribution of wildfire classes. This way, we were able to capture our larger fires. It greatly improved recall which is ultimately the target we wanted to pursue, as this helped us predict  larger and more destructive wildfires. 

The second modeling breakthrough we had was harnessing geospatial data through KMeans clustering of longitude and latitudinal data. We then One-Hot-Encoded it which gave us a sparse matrix that was the most important predictive element of our model. We believe that this is because terrain features matter immensely when determining the potential size of a wildfire. (See confusion matrix below)

The third major breakthrough was the trailing averages as noted in the summary above. Adding that data essentially doubled our recall scores for medium sized fires, which improved our overall model recall. Next, since our Neural Network is a blackbox model, we were not able to glean as much insight into features. We utilized Random Forest Classification to compliment insights from our Neural Network model by providing top features and weights.

**Top 3 features (excluding location clusters):**
|Feature|Importance|Feature Description|  
|---|---|---|  
|tavg_t3m|0.05085|Average Temperature Past 3 Months|  
|pcp|0.04720|Month Precipitation|  
|tavg_t6m|0.04633|Average Temperature Past 6 Months|


![](/visuals/confusion_matrix_fire.png)

---------------------------
### Conclusions

1. Wildfires are extremely complex phenomena. While the NOAA data offered a set of independent variables which fairly comprehensively described weather history, we were not able to include in our model other important factors which also affect final fire size, such as wind or terrain features (e.g. land cover or incline).

2. Switching our target variable from a continuous one (fire size, in acres) to a multi-class problem improved the score from an R2 of under 10% to accuracy of over 60%. Further, re-defining the problem as a binary classification of "large" vs "small" fires drove accuracy up to 62-77%, depending on the threshold chose to delineate between the two classes.

3. Because our goal was to improve preparedness and help contain damage from wildfires without putting efforts into preventing fires which weren't likely to spread, our ultimate focus was on increasing the recall of our model, and moreover - to increase its recall with regards to large fires.

4. Each of the seven-class classifications requires sklearn's estimators to perform 7 x (7-1) / 2 = 21 separate classifications - fitting some of the estimators we evaluated (e.g. SVM) was very computationally expensive.

5. Despite being computationally expensive, we were able to create a model that gave us a significant increase in our recall score. We can confidently predict very large fires and some mid range fires with our current model.

--------------------------------
**Classes and model improvements:**
|Class| Size Acres|Baseline (% of dataset)|Final Model|
|---|---|---|---|
|A| >0<=0.25|62%|59%|
|B|0.26-9.9|29%|1%|
|C|10.0-99.9|6%|20%|
|D|100-299|2%|50%||
|E|300 to 999|1%|58%|
|F|1000 to 4999|2%|59%|
|G|5000+|0.07%|86%|

-----------------------
### Recommendations

For further research we recommend extracting NOAA wind data such as wind speed, gusts and potentially wind directionality. Furthermore, vegetation data and environmental composition data which is available on Google’s Earth Engine’s LANDFIRE databases potentially play a significant part in telling a deeper story on a wildfire destructive ability. Merging those features into our datasets was unfortunately out of reach due to the time restraints thus we did not include them. However we believe these features merged into our current dataset could expand in a worthwhile manner.

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
|Feature|Description|
|---|---|
|pcp|Precipitation|
|tavg|Average Tempurature|
|pdsi|Hydrological Drought Index|
|phdi|Palmer Hydrological Drought Index|
|zndx|Palmer Zindex Data|
|pmdi|Palmer Drought Severity Index|
|sp02 - sp24|Precipitation|
|tmin|Minimum Temperature|
|tmax|Maximum Temperature|
|month_2 - month_12|Dummy Month Variables|


