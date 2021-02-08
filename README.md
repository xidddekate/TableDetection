# Steps to detect a table from image or PDF Successfully

git clone the code...

In the “data/images/” folder there are 403 image files from different types of documents.In addition to the images there are also 2 csv files with the ground truth data for this dataset. Each file has lines for each table found in each file, in the following format:
\<filename\>, \<xmin\>, \<ymin\>, \<xmax\>, \<ymax\>, \<class\> (in this case “class” will always be “table”)
    
Custom data can also be used.
Now,there is a need to process the images to make the contents better understandable for the object detection network. Run pre-processing.ipynb and there should be 2 additional directories in “data” folder — ”train” and “val”. These hold the preprocessed image files for training and validating the results.

# TFRecords

Run ==> pip3 install luminoth
<br>Now, create TFRecords using the command line tool “lumi” which comes with Luminoth.In the directory where “data” folder is placed open a terminal or command line and run the following line:
lumi dataset transform --type csv --data-dir data/ --output-dir tfdata/ --split train --split val --only-classes=table

This will create a folder called “tfdata” with the TFRecords needed for the training of the network.
 
 
 # Training

Edit the sample_config.yml file (already edited, refer for more details). Again, in the directory where “data” folder is placed open a terminal or command line and run the following line:
lumi train -c config.yml

# Predictions

Run the following line:
lumi checkpoint create config.yml

this will create the checkpoint
Now, use the command line tool to make a prediction (make sure to use the id of your checkpoint in following command):

lumi predict --checkpoint \<enter checkpoint-id here\> data/val/9541_023.png
    
Output will be cordinates of table area.This information can be used to mark the area in the original, unprocessed image.

To get quick view on the prediction, the small web application can be used that comes with Luminoth.
lumi server web --checkpoint \<enter checkpoint-id here\>
or
lumi server web
