# README File for RBFW3 Data
<p>The article titled "Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (*Malurus melanocephalus*) in the nonbreeding season" makes use of two data sets. The primary data set is the file "RBFW3\_Anon\_Data\_210720.csv" and consists of focal behavioural observations. In the R code for this project, it is referred to as "RBFW." The data set "RBFW3\_Anon\_Opportunistic\_Data\_230116.csv" consists of opportunistic behavioural observations and is used to generate supplemental materials for the article. It is referred to as "RBFW_sup" in R code.</p>

<p>These two datasets are available in the "Data" GitHub folder that contains the present README file and have also been uploaded to Figshare using the following DOI links: </p>

* Focal data: [https://doi.org/10.6084/m9.figshare.15088014](https://doi.org/10.6084/m9.figshare.15088014)
* Opportunistic data: [https://doi.org/10.6084/m9.figshare.21790187](https://doi.org/10.6084/m9.figshare.21790187)

The contents of these two data sets are further detailed below:

## Focal Data ("RBFW3\_Anon\_Data_210720.csv")
<p, markdown = "1">This is the dataset used in the main analyses in the manuscript "Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (*Malurus melanocephalus*) in the nonbreeding season". Each row corresponds to a behavioural observation on a single male fairywren.</p, markdown="1">

**Columns Describing Observations and Focal Male Characteristics:** 
<p>`Observation_ID` = unique identifier for each behavourial observation (or row)<br>
`Date` = The date the observation was conducted in YYYY-MM-DD format<br>
`Year` = The year the observation was conducted<br>
`Time` = The time that the observation started in 24-hour format<br>
`Bird_ID` = A unique identifier for each focal male (equivalent to color band combinations) <br>
`Observer` = Identity of the research who collected the data<br>
`Sex` = Sex of the bird being observed (always male in this dataset)<br> 
`Plumage_score` = Numerical scoring of a focal male's plumage colouration as per Karubian 2002<br>
`Plumage_group` = Categories of plumage scores (DULL, MOULT, or BRIGHT) as per Karubian 2002<br>
`Age` = Age of each focal male obtained through banding records<br>
`Age_exact` = Either "exact" if we were able to determine a bird's precise age or "minimum" if only a minimum estimate (e.g., after second year) was possible</p>

**Behavioural Variables:**
<p, markdown="1">The variables `Alarm_calling`, `Allopreening`, `Calling`, `Chasing`, `Chipping`, `Courtship`, `Flying`, `Foraging`, `Other`, `Preening`, `Scolding`, `Singing`, `Sitting`, `U`, and `Vocalizing` provide the number of seconds during a focal observation in which a male was engaging in these behaviours. These behaviors are described in table S1 (an ethogram). Note that `U` in the dataset is equivalent to "Out of sight" in table S1. During some years of our study, certain behaviours were not considered. In these situations, these values are given as "NA" rather than an integer. In particular, only `Vocalizing` (any vocalization other than alarm calling) was recorded in 2016 while more specific categories of vocalizations (e.g., `Calling`, `Chipping`, `Scolding`, and `Singing`) were reported in 2017 and 2018.</p, , markdown="1"> 

`Observation_time` = Duration of a focal observation in seconds
 
<p, markdown = "1"> The variables `Alarm_calling_prop`, `Chasing_prop`, `Courtship_prop`, `Flying_prop`, `Foraging_prop`, `Other_Prop`, `Preening_prop`, `Sitting_prop`, and `Vocalizing_Prop` are calculated by dividing the number of seconds spent engaged in each behaviour (the value of the corresponding column without the "prop" suffix) by the `Observation_time` variable minus the `U` (time bird was not visible) variable. For example: <br>

`Preening_prop` = `Preening`/(`Observation_time` - `U`)</p, markdown="1">


## Opportunistic data ("RBFW3\_Anon\_Opportunistic\_Data\_230116.csv")
<p, markdown = "1">This data set consists of the opportunistic behavioural observations described in the manuscript "Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (*Malurus melanocephalus*) in the nonbreeding season". Each row corresponds to a pairwise behavioural interaction between fairywrens.</p, markdown = "1">

<p, markdown = "1">`Observation_ID` = unique identifier for each behavioural observation (or row)<br>
`Date` = The date the observation was conducted in YYYY-MM-DD format<br>
`Year` = The year the observation was conducted<br>
`Time` = The time that the observation started in 24-hour format and displayed as "#H #M #S" (e.g., "13H 15M 0S" for 1:15 pm); please note that the time was recorded to the nearest minute, so "#S" is always "0S"<br>
`Behavior` = the social behaviour observed occurring between the sender and receiver fairywren; in this data set, only "Allopreening", "Courtship", and "Chasing" are included </p, markdown = "1">

<p, markdown = "1">`Sender_ID` = A unique identifier for the fairywren observed sending a behavioural signal (e.g., for courtship, this would be a male performing a courtship display); for allopreening, the distinction between sender and receiver is arbitrary; please note that `Sender_ID` and `Receiver_ID` are equivalent to color band combinations for the fairywrens we observed, but they were generated independently of the `Bird_ID` column from our focal behavioural data set ("RBFW3\_Anon\_Data\_210720.csv"), so the two data sets cannot be linked using these IDs<br>
`Sender_plumage_category` = Categories of plumage scores as per Karubian 2002; the same scoring system is used in this data set as in our focal behavioural data ("RBFW3\_Anon\_Data\_210720.csv") except that Unornamented = DULL, Moulting = MOULT, and Ornamented = BRIGHT<br>
`Sender_sex` = Sex of the bird sending the behavioural signal ("M" for male or "F" for female)<br>
`Sender_age` = Age of the bird sending the behavioural signal obtained through banding records<br>
`Sender_age_exact` = Either "exact" if we were able to determine a sender's precise age or "minimum" if only a minimum estimate (e.g., after second year) was possible</p>

<p, markdown = "1">`Receiver_ID` =  A unique identifier for the fairywren observed receiving a behavioural signal (e.g., for courtship, this could be a female being courted by a male)<br>
`Receiver_plumage_category` = Categories of plumage scores as per Karubian 2002<br>
`Receiver_sex` = Sex of the bird receiving the behavioural signal ("M" for male or "F" for female) <br>
`Receiver_age` = Age of the bird receiving the behavioural signal obtained through banding records<br>
`Receiver_age_exact` = Either "exact" if we were able to determine a receiver's precise age or "minimum" if only a minimum estimate (e.g., after second year) was possible</p, markdown = "1">

<p, markdown = "1">`Social_mate` = Whether or not the sender and receiver observed interacting during the nonbreeding period became social mates during the upcoming breeding season<br> 
`Prev_breeding_group` = Whether or not the sender and receiver observed interacting during the nonbreeding season belonged to the same breeding group (social mate, offspring, or related helpers) during the previous breeding season; here, "1" indicates previous breeding group members and "0" indicates that two birds did not belong to the same breeding group previously</p, markdown = "1">

<p>In this data set, the value "NA" represents missing data. </p>
