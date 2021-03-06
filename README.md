# PHBS_MLF_2018
# Satellite_Image_Recognition

# Group members:
Name|Student id|Github contact
-----|---------|--------------
Giacomo Galanti|1802010267|[giacomogalanti](https://github.com/giacomogalanti)
Giovanni Ostuni|1802010237|[giovanniostuni](https://github.com/giovanniostuni)
Matteo Rossi Tantini|1802010274|[MatteoRossiTantini](https://github.com/MatteoRossiTantini)
Jian Zhou|1801212818|[zhoujianberkeley](https://github.com/zhoujianberkeley)

the project has been developed equally by each member of the group.

# Motivation:
 
Image recognition is a disruptive technology with many applications from self-driving cars to national security. This specific project could be used for logistic, monitoring port activities and defense purposes.
 
# Data description:

The dataset from [Kaggle](https://www.kaggle.com/rhammell/ships-in-satellite-imagery/home) contains 4000 pictures available both as PNG files and in .JSON format. 1000 pictures are classified as “ships” (1) and the other 3000 as “non-ships” (0). These include also partial parts of real ships (ex. Is missing the back part).

The .JSON file contains a list of three digit number between (0, 256) since any picture is stores as a 19,200 list of integers. As said before, every picture has 80x80=6400 pixels, to each of one three (0, 256) numbers are associated which indicate the intensity for RED, GREEN and BLUE of every pixel.

# Methodology:
#data analysis:

<img width="650" alt="screenshot 2018-11-08 at 19 36 25" src="https://user-images.githubusercontent.com/42959419/48211648-e3301e00-e3b4-11e8-84cc-09d8ab8e70fa.png">
<img width="659" alt="screenshot 2018-11-08 at 19 51 52" src="https://user-images.githubusercontent.com/42959419/48211716-0c50ae80-e3b5-11e8-8ccf-fec53f2ec2e9.png">

#reshape data:

<img width="465" alt="screenshot 2018-11-09 at 00 29 41" src="https://user-images.githubusercontent.com/42959419/48212480-a402cc80-e3b6-11e8-90c1-731bb59d5993.png">

<img width="468" alt="screenshot 2018-11-09 at 00 29 50" src="https://user-images.githubusercontent.com/42959419/48212523-bc72e700-e3b6-11e8-96fb-c7f7e2070c27.png">


#plot of RGB intensities:

<img width="620" alt="screenshot 2018-11-08 at 20 22 27" src="https://user-images.githubusercontent.com/42959419/48211750-212d4200-e3b5-11e8-92b3-343c7d476dba.png">


Then we modified the pixel scale, dividing it by 255, to obtain it in a range [0,1]

# Benchmarking the CNN with logistic classifier and decision tree
  * Compare Multiple Classifiers:
  
  * K-Fold Cross-Validation Accuracy:
  
  * LR: 0.890938 (0.008778)
  * DTC: 0.898438 (0.018606)

Even if we knew that the CNN model would be the best performer, being the task image recognition, we decided to run Logistic Regression and Decision Tree models, in order to compare their results with the CNN. Having worked directly on our PC (no GPU) we trained only these two models beacuse of computational power (took 1h). 

# CNN
   * Convolutional and pooling layers 1 and 2
     * From the input layer (4000x80x80x3 (batch_size, height, width, channels) we extract 32 5x5 filters (#convolutional layer1) which will shrink to a 2x2 size with #pooling layer 1 (we applied MaxPooling).
     * From here we connect a second convolutional layer exctracting 64 5x5 filters (#convolutional layer 2), then we apply another 2x2 pooling layer.

   * dense layer
     * Having applied 2 pooling layers, the picture size (80x80) is reduced twice by 50% (20x20). 
     * Before applying the dense layer, we flatten our pool2 into 2 dimensions (-1, 20x20x64).
     * So now we can add a dense layer to perform classification on the features extracted by the convolution/pooling layers.
     * We apply dropout regolarization to improve the results (rate=0.4 means that 40% of the elements will be dropped out during training).

   * logit layer
     * The logit layer is the final layer with an outpout (0,1).

# Training and evalutation
We calculate a loss function valid for both train and evaluation sets
We calculate the optimaser for the training (gradient descent learning_rate=0.001)
And define accuracy as a metric on the evaluation set.
   
Given the time used we set logging hooks every 50 iterations on a total of 200 steps. Morover we set batch_size=100 (we
train our model on subsets of 100 samples shuffled).
So, we obtained 3 intermediate results:

Hook at|Result|
-----|---------
50|<img width="479" alt="screenshot 2018-11-08 at 23 37 41" src="https://user-images.githubusercontent.com/42959419/48211827-4ae66900-e3b5-11e8-9018-4be7e9b96fb2.png">
100|<img width="476" alt="screenshot 2018-11-08 at 23 37 52" src="https://user-images.githubusercontent.com/42959419/48211859-5d60a280-e3b5-11e8-81bf-73dd0e485abb.png">
150|<img width="621" alt="screenshot 2018-11-08 at 23 38 02" src="https://user-images.githubusercontent.com/42959419/48211872-694c6480-e3b5-11e8-944d-4cbfa5592d9a.png">
      
## Model final result:
Final Hook|Final Result|
-----|---------
200|<img width="832" alt="screenshot 2018-11-08 at 23 38 35" src="https://user-images.githubusercontent.com/42959419/48211914-7d906180-e3b5-11e8-90f8-64473a76a724.png">

The CNN model has an accuracy of 98%, compare to Logistic Regression and Decision Tree models, which have an accuracy below 90%.
This confirmes our thesis that the CNN model is the best performer. 

# Future Project Application:
Unfortunately, our Kaggle dataset is biased. It takes as input relatively little images given that each pixel is 3m every 80x80 image is 240x240 meters on earth, with the sever risk of having a partial ship (the front in A and the back in B, and labeled as 0). In the dataset, even if the 90% of ship is in one picture the label is 0. 
We thought about begging the closest images together to avoid this problem, but they are not clearly ordered within the dataset. 
This means that, in order to deliver a product able to work in reality, we have to take control of the creation of the dataset. Given that the images are available online we have to split them ordinately following a grid order. 
Starting from the assumption that our CNN fires 1 when a certain concentration and pattern of pixel/color is found:
 * With 80x80 pixels, through the use of the coordinate feature (not used before in our project) we can take into account also the concentration of pixel/color in the next pictures. The intuition would be to expand the area of analysis beyond the border of a single image. Unfortunately, our model (trained on the Kaggle dataset) is used to classify partial ships as 0. So, we need to create label for pictures giving 1 to both complete ships and partial ones. In this way, we believe we could classify also those partial images as ship (label=1), with an high test accuracy on real life application.

