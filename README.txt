============================================================
ALZHEIMER'S DISEASE AND HEALTHY AGING DATA ANALYSIS
============================================================

Project Type:
Exploratory Data Analysis and Visualization in R

Language:
R

Software:
R / RStudio

Dataset:
export.csv


------------------------------------------------------------
1. PROJECT OVERVIEW
------------------------------------------------------------

This project analyzes population-level data related to
Alzheimer's disease, cognitive decline, and healthy aging.

The main purpose of the project is to explore patterns and
relationships in cognitive decline among older adults and
investigate how cognitive health measures vary across:

- Time
- Age groups
- Sex
- Geographic locations
- Physical activity
- Depression
- Smoking
- Obesity
- Mental health
- Sleep and other healthy-aging factors

The analysis uses R for data cleaning, statistical analysis,
data visualization, and exploratory analysis.


------------------------------------------------------------
2. IMPORTANT DATASET NOTE
------------------------------------------------------------

This dataset is a population-level public health dataset.

It should NOT be treated as an individual patient dataset and
does not provide an individual-level Alzheimer's disease
diagnosis.

Therefore, this project focuses on reported cognitive decline,
memory loss, and related healthy-aging indicators rather than
predicting whether an individual person has Alzheimer's disease.


------------------------------------------------------------
3. MAIN OBJECTIVES
------------------------------------------------------------

The project aims to answer the following questions:

1. How have cognitive decline measures changed over time?

2. How does subjective cognitive decline vary between age
   groups?

3. How do cognitive decline measures differ by sex?

4. How does reported cognitive decline vary across locations?

5. Is physical inactivity associated with subjective cognitive
   decline?

6. Is depression associated with subjective cognitive decline?

7. How are smoking, obesity, mental distress, and sleep related
   to cognitive decline?

8. What patterns can be identified in the healthy-aging data?


------------------------------------------------------------
4. DATASET CONTENT
------------------------------------------------------------

The dataset contains health-related observations collected
across multiple years and geographic locations.

The dataset includes information covering areas such as:

- Overall Health
- Screenings and Vaccines
- Nutrition, Physical Activity and Obesity
- Caregiving
- Mental Health
- Smoking and Alcohol Use
- Cognitive Decline

The Cognitive Decline category includes measures related to:

- Subjective cognitive decline or memory loss
- Need for assistance with daily activities because of
  cognitive decline or memory loss
- Functional difficulties associated with cognitive decline
- Discussions with health professionals about cognitive decline


------------------------------------------------------------
5. COGNITIVE DECLINE ANALYSIS
------------------------------------------------------------

The main outcome of interest is:

"Subjective cognitive decline or memory loss among older adults"

Other cognitive-decline measures are also examined to provide
a broader view of cognitive health.


------------------------------------------------------------
6. VARIABLES OF INTEREST
------------------------------------------------------------

Important variables used in the analysis may include:

- year_start
- location_desc
- class
- topic
- data_value
- stratification1
- stratification2

The exact variable names should be checked after importing the
dataset into R.


------------------------------------------------------------
7. SOFTWARE REQUIREMENTS
------------------------------------------------------------

Install the following software:

1. R
2. RStudio

Recommended R packages:

- tidyverse
- janitor
- skimr
- GGally
- corrplot
- gtsummary
- broom
- pROC


------------------------------------------------------------
8. INSTALLATION OF R PACKAGES
------------------------------------------------------------

Run the following command in the RStudio Console:

install.packages(c(
  "tidyverse",
  "janitor",
  "skimr",
  "GGally",
  "corrplot",
  "gtsummary",
  "broom",
  "pROC"
))


------------------------------------------------------------
9. LOADING THE PACKAGES
------------------------------------------------------------

Use:

library(tidyverse)
library(janitor)
library(skimr)
library(GGally)
library(corrplot)
library(gtsummary)
library(broom)
library(pROC)


------------------------------------------------------------
10. IMPORTING THE DATA
------------------------------------------------------------

Place export.csv in the project working directory.

Then use:

data <- read.csv("export.csv")

Check that the data was imported correctly:

head(data)
dim(data)
names(data)
str(data)
summary(data)


------------------------------------------------------------
11. DATA CLEANING
------------------------------------------------------------

Clean the column names:

data <- data %>%
  clean_names()

Check for missing values:

colSums(is.na(data))

Check for duplicate rows:

sum(duplicated(data))


------------------------------------------------------------
12. EXPLORATORY DATA ANALYSIS
------------------------------------------------------------

The project begins with exploratory analysis to understand:

- Number of observations
- Number of variables
- Variable types
- Missing values
- Duplicate records
- Unique locations
- Available years
- Health topics
- Population stratifications


Useful commands include:

head(data)

dim(data)

names(data)

str(data)

summary(data)

skim(data)

sort(unique(data$year_start))

sort(unique(data$location_desc))

sort(unique(data$topic))


------------------------------------------------------------
13. COGNITIVE DECLINE DATA
------------------------------------------------------------

Extract the Cognitive Decline section:

cognitive <- data %>%
  filter(class == "Cognitive Decline")

View the available topics:

sort(unique(cognitive$topic))


------------------------------------------------------------
14. ANALYSIS OF COGNITIVE DECLINE OVER TIME
------------------------------------------------------------

Calculate the average reported value by year and topic:

cognitive_year <- cognitive %>%
  group_by(year_start, topic) %>%
  summarise(
    mean_value = mean(data_value, na.rm = TRUE),
    .groups = "drop"
  )


------------------------------------------------------------
15. VISUALIZATION OF COGNITIVE DECLINE OVER TIME
------------------------------------------------------------

A line graph can be created using:

ggplot(cognitive_year,
       aes(x = year_start,
           y = mean_value,
           color = topic,
           group = topic)) +
  geom_line(linewidth = 1) +
  geom_point() +
  labs(
    title = "Cognitive Decline Measures Over Time",
    x = "Year",
    y = "Average Percentage",
    color = "Cognitive Measure"
  ) +
  theme_minimal()


------------------------------------------------------------
16. AGE GROUP ANALYSIS
------------------------------------------------------------

The project compares cognitive decline between different
age groups.

Example:

cognitive_age <- cognitive %>%
  group_by(year_start, topic, stratification1) %>%
  summarise(
    mean_value = mean(data_value, na.rm = TRUE),
    .groups = "drop"
  )


Visualization:

ggplot(cognitive_age,
       aes(x = year_start,
           y = mean_value,
           color = stratification1)) +
  geom_line(linewidth = 1) +
  geom_point() +
  facet_wrap(~ topic, scales = "free_y") +
  labs(
    title = "Cognitive Decline by Age Group",
    x = "Year",
    y = "Percentage",
    color = "Age Group"
  ) +
  theme_minimal()


------------------------------------------------------------
17. SEX ANALYSIS
------------------------------------------------------------

Cognitive decline measures can also be compared between males
and females.

Example:

cognitive_sex <- cognitive %>%
  filter(stratification2 %in% c("Female", "Male")) %>%
  group_by(year_start, topic, stratification2) %>%
  summarise(
    mean_value = mean(data_value, na.rm = TRUE),
    .groups = "drop"
  )


------------------------------------------------------------
18. GEOGRAPHIC ANALYSIS
------------------------------------------------------------

The project examines differences in subjective cognitive
decline between geographic locations.

Example:

memory_data <- cognitive %>%
  filter(
    topic ==
      "Subjective cognitive decline or memory loss among older adults"
  )

memory_location <- memory_data %>%
  group_by(location_desc) %>%
  summarise(
    mean_value = mean(data_value, na.rm = TRUE),
    .groups = "drop"
  ) %>%
  arrange(desc(mean_value))


Visualization:

ggplot(memory_location,
       aes(x = reorder(location_desc, mean_value),
           y = mean_value)) +
  geom_col() +
  coord_flip() +
  labs(
    title = "Subjective Cognitive Decline by Location",
    x = "Location",
    y = "Average Percentage"
  ) +
  theme_minimal()


------------------------------------------------------------
19. HEALTHY-AGING FACTORS
------------------------------------------------------------

The project examines potential relationships between cognitive
decline and other health indicators.

Examples include:

- Physical inactivity
- Depression
- Smoking
- Obesity
- Frequent mental distress
- Sleep


These analyses identify associations in population-level data.


------------------------------------------------------------
20. PHYSICAL INACTIVITY ANALYSIS
------------------------------------------------------------

Example:

cognitive_memory <- data %>%
  filter(
    topic ==
      "Subjective cognitive decline or memory loss among older adults"
  )

physical_inactivity <- data %>%
  filter(
    topic ==
      "No leisure-time physical activity within past month"
  )

memory_summary <- cognitive_memory %>%
  group_by(location_desc, year_start) %>%
  summarise(
    cognitive_decline = mean(data_value, na.rm = TRUE),
    .groups = "drop"
  )

activity_summary <- physical_inactivity %>%
  group_by(location_desc, year_start) %>%
  summarise(
    physical_inactivity = mean(data_value, na.rm = TRUE),
    .groups = "drop"
  )

comparison <- memory_summary %>%
  inner_join(
    activity_summary,
    by = c("location_desc", "year_start")
  )


------------------------------------------------------------
21. CORRELATION ANALYSIS
------------------------------------------------------------

Selected health topics can be combined to examine correlations.

Potential variables include:

- Subjective cognitive decline
- Obesity
- Physical inactivity
- Current smoking
- Depression
- Frequent mental distress
- Sufficient sleep

A correlation matrix can be generated using:

correlation_data <- topic_wide %>%
  select(where(is.numeric))

cor_matrix <- cor(
  correlation_data,
  use = "pairwise.complete.obs"
)

corrplot(
  cor_matrix,
  method = "color",
  type = "upper",
  tl.col = "black",
  tl.cex = 0.7
)


------------------------------------------------------------
22. RECOMMENDED VISUALIZATIONS
------------------------------------------------------------

The following figures are recommended for the project:

1. Distribution of health categories

2. Cognitive decline trends from 2015–2022

3. Four cognitive decline measures over time

4. Cognitive decline by age group

5. Cognitive decline by sex

6. Geographic differences in cognitive decline

7. Physical inactivity vs cognitive decline

8. Depression vs cognitive decline

9. Smoking vs cognitive decline

10. Correlation heatmap of selected health indicators


------------------------------------------------------------
23. STATISTICAL INTERPRETATION
------------------------------------------------------------

Results should be interpreted as population-level associations.

For example:

"There was an association between higher reported physical
inactivity and higher reported subjective cognitive decline."

Avoid making causal statements such as:

"Physical inactivity causes Alzheimer's disease."

The dataset is observational and population-level, so observed
relationships do not automatically demonstrate causation.


------------------------------------------------------------
24. LIMITATIONS
------------------------------------------------------------

Important limitations of this project include:

1. The dataset is not an individual patient-level Alzheimer's
   disease dataset.

2. It does not provide an individual Alzheimer's diagnosis.

3. The analysis is primarily descriptive and observational.

4. Associations do not prove causation.

5. Reported health measures may be affected by self-reporting
   and survey methodology.

6. Geographic and population differences may be influenced by
   factors not included in the dataset.

7. Aggregated data cannot be used to determine individual
   Alzheimer's disease risk.


------------------------------------------------------------
25. PROJECT STRUCTURE
------------------------------------------------------------

Recommended project folder structure:

Alzheimers_Healthy_Aging/
|
|-- export.csv
|
|-- alzheimers_analysis.R
|
|-- README.txt
|
|-- results/
|   |
|   |-- figures/
|   |
|   |-- tables/
|
|-- report/
|
|-- data/
|
|-- scripts/


------------------------------------------------------------
26. R SCRIPT ORGANIZATION
------------------------------------------------------------

The main R script should be organized into sections:

1. Load packages
2. Import dataset
3. Inspect dataset
4. Clean dataset
5. Check missing values
6. Exploratory data analysis
7. Cognitive decline analysis
8. Time-series analysis
9. Age-group analysis
10. Sex analysis
11. Geographic analysis
12. Healthy-aging factor analysis
13. Correlation analysis
14. Statistical analysis
15. Visualizations
16. Export results


------------------------------------------------------------
27. EXPECTED OUTCOMES
------------------------------------------------------------

At the end of the project, the analysis should provide:

- A clear description of the dataset
- Clean and organized data
- Summary statistics
- Trends in cognitive decline
- Age-group comparisons
- Sex comparisons
- Geographic comparisons
- Relationships between cognitive decline and healthy-aging
  factors
- Statistical summaries
- Publication-quality visualizations
- A discussion of limitations and findings


------------------------------------------------------------
28. CONCLUSION
------------------------------------------------------------

This project provides an exploratory analysis of cognitive
decline and healthy aging using population-level public health
data.

The analysis focuses on identifying patterns and associations
between cognitive health and demographic, behavioral, and
health-related factors.

The findings should be interpreted as descriptive and
associational rather than as evidence of individual Alzheimer's
disease diagnosis or causation.


------------------------------------------------------------
29. AUTHOR
------------------------------------------------------------

Project:
Alzheimer's Disease and Healthy Aging Data Analysis

Language:
R

Environment:
RStudio

Dataset:
export.csv

============================================================
END OF README
============================================================