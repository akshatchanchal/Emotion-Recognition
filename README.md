# Emotion-Recognition
## Facial Detection and emotion recognition using Python, dlib ,OpenCV


Having your computer know how you feel? Madness!  
Facial expressions are one of the most powerful, natural and immediate means for human being to communicate their emotions and intensions. Recognition of facial expression has many applications including human-computer interaction, cognitive science, human emotion analysis, personality development ,from a dynamic music player that plays music fitting with what you feel, to an emotion-recognizing robot.emotional state of a person may influence concentration, task solving and decision making skills, affective computing vision is to make systems able to recognize and influence human emotions in order to enhance productivity and effectiveness of working with computers. 


- Facial landmarks:  
Identifying faces in photos or videos is very cool, but this isn’t enough information to create powerful applications, we need more information about the person’s face, like position, whether the mouth is opened or closed, the Dlib, a library capable of giving you 68 points (landkmarks) of the face.

- Dlib is a landmark’s facial detector with pre-trained models, the dlib is used to estimate the location of 68 coordinates (x, y) that map the facial points on a person’s face.  

- To be able to recognize emotions on images we will use OpenCV. OpenCV has a few ‘facerecognizer’ classes that we can also use for emotion recognitionThe Open Source Computer Vision Library has >2500 algorithms, extensive documentation and sample code for real-time computer vision.  

- CK+ dataset(cohn-kanade database): It is organised into two folders, one containing images, the other txt files with emotions encoded that correspond to the kind of emotion shown. From the readme of the dataset, the encoding is: {0=neutral, 1=anger, 2=contempt, 3=disgust, 4=fear, 5=happy, 6=sadness, 7=surprise}.  

- A support vector machine (SVM) is a supervised machine learning model that uses classification algorithms for two-group classification problems

> ## The approach is to first extract facial landmark points from the images, randomly divide 80% of the data into a training set and 20% into a test set, then feed these into the classifier and train it on the training set. Finally we evaluate the resulting model by predicting what is in the test set to see how the model handles the unknown data.







### DATASET:  

In the readme file, the authors mention that only a subset (327 of the 593) of the emotion sequences actually contain archetypical emotions. Each image sequence consists of the forming of an emotional expression, starting with a neutral face and ending with the emotion. So, from each image sequence we want to extract two images; one neutral (the first image) and one with an emotional expression (the last image).  


### Testing the landmark detector  
This will result in your face with a lot of dots outlining the shape and all the “moveable parts”
It uses Dlib (which is written in C++) whichh is used for facial landmark detection.    
    • get_frontal_face_detector gets the face from the image .It is based on SVM and HOG (Histogram of orient gradients)  
    • Shape_predictor takes image region containing face as input and outputs a set of locations that define the pose of the object that is the moving parts of the face in our case.  
    • The image is converted into grayscale using opencv.  
    • The contrast of the image is improved using Clahe from opencv (contrast limited adaptive his togram equalization). A good image is defined as the one having pixels from all regions of the image.(0=Black,255=White) . It has parameters like clip limit and grid size. Clip limit is set to clip / limit the amplification. Clahe does not overamplify noise in relatively homogenous regions of an image.  


### Extracting faces  
The classifier will work best if the training and classification images are all of the same size and have (almost) only a face on them (no clutter). We need to find the face on each image, convert to grayscale, crop it and save the image to the dataset. We can use a HAAR filter from OpenCV to automate face finding.  

#### Extracting features from the faces  

The first thing to do is find ways to transform these nice dots overlaid on your face into features to feed the classifer. Features are little bits of information that describe the object or object state that we are trying to divide into categories.  
The same classifying algorithm might function tremendously well or not at all depending on how well the information we feed it is able to discriminate between different objects or object states.  
    • A less destructive way could be to calculate the position of all points relative to each other. To do this we calculate the mean of both axes, which results in the point coordinates of the sort-of “centre of gravity” of all face landmarks. We can then get the position of all points relative to this central point.  
    • Get the angles of each point relative to the central point and if the face is tilted then each point is offset by the angle of the nose bridge.  
    • Now it’s time to put all of the above together with some stuff from the first post. The goal is to read the existing dataset into a training and prediction set with corresponding labels, train the classifier (we use Support Vector Machines with linear kernel from SKLearn, but feel free to experiment with other available kernels such as polynomial or rbf, or other classifiers!), and evaluate the result. This evaluation will be done in two steps; first we get an overall accuracy after ten different data segmentation, training and prediction runs, second we will evaluate the predictive probabilities.  


#### Creating the training and classification set  

Now we get to the fun part! The dataset has been organised and is ready to be recognized, but first we need to actually teach the classifier what certain emotions look like. The usual approach is to split the complete dataset into a training set and a classification set. We use the training set to teach the classifier to recognize the to-be-predicted labels, and use the classification set to estimate the classifier performance.  

The reason for splitting the dataset: estimating the classifier performance on the same set as it has been trained is unfair.  
We randomly sample and train on 80% of the data and classify the remaining 20%, and repeat the process 10 times.In the end on my machine this returned 90.3092783505 % corrrect.  


### Results
With 5 emotions (leaving out "contempt", "fear" and "sadness"), because the 3 categories had very few images:  

| Fisher Face classifier: | 90.30% |
| ------ | ------ |
| Linear SVM:| 89.69 |  
| Polynomial SVM:| 89.58% |

Therefore, we can note that all classifiers were better when using the reduced dataset because the categories are more balanced.  

> #### Reference:  
>  van Gent, P. (2016). Emotion Recognition With Python, OpenCV and a Face Dataset. A tech blog about fun things with Python and embedded electronics. Retrieved from: http://www.paulvangent.com/2016/04/01/emotion-recognition-with-python-opencv-and-a-face-dataset/
