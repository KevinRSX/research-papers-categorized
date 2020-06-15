# Attention-aware Multi-stroke Style Transfer

## Resources

Link to paper: http://openaccess.thecvf.com/content_CVPR_2019/papers/Yao_Attention-Aware_Multi-Stroke_Style_Transfer_CVPR_2019_paper.pdf

## Introduction

Major (conflicting) problems of neural style transfer by Gatys et al. (this point is argued by a lot of related papers)

- Inefficient (hundreds of iterations in testing stage)
- Restricted number of styles
  - Coordinate high-level statistical distribution between both content and style features (AdaIN, WCT): introduce unexpected or distorted patterns because of treating diverse image regions in an indiscriminative way
  - Swap content image patch with the closest style image patch at the intermediate layer of a trained autoencoder (StyleSwap, Avatar-Net): insufficient style/inconsistent spatial distribution of visual attention
- Stroke Pyramid: lacking of details, inflexible to handle arbitrary styles in one feedforward pass



AAMS:

- Self-attention: capture salient characteristics and long-range region relations of the input image
- Multi-style style swap: break the limitation of fixed receptive field in high-level feature space and produce multiple feature maps reflecting different stroke patterns
- Fusion: integrate multiple stroke patterns into different spatial regions of the input image harmoniously



## Implementation

### Model

In `net/aams.py`

- [x] `transfer(self, contents, styles, inter_weight=1.0)`
- [x] `decode(x, encode_features, reuse=False)`
- [ ] `self_attention_autoencoder(x)`
- [x] `self_attention(x, size, scope='self_attention', reuse=False)`
- [ ] `multi_scale_style_swap(content_features, style_features, patch_size=5)`
- [ ] `multi_stroke_fusion(stylized_maps, attention_map, theta=50.0, mode='softmax')`
- [ ] `build_graph(self, x)`



In `net/util.py` (utility functions)

- [ ] `adain_colarization()`
- [ ] 