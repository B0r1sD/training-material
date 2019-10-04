---
layout: tutorial_hands_on

title: Cleaning GBIF data for the use in Ecology
zenodo_link: ''
questions:
- How can I get ecological data from GBIF?
- How do I check and clean the data from GBIF?

- Which ecoinformatics techniques are important to know for this type of data?
  - Data handling
  - Data filtering
  - Data visualization, notably dealing with GIS data
objectives:
- Get occurrence data on a species
- Visualize the data to understand them
- Clean GBIF dataset for further analyses
time_estimation: 0H30
key_points:
- Take the time to look at your data first, manipulate it before analyzing it
contributors:
- yvanlebras
- sbenateau

---


# Introduction
{:.no_toc}

<!-- This is a comment. -->

GBIF (Global Biodiversity Information Facility, www.gbif.org) is for sure THE most remarkable biodiversity data aggregator worldwide giving access to more than 1 billion reords across all taxonomic groups. The data provided via these sources are highly valuable for research. However, some issues exist concerning data heterogeneity, as they are obtained from various collection methods and sources.

In this tutorial we will propose a way to clean occurence records retrieved from GBIF.

This tutorial is based on the Ropensci {% cite zizka2018 %} tutorial.

> ### Agenda
>
> In this tutorial, we will cover:
>
> 1. Download data from GBIF
> 2. Clean data
> 3. Convert text data into GIS format
> 4. Visualize spatialized data
> {:toc}
>
{: .agenda}

# Retrive data from GBIF

## Get data

> ### {% icon hands_on %} Hands-on: Data upload
>
> 1. Create a new history for this tutorial
> 2. Import the files from GBIF
> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **Get species occurrences data** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Scientific name of the species"*: write the scientific name of something you are interested on, for example `Loligo vulgaris`
>    - *"Data source to get data from"*: `Global Biodiversity Information Facility : GBIF`
>    - *"Number of records to return"*: `999999` is a maimum value
>
>    > ### {% icon comment %} Comment
>    >
>    > ***TODO***: *Consider adding a comment regarding the maximum value, the R package spocc ...*
>
>    {: .comment}
>
{: .hands_on}
>
> 4. Check that the datatype
>
>    {% include snippets/change_datatype.md datatype="datatypes" %}
>
> 5. Add to each dataset a tag corresponding to the species and/or the data source for example
>
>    {% include snippets/add_tag.md %}
>
{: .hands_on}


## Where come from the records?

> ### {% icon hands_on %} Hands-on: Here we propose to investigate the contaent of the dataet looking notably at the "basisOfRecord" attribute to know more about heterogenity related to the data collection origin.
>
> 1. **Count** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"from dataset"*: `output` (output of **Get species occurrences data** {% icon tool %})
>    - *"Count occurrences of values in column(s)"*: `c[17]`
>
>
>    > ### {% icon comment %} Comment
>    >
>    > This tool is one of the important "classical" Galaxy tool who allows you to better synthtize information content of your data. Here we apply this tool to the 17th column (corresponding to the basisOfRecord attribute) but don't hesitate to investigate others attributes!
>    {: .comment}
>
{: .hands_on}

> ### {% icon question %} Questions
>
> 1. How many different types of data collection origin are there?
> 2. What is your assumption regarding this heterogeneity?
>
> > ### {% icon solution %} Solution
> >
> > 1. 5
> > 2. each basisOfRecord type is related to different collection method so different data quality
> >
> {: .solution}
>
{: .question}

## Filtering data thanks to the data origin

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **Filter** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Filter"*: `output` (output of **Get species occurrences data** {% icon tool %})
>    - *"With following condition"*: `c17=='HUMAN_OBSERVATION' or c17=='OBSERVATION' or c17=='PRESERVED_SPECIMEN'`
>    - *"Number of header lines to skip"*: `1`
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

> ### {% icon question %} Questions
>
> 1. How many records are kept and what is the percentage of filtered data?
> 2. Why are we keeping only these 3 types of data collection origin?
>
> > ### {% icon solution %} Solution
> >
> > 1. 470 and 8.79% of records were drop out
> > 2. These data collection methods are the most relevant
> >
> {: .solution}
>
{: .question}

{: .hands_on}
>
> 1. Add to the output dataset a propagating tag corresponding to the filtering criteria adding `#basisOfRecord` string for example
>
>    {% include snippets/add_tag.md %}
>
{: .hands_on}

## Have a look at the age of records

> ### {% icon hands_on %} Hands-on: Here we propose to have a look at the age of records to know if there is some ancient data to maybe not consider.
>
> 1. **Summary Statistics** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Summary statistics on"*: `out_file1` (output of **Filter** {% icon tool %})
>    - *"Column or expression"*: `c57`
>
> 2. Add to the output dataset a propagating tag corresponding to the filtering criteria adding `#ageOfRecord` string for example
>
{: .hands_on}

> ### {% icon question %} Questions
>
> 1. What is the year of the oldier and younger records?
> 2. Why do you think of interest to treat differently ancient and recent records?
>
> > ### {% icon solution %} Solution
> >
> > 1. From 1903 to 2018
> > 2. We can assume ancient records are not made in the same way than recent one so keeping ancient records can enhance heterogeneity of our dataset.
> >
> {: .solution}
>
{: .question}

## Sub-step with **Filter**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **Filter** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Filter"*: `out_file1` (output of **Filter** {% icon tool %})
>    - *"With following condition"*: `c57>0 and c57<99`
>    - *"Number of header lines to skip"*: `1`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Summary Statistics**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **Summary Statistics** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Summary statistics on"*: `out_file1` (output of **Filter** {% icon tool %})
>    - *"Column or expression"*: `c39`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Filter**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **Filter** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Filter"*: `out_file1` (output of **Filter** {% icon tool %})
>    - *"With following condition"*: `c39>1945`
>    - *"Number of header lines to skip"*: `1`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Count**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **Count** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"from dataset"*: `out_file1` (output of **Filter** {% icon tool %})
>    - *"Count occurrences of values in column(s)"*: `c[31]`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Filter**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **Filter** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Filter"*: `out_file1` (output of **Filter** {% icon tool %})
>    - *"Number of header lines to skip"*: `1`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **OGR2ogr**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. **OGR2ogr** {% icon tool %} with the following parameters:
>    - {% icon param-file %} *"Gdal supported input file"*: `out_file1` (output of **Filter** {% icon tool %})
>    - *"Conversion format"*: `GEOJSON`
>    - *"Specify advanced parameters"*: `Yes, see full parameter list.`
>        - In *"Add an input dataset open option"*:
>            - {% icon param-repeat %} *"Insert Add an input dataset open option"*
>                - *"Input dataset open option"*: `X_POSSIBLE_NAMES=longitude`
>            - {% icon param-repeat %} *"Insert Add an input dataset open option"*
>                - *"Input dataset open option"*: `Y_POSSIBLE_NAMES=latitude`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}


## Re-arrange

To create the template, each step of the workflow had its own subsection.

***TODO***: *Re-arrange the generated subsections into sections or other subsections.
Consider merging some hands-on boxes to have a meaningful flow of the analyses*

# Conclusion
{:.no_toc}

Sum up the tutorial and the key takeaways here. We encourage adding an overview image of the
pipeline used.
