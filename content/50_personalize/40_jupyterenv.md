+++
title = "Notebook Setup"
chapter = false
weight = 40
+++

### Downloading required additional files

We need to download two files before starting work, which are all stored within the Lab's Git repository:

- the Notebook file that contains the lab - **personalize_sample_notebook.ipynb**
- a file that is part of the MovieLens dataset - **u.item** - that has been edited slightly to remove some control characters that cause on of the _pandas_ library calls to fail, and also to include working URLs for the various movie posters that our application will show later

1. Go to the Git repository address, https://github.com/drandrewkane/AI_ML_Workshops, navigate to the **Lab 6** and download the files called **u.item** and **personalize_sample_notebook.ipynb** respectively.  Use any method that you are comfortable with, but do not clone the whole repository as it is quite large - for instance, try opening the files within Git in RAW format and saving them locally (be careful to maintain the correct file extentions)
2. In the notebook, assuming the status is now **InService**, and click on the **Upload** button, and in the dialog select the two files from the location that you stored them and upload them.

    ![Upload files](/images/uploadFiles.png)

3. Click on each of the two **Upload** buttons to actually upload the files, waiting for the first to complete before starting the second.

    ![Upload files part-2](/images/uploadFiles2.png)

4. Once both are upload you can click on the notebook **.ipynb** file and the lab notebook will open, and you can now begin to work through the lab notebook.

### Working Through a Jupyter Notebook

1. A notebook consisted of a number of cells; in SageMaker these will typically either be _Code_ or _Markdown_ cells.  Markdown is used to allow for documentation to be defined inline with the code, giving the author a rich set of markdown formatting options.  The first cell in this notebook, which is called **Get the Personalize boto3 Client**, is Markdown, and if you select any cell then the whole cell is highlighted.

    ![Example cell types](/images/cellTypes.png)

2. The first Markdown cell describes what the following Code cell is going to do – for the sake of this lab you do not have to understand the code that is being run in the Code cell, rather you should just appreciate what the notebook is doing and how you interact with a Jupyter notebook.

    ![First code cell](/images/loadBoto3Pre.png)

3. To the left of a Code module is a set of empty braces **[ ]**.  By highlighting the cell and then selecting the _Run_ command in the menu bar, the Jupyter notebook will execute this code, outputting and code outputs to the notebook screen and keeping any results data internally for re-use in future steps.  Do this now to execute the first code cell.

    *Note: if a Markdown cell is highlighted, then clicking **Run** will move the highlight to the next cell*

3. Whilst the code is executing the braces will change to be **[\*]**, indicating that it is executing, and once complete will change to **[1]**.  Future cells will have increasing numbers inside the braces, and this helps you see the order in which cells have been exected within the notebook.  Directly below the code, but still within the Code cell, is the output from the code execution - this will include any error messages that your code has thrown.

    ![First execution](/images/loadBoto3Post.png)

5. Now please continue to work through the notebook lab - read the comments prior to each Code cell in order to get an understanding as to what is going on, as these explain why we are doing each step and how it ties in to using the Amazon Personalize service.