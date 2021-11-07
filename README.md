# Real-Time-Social-Distancing-Detector

## What is social distancing?

![](https://929687.smushcdn.com/2407837/wp-content/uploads/2020/05/social_distance_detector_example.png?lossy=1&strip=1&webp=0)

Social distancing is a method used to control the spread of contagious diseases.

As the name suggests, social distancing implies that people should physically distance themselves from one another, reducing close contact, and thereby reducing the spread of a contagious disease (such as coronavirus):

![](https://929687.smushcdn.com/2407837/wp-content/uploads/2020/05/social_distance_detector_spread.gif?size=660x396&lossy=1&strip=1&webp=0)

Social distancing is arguably the most effective nonpharmaceutical way to prevent the spread of a disease — by definition, if people are not close together, they cannot spread germs.


# **We can use OpenCV, computer vision, and deep learning to implement social distancing detectors.**

The steps to build a social distancing detector include:

1) Apply object detection to detect all people (and only people) in a video stream (see this tutorial on building an OpenCV people counter).
2) Compute the pairwise distances between all detected people.
3) Based on these distances, check to see if any two people are less than N pixels apart.

Webcam is used to capture the video and detect people in real-time. If people are very close to each other, a red bounding box is displayed around them indicating that they are not maintainting social distance.


## Execute

Use the following command to execute the code.
```
python social_distance_detection.py --prototxt SSD_MobileNet_prototxt.txt --model SSD_MobileNet.caffemodel --labels class_labels.txt
```

## Model

The MobileNet SSD model can detect 20 objects. The list of objects that can be detected can be found in the [class_labels.txt](class_labels.txt) file.
You can also load any pre-trained model from Deep Learning frameworks like Caffe, Tensorflow, Torch and Darknet.

## Method

Single Shot object Detection (SSD) using MobileNet and OpenCV were used to detect people. A bounding box is displayed around every person detected. 

To detect the distance of people from camera, triangle similarity technique was used. Let us assume that a person is at a distance D (in centimetres) from camera and the person's actual height is H (I have assumed that the average height of humans in 165 centimetres). Using the object detection code above, we can identify the pixcel height P of the person using the bounding box coordinates. Using these values, the focal length of the camera can be calculated using the below formula:

```
Eq 1: F = (P x D) / H
```
After calculating the focal length of the camera, we can use the actual height H of the person, pixcel height P of the person and focal length of camera F to calculate the distance of the person from camera. Distance from camera can be calculated using:

```
Eq 2: D' = (H x F) / P
```
Now that we know the depth of the person from camera, we can move on to calculate the distance between two people in a video. There can be n number of people detected in a video. So the Euclidean distance is calculated between the mid-point of the bounding boxes of all the people detected. By doing this, we have got our x and y values. These pixcel values are converted into centimetres using Eq 2.

We have the x, y and z (distance of the person from camera) coordinates for every person in cms. The Euclidean distance between every person detected is calculated using the (x, y, z) cordinates. If the distance between two people is less than 2 metres or 200 centimetres, a red bounding box is displayed around them indicating that they are not maintaining social distance. The object's distance from camera was converted to feet for visualization purpose.

![](https://github.com/Subikshaa/Social-Distance-Detection-using-OpenCV/raw/master/demo.gif)

