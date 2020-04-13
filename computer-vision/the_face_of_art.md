# The Face of Art: Landmark Detection and Geometric Style in Portraits

Link to paper: http://www.faculty.idc.ac.il/arik/site/foa/The_Face_of_Art.pdf

## Motivation

- Most algorithms for landmark detection that work well for natural images fail when applied to more realistic inputs in two domains: texture appearance differences, and geometric differences
- Large scale datasets that are necessary to train neural networks are unavailable



## Contributions

- A method for artistic facial feature detection based on neural networks
- The collection of an “artistic-faces” dataset for future research
- A method for analyzing the geometric style of artists and portraits, creating a signature of style
- **A method for geometry-aware style transfer for portraits** (our project may focus on this)



## Method

Base: ECT approach: estimation, correction, and tuning:

- Estimation: compute a global localization of each landmark based on the peak response points in the response maps, which are learned using a fully convolutional network
- Correction: a more accurate initial shape is computed by correcting outlier landmarks using a pre-trained point distribution model (PDM)
- Tuning: Landmark locations are fine-tuned based on weighted regularized mean shift

<img src="C:/Users/Kevin/Desktop/research-papers-categorized/assets/images/faceofart-ect-process.png" style="zoom:75%;" />

This work enhances the ETC framework by:

- using artistic augmentation in training procedure
- adding an STN component to the network and using an feature-based correction step to reduce the dependency between the different facial features



We focus on the artistic augmentation step:

- texture augmentation: needs style image and accept input for portrait only
- geometric augmentation: geometrically transform the landmarks



## Inspiration

What relates this paper with our style transfer project is its augmentation step. However, the input needs style image and portraits should be of some certain size (256*256), which is not especially helpful when dealing with the edges. Geometric augmentation is more focused on the utilization in accurate landmark detection.