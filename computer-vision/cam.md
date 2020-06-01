# Learning Deep Features for Discriminative Localization

## Resources

Paper: http://cnnlocalization.csail.mit.edu/Zhou_Learning_Deep_Features_CVPR_2016_paper.pdf



## Introduction

Assumptions:

- CNNs can behave as object detectors despite no supervision on the location of the object was provided
- Fully connected layers will make this ability lost.
- NIN and GoogleNet use global average pooling (GAP) as a structural regularizer to preserve the ability while maintaining high performance

Contributions:

- With tweaking, networks with GAP layers can retain remarkable localization ability until the final layer
- The tweaking allows identifying easily the discriminative image regions in a single forward-pass for a wide variety of tasks, even those that the network was not originally trained for.