# PHBS_MLF_2018
# Satellite_Image_Recognition

# Group members:
Name|Student id|Github contact
-----|---------|--------------
Giacomo Galanti|1802010267|@giacomogalanti
Giovanni Ostuni|1802010237|@giovanniostuni
Matteo Rossi Tantini|1802010274|@MatteoRossiTantini
Jian Zhou|1801212818|@zhoujianberkeley

# Motivation:
 
Image recognition is a disruptive technology with many applications from self-driving cars to national security. This specific project could be used for logistic, monitoring port activities and defense purposes.
 
# Data description:

The dataset from [Kaggle] (https://www.kaggle.com/rhammell/ships-in-satellite-imagery/home) contains 4000 pictures available both as PNG files and in .JSON format. 1000 pictures are classified as “ships” (1) and the other 3000 as “non-ships” (0). These include also partial parts of real ships (ex. Is missing the back part), which opens on how to set the threshold of the classifier.

The .JSON file contains a list of three digit number between (0, 256) since any picture is stores as a 19,200 list of integers. As said before, every picture has 80x80=6400 pixels, to each of one three (0, 256) numbers are associated which indicate the intensity for RED, GREEN and BLUE of every pixel.
