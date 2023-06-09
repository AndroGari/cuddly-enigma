# Text Overlap Detection

Text Overlap Detection  To classify whether a creative has image and text overlap or not. Given an input creative (combination of image and text), 
it should be able to tell if the text infringes upon the image and makes a poor creative.

Model results can be viewed here: [Model Results](https://github.com/AndroGari/cuddly-enigma/tree/master/Plot%20Curves). <br>
Notebook for model can be viewed here: [Notebook](https://github.com/AndroGari/cuddly-enigma/blob/master/TextOverlapDetection.ipynb). <br>
All predictions made by the model can be viewed here: [Predictions](https://github.com/AndroGari/cuddly-enigma/tree/master/Results)

The creatives may contain an overlapping of text in an image. Some of the examples for the same are shown here.

![img](https://github.com/AndroGari/cuddly-enigma/blob/master/Results/1_Overlap.png)

## Approaches 

This can be checked with computer vision techniques. Here we discuss 4 approaches to deal with it.
**We chose Apporach 2**

### Approach 1

Simple object Detection for the target product and the text in the image. The bounding box generated for each of them will be checked if they intersect with each other, 
if yes it will be termed as `Overlapping`, if not then, `No Overlapping`.

**Problem:** In this case the bounding box is critical, if the text does not overlaps but the bounding box does, then still it is classified as an `Overlapping`. Hence
generating False Positives.

For example, here in this case, the text is not overlapping but the bounding box is.

![IMG](https://github.com/AndroGari/cuddly-enigma/blob/master/UtilRes/boundW.png)

### Approach 2

To overcome the problem in the first case, a simple solution can be to consider the overlap ratio for the bounding boxes. After a bit of testing, `10%` turns out to be
a good threshold for the overlap.

Trained custom YOLO model for target prediction. (Normal YOLO model will not consider the objects hold by the humans).

Some examples for the same are:

![IMG](https://github.com/AndroGari/cuddly-enigma/blob/master/Results/4_Overlap.png)

![IMG](https://github.com/AndroGari/cuddly-enigma/blob/master/Results/7_Overlap.png)

![IMG](https://github.com/AndroGari/cuddly-enigma/blob/master/Results/5_NoOverlap.png)

![IMG](https://github.com/AndroGari/cuddly-enigma/blob/master/Results/6_Overlap.png)

**This approach turns out to be the best computationally as well as accuracy wise. We detect all the test images correctlly with this approach.**

### Approach 3

This is a strainght forward approach where we train a classification based object detection model, to check if overlappping happens or not.

The accuracy for this model is very low, and it fails for most of the cases.

![IMG](https://github.com/AndroGari/cuddly-enigma/blob/master/UtilRes/check.png)

### Approach 4

This is a bit computationally expensive approach but very high at accuracy. Here we segment the target objects to exactly get the location of target and then segment the words as well.

![IMG](https://github.com/AndroGari/cuddly-enigma/blob/master/UtilRes/segmetn.png)

## Future Scope

Bounding boxes can be determined for the target product then points can be determined from the image and passed to a SAM model to get the accurate segmentations, this way the products where multiple objects are present are also handled. Segmentation ensures higher accuracy for intersection of the product and the text.
