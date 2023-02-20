# README File for RBFW3 Data
The article entitled "Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (*Malurus melanocephalus*) in the nonbreeding season" makes use of two data sets. The primary data set is the file "RBFW3_Anon_Data_210720.csv" and consists of focal behavioural observations. In the R code for this project, it is referred to as "RBFW." The data set "RBFW3_Anon_Opportunistic_Data_230116.csv" consists of opportunistic behavioural observations and is used to generate supplemental materials for the article. It is referred to as "RBFW_sup" in R code.

These two datasets are available in the "Data" GitHub folder that contains the present README file and have also been uploaded to Figshare using the following DOI links: 

The contents of these two data sets are further detailed below:

## Focal Data ("RBFW3_Anon_Data_210720.csv")
This is the dataset used in the main analyses in the manuscript "Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (Malurus melanocephalus) in the nonbreeding season". Each row corresponds to a behavioural observation on a single male fairywren.

Columns Describing Observations and Focal Male Characteristics:  
Observation_ID = unique identifier for each behavourial observation (or row)
Date = The date the observation was conducted in YYYY-MM-DD format
Year = The year the observation was conducted. 
Time = The time that the observation started in 24-hour format
Bird_ID = A unique identifier for each focal male (equivalent to color band combinations) 
Observer = Identity of the research who collected the data
Sex = Sex of the bird being observed (always male in this dataset) 
Plumage_score = Numerical scoring of a focal male's plumage colouration as per Karubian 2002
Plumage_group = Categories of plumage scores (DULL, MOULT, or BRIGHT) as per Karubian 2002. 
Age = Age of each focal male obtained through banding records
Age_exact = Either "exact" if we were able to determine a bird's precise age or "minimum" if only a minimum estimate (e.g., after second year) was possible

Behavioural Variables: 
The variables Alarm_calling, Allopreening, Calling, Chasing, Chipping, Courtship, Flying, Foraging, Other, Preening, Scolding, Singing, Sitting, U, and Vocalizing) provide the number of seconds during a focal observation in which a male was engaging in these behaviours. These behaviors are described in table S1 (an ethogram). Note that U in the dataset is equivalent to "Out of sight" in table S1. During some years of our study, certain behaviours were not considered. In these situations, these values are given as "NA" rather than an integer. In particular, only vocalizing (any vocalization other than alarm calling) was recorded in 2016 while more specific categories of vocalizations (e.g., calling chipping, scolding, and singing) were reported in 2017 and 2018. 

Observation_time = Duration of a focal observation in seconds
 
The variables Alarm_calling_prop, Chasing_prop, Courtship_prop, Flying_prop, Foraging_prop, Other_Prop, Preening_prop, Sitting_prop, and Vocalizing_Prop are calculated by dividing the number of seconds spent engaged in each behaviour (the value of the corresponding column without the "prop" suffix) by the Observation_time variable minus the U (time bird was not visible) variable. For example: 

Preening_prop = Preening/(Observation_time - U)



## Opportunistic data ("RBFW3_Anon_Opportunistic_Data_230116.csv")
This data set consists of the opportunistic behavioural observations described in the manuscript "Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (Malurus melanocephalus) in the nonbreeding season". Each row corresponds to a pairwise behavioural interaction between fairywrens.

Observation_ID = unique identifier for each behavioural observation (or row)
Date = The date the observation was conducted in YYYY-MM-DD format
Year = The year the observation was conducted
Time = The time that the observation started in 24-hour format and displayed as "#H #M #S" (e.g., "13H #15M #0S" for 1:15 pm); please note that the time was recorded to the nearest minute, so the "#S" is always "0S"
Behavior = the social behaviour observed occurring between the sender and receiver fairywren; in this data set, only "Allopreening", "Courtship", and "Chasing" are included 

Sender_ID = A unique identifier for the fairywren observed sending a behavioural signal (e.g., for courtship, this would be a male performing a courtship display); for allopreening, the distinction between sender and receiver is arbitrary; please note that "Sender_ID" and "Receiver_ID" are equivalent to color band combinations for the fairywrens we observed, but they were generated independently of the "Bird_ID" column from our focal behavioural data set ("RBFW3_Anon_Data_210720.csv"), so the two data sets cannot be linked using these IDs
Sender_plumage_category = Categories of plumage scores as per Karubian 2002; the same scoring system is used in this data set as in our focal behavioural data ("RBFW3_Anon_Data_210720.csv") except that Unornamented = DULL, Moulting = MOULT, and Ornamented = BRIGHT
Sender_sex = Sex of the bird sending the behavioural signal ("M" for male or "F" for female) 
Sender_age = Age of the bird sending the behavioural signal obtained through banding records
Sender_age_exact = Either "exact" if we were able to determine a sender's precise age or "minimum" if only a minimum estimate (e.g., after second year) was possible

Receiver_ID =  A unique identifier for the fairywren observed receiving a behavioural signal (e.g., for courtship, this could be a female being courted by a male)
Receiver_plumage_category = Categories of plumage scores as per Karubian 2002
Receiver_sex = Sex of the bird receiving the behavioural signal ("M" for male or "F" for female) 
Receiver_age = Age of the bird receiving the behavioural signal obtained through banding records
Receiver_age_exact = Either "exact" if we were able to determine a receiver's precise age or "minimum" if only a minimum estimate (e.g., after second year) was possible

Social_mate = Whether or not the sender and receiver observed interacting during the nonbreeding period became social mates during the upcoming breeding season 
Prev_breeding_group = Whether or not the sender and receiver observed interacting during the nonbreeding season belonged to the same breeding group (social mate, offspring, or related helpers) during the previous breeding season; here, "1" indicates previous breeding group members and "0" indicates that two birds did not belong to the same breeding group previously

In this data set, the value "NA" represents missing data. 