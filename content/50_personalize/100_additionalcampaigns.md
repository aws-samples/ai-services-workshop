+++
title = "Additional Campaigns"
chapter = false
weight = 100
+++

If you look at the embedded documentation you'll see that it talks about 3 other models, which there isn't time to build during this Lab.  They involve the user of additional data files - a user demographic file, and a item metadata file, all of which are supplied with the Movie Lens data set in your Sagemaker Notebook.  Because they required additional data-sets, you need to create each of these within their own Personalize Dataset Group, and you also need to re-import the original interactions file **DEMO-movie-lens-100k.csv** that you uploaded into S3 during the notebook - this is because Personalize trains solutions on all data files witin the Dataset Group.

The three models that you should build are as follows:

- Using a USERS file, create a model that takes into account user's demographic details such as age, gender and occupation
- Using an ITEMS metadata file, create a model that also takes into account the movie year and the top-4 genres associated with that movie as 4 separate metadata fields
- Using an ITEMS metadata file, create a model that also takes into account the movie year and then compounds the top-4 genres into a single metadata field

Observations are that demographics are absolutely not a good indicator for movies recommendations, nor for things like book recommendations - this isn't an issue with Amazon Personalize, rather it is a know issue with using age and gender to predict likes and dislikes of media.  Also, the single, compound genre certainly seems more accurate for the first 5 or 10 responses, but for the set of 25 response as a whole the multiple genre model probably gets a better list of movies than the compound one.