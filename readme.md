# SALT-C

SALT-C (Sandardization Algorithm for Laboratory Test -  Categorical data) algorithm process categorical laboratory data into standardized format. The overall procedure is described below. The following subsections will describe each method in detail.

## Lab tests and value sets 
Measurement_concept_id, Measurement_source_value, Value_as_concept_id, Value_source_value are available at [labdata_valuesets].


## Sample data

We created and loaded sample data of categorical laboratory test results. you can find this at data folder. 

## Process of SALT-C

### Data Preparation

The SALT-C algorithm is designed to extract categorical laboratory data from database or CSV file, and preprocesses the extracted data including converting to lowercase, removing space from both sides, and restoring abbreviations of urinalysis data. We designed the SALT-C algorithm that preprocesses urinalysis data using regular expression prior to the similarity measure step to eliminate numeric features that do not fit into this algorithm, such as ‘2+ 150’ which represents the concept of ‘urine glucose test = ++’. For example, ‘150 2+’, ‘150 ++’, ‘2+’, ‘++’, ‘2+ 150’, ‘++ 150’ values are converted into ‘++’.

### Laboratory Tests Categorization

Classifier logic of SALT-C algorithm categorizes laboratory tests into predefined six groups. First, Classifier extracts group of the major values which covers over 95% of each laboratory test’s values. Second, if one or more values in the group of major values overlaps with the values of the predefined value sets, then each laboratory test is categorized into corresponding group. Classifier sorts laboratory test by searching value sets in the following order: Color value set, Urine dipstick valueset, Presence finding valueset, Bloodtype value set, Pathogeneses qualifier value set. We design classifier logic that searches for urinalysis-related value sets (Color value set, Urine dipstick qualifier value set) ahead of other value sets for following reasons:(1) More than 70% of total categorical laboratory results are urinalysis data and should be categorized into the color value set and the urine dipstick value set. (2) To prevent urinalysis test from being categorized into unsuitable group because there are three value sets that have ‘+’data-related synonyms as part of the value set, i.e., ‘+’ value of Urine dipstick qualifier value set, ‘positive’ value of Presence finding qualifier value set, ‘Rh positive’ value of Blood type value set. 

### Character-level Vectorization

In SALT-C algorithm, we choose vectorization scheme as character level embedding to represent laboratory data and calculate the similarity score between raw data and values of value sets. With this approach SALT-C algorithm keep a list of all possible characters instead of keeping a list of every word. This means that instead of keeping a list of every laboratory value, SALT-C algorithm keep a list of different characters and its counts which come from the alphabet[a-z], and special characters [ -, +, (, ), .].

### Data Cleaning Using Similarity Measure

After SALT-C algorithm classify laboratory test into 6 groups, it vectorize and calculate the similarity score between word and value of value sets. As a method of measuring the similarity, we used and compared cosine similarity, Euclidean distance, levenshtein distance, and hybrid method which is a combination of cosine similarity and Euclidean distance.

[labdata_valuesets]:https://github.com/rpmina/SALT_C/blob/master/labdata_valuesets/labtest_valueset.xlsx
