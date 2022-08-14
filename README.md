# An investigation into the Performance of a Multi-Linear Regression  Model in Tackling Face Alignment.

## Introduction:
The task of face alignment is described as identifying the geometric structure of faces in digital images. Given the location and size of a face, the task is to determine the shape of various facial components.<sup>1</sup> In this task, a set of 42 points are used to identify the facial landmarks of the face. This report investigates the performance and design of a multi-linear regression model that is used to predict these points. The research question that this report aims to solve is, therefore, **“How suitable is a multi-linear regression model for face alignment?”**. 

A way to generate optimal key points used by SIFT for feature descriptor generation is first looked at. The report then examines how these feature descriptors are used as input to each linear regressor. Qualitative results through success and failure cases are then analyzed, followed by quantitative results that evaluate different accuracy measures and plots that are used to examine how the model performs.

## Methods:

<img width="321" alt="image" src="https://user-images.githubusercontent.com/84683922/184535547-b8404370-745c-4125-ab11-e92b6b3da709.png">

### The Model:
The model created to solve this task used 42 linear regressors to predict each point in an image separately. The model used SIFT (Scale-Invariant Feature Transform), a feature detection algorithm that helps locate the local features in an image. 

SIFT offers the following advantages which make it a great choice for solving this task:
* Features are local and resistant to viewing conditions and the presence of clutter.
* The features' distinctiveness makes matching them to other features easier.<sup>2</sup>
* Features are invariant to uniform scaling, orientation, and illumination changes.<sup>3</sup>

To use optimal key points when generating feature descriptors for each linear regressor, the mean locations of each point in the training set were first computed (Figure 1).

With the optimal point locations being available, upon the model’s initialization, each linear regressor was fitted with:
* X - The feature descriptors generated for that point in all training images, after they are converted to gray-scale to reduce complexity at the cost of slightly decreasing the matching ratio.<sup>4</sup>
* y - The point’s ground truth locations in the training images.

A predict function, which takes in a set of images, was defined. As each linear regressor predicts the locations of a single point, the predict function uses all 42 linear regressors to predict the locations of all points. The results are stored in a tensor that is reshaped to (num_of_samples, num_of_points, 2) to match the ground truth points’ shape. To evaluate the model’s predictions quantitatively, a function that calculates the euclidean distance was used (Figure 2).

<img width="628" alt="image" src="https://user-images.githubusercontent.com/84683922/184535659-9d0f2247-ef43-4531-b428-709194f6fd9e.png">

## Results & Analysis:
### Images with a high predicted points accuracy (Validation Set):

<img width="835" alt="image" src="https://user-images.githubusercontent.com/84683922/184535750-fa1969dc-a76b-423f-aea5-32cd62027dfa.png">

### Example Images Set:

<img width="756" alt="image" src="https://user-images.githubusercontent.com/84683922/184535771-869a76d7-f53f-438b-8c12-2e0de3291de7.png">

Looking at these two sets of images, we can infer multiple characteristics that accurately labeled images share:
* The face being aligned is facing the camera with no solid, non-clear, objects interfering with the camera’s view of the face.
* The face’s lightning is balanced, with the same skin tone being shared across the face as a result.
* The face can be easily distinguished from the background, either as a result of a clear separation from the background or hair surrounding the face.
* The face is located in the middle of the image, with the locations of its facial landmarks being close to the optimal locations used for feature descriptor generation (Figure 1).

The example images also give insight into what the reasons for failure are. These are investigated in the next section.


### Failure Cases (Validation Set):

<img width="565" alt="image" src="https://user-images.githubusercontent.com/84683922/184535906-e7119927-a90b-4b33-8de9-1b62ec843317.png">

### Quantitative Analysis:

<img width="569" alt="image" src="https://user-images.githubusercontent.com/84683922/184535956-dd4192a8-a808-4a94-89c1-b7dd5b6d882f.png">

Most predicted points lie within a distance of ~20 from their actual location, with a low median distance of 6.279. However, a large number of outliers is present (Figure 5), this means that if a way to handle outliers was implemented, the model will be able to perform significantly better.

<img width="524" alt="image" src="https://user-images.githubusercontent.com/84683922/184535976-345ed0bc-8130-4911-a1e6-1436721fd8c1.png">

The model gives out predictions that replicate what the regressors learned from the training images (Figure 6). The results also restate the model’s struggles with outliers as it struggles to match them to their actual locations. A linear feature representation was also able to capture the relationship of the features for all points with only one exception, point #4 (Figure 7), which needs polynomial regression for its features to be captured correctly.

## Conclusion:
In conclusion, this report showcased how a model that deals with the issue of face alignment, through the use of linear regression and SIFT feature descriptors, is designed and implemented. Qualitative results showed that having an image with balanced lighting, a face in the middle of the image, that is easily distinguishable from the background, and facing the camera, caused the model to give accurate predictions. 

The reasons for failure were also identified, these were facial feature misinterpretation, dark face edges combined with a dark background, beards, sideway oriented faces, and bad lighting. Quantitative results then showed that the model performs well with all points, except for extreme outliers and point #4, which needs polynomial regression for its features to be captured correctly.

## Bibliography:
1 Gu L., Kanade T. (2009) Face Alignment. In: Li S.Z., Jain A. (eds) Encyclopedia of Biometrics. Springer, Boston, MA. https://doi.org/10.1007/978-0-387-73003-5_186 

2 Tyagi, Deepanshu. “Introduction to SIFT( Scale Invariant Feature Transform).” Medium, 7 Apr. 2020, medium.com/data-breach/introduction-to-sift-scale-invariant-feature-transform-65d7f3a72d40.

3 Lowe, David. Object Recognition from Local Scale-Invariant Features. 1999.

4 Zhao, Yan, et al. “Image Matching Algorithm Based on SIFT Using Color and Exposure Information.” Journal of Systems Engineering and Electronics, vol. 27, no. 3, 1 June 2016, pp. 691–699, ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=7514695, 10.1109/JSEE.2016.00072. Accessed 5 May 2022.

## Appendix:
### Section 1 - Full Code:
See Face_Alignment.ipynb file.

### Section 2 - Predicted Points’ boxplots:

<img width="608" alt="image" src="https://user-images.githubusercontent.com/84683922/184536149-1feb1dc0-3a60-4315-aa1d-c41f8eb686cf.png">
<img width="608" alt="image" src="https://user-images.githubusercontent.com/84683922/184536159-db45b545-677c-47d5-8c5f-e65fae39b499.png">





