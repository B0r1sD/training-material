# Introduction 

This tutorial is the part of [Single-cell RNA-seq: Case Study]({% link topics/single-cell/index.md %}) series and focuses on generating a single cell matrix using [Alevin]( https://salmon.readthedocs.io/en/latest/alevin.html) in bash command line. It is a replication of the [previous tutorial]({% link topics/single-cell/tutorials/scrna-case_alevin/tutorial.md %}) and will guide you through the same steps that you followed in the previous tutorial and will give you more understanding of what is happening ‘behind the scenes’ or ‘inside the tools’ if you will.
We will work on the case study data from a mouse model of fetal growth restriction {% cite Bacon2018 %} (see [the study in Single Cell Expression Atlas](https://www.ebi.ac.uk/gxa/sc/experiments/E-MTAB-6945/results/tsne) and [the project submission](https://www.ebi.ac.uk/arrayexpress/experiments/E-MTAB-6945/)).
As a recap, we fill go from raw FASTQ files to a cell x gene data matrix in AnnData format. After completing the previous tutorial you should already know what is a data matrix and AnnData format. We will perform the following steps:
1.	Getting the appropriate files
2.	Making a transcript-to-gene ID mapping
3.	Creating Salmon index
4.	Quantification of transcript expression using Alevin
5.	(Quality control using Alevin)
6.	Creating Summarized Experiment from the Alevin output
7.	Adding metadata
8.	Combining samples data


## Launching JupyterLab

> <warning-title>Data uploads & JupyterLab</warning-title>
> There are a few ways of importing and uploading data into JupyterLab. You might find yourself accidentally doing this differently than the tutorial, and that's ok. There are a few key steps where you will call files from a location - if these don't work from you, check that the file location is correct and change accordingly!
{: .warning}

> {% snippet faqs/galaxy/interactive_tools_jupyter_launch.md %}

Welcome to JupyterLab!

> <warning-title>Danger: You can lose data!</warning-title>
> Do NOT delete or close this notebook dataset in your history. YOU WILL LOSE IT!
{: .warning}

## Open the notebook

You have two options for how to proceed with this JupyterLab tutorial - you can run the tutorial from a pre-populated notebook, or you can copy and paste the code for each step into a fresh notebook and run it. The initial instructions for both options are below.

> <hands-on-title>Option 1: Open the notebook directly in JupyterLab</hands-on-title>
>
> 1. Open a `Terminal` in JupyterLab with File -> New -> Terminal
>
>   ![Screenshot of the Launcher tab with an arrow indicating where to find Terminal.](../../images/scrna-casestudy-monocle/terminal_choose.jpg "This is how the Launcher tab looks like and where you can find Terminal.")
>
> 2. Run
>    ```
>    wget {{ ipynbpath }}
>    ```
>
> 3. Select the notebook that appears in the list of files on the left.
>
>
> Remember that you can also download this {% icon notebook %} [Jupyter Notebook]({{ ipynbpath }}) from the {% icon galaxy_instance %} Supporting Materials in the Overview box at the beginning of this tutorial.
{: .hands_on}

> <hands-on-title>Option 2: Creating a notebook</hands-on-title>
>
> 1. Select the **Bash** icon under **Notebook**
>
>   ![Bash icon](../../images/bash.png "Bash Notebook Button")
>
> 2. Save your file (**File**: **Save**, or click the {% icon galaxy-save %} Save icon at the top left)
>
> 3. If you right click on the file in the folder window at the left, you can rename your file `whateveryoulike.ipynb`
>
{: .hands_on}

> <warning-title>You should <b>Save</b> frequently!</warning-title>
> This is both for good practice and to protect you in case you accidentally close the browser. Your environment will still run, so it will contain the last saved notebook you have. You might eventually stop your environment after this tutorial, but ONLY once you have saved and exported your notebook (more on that at the end!) Note that you can have multiple notebooks going at the same time within this JupyterLab, so if you do, you will need to save and export each individual notebook. You can also download them at any time.
{: .warning}

Let's crack on!

> <agenda-title></agenda-title>
>
> In this tutorial, we will cover:
>
> 1. TOC
> {:toc}
>
{: .agenda}

{% snippet topics/single-cell/faqs/notebook_warning.md %}


## Installation

Before we start working on the tutorial notebook, we need to install required packages.

><hands-on-title>Installing the packages</hands-on-title>
>
> 1. Navigate back to the `Terminal` (see Option 1 in the box above)
> 2. In the Terminal tab open, write the following, one line at a time:
> ```
>conda install -y -c bioconda bioconductor-tximeta
>```
>```
>conda install -y -c bioconda bioconductor-dropletutils
>```
>```
>conda install -y -c bioconda atlas-gene-annotation-manipulation
>```
>
{: .hands_on}


Installation will take a while, so in the meantime, when it's running, you can open the notebook and follow the rest of this tutorial there!
