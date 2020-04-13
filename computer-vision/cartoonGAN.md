# CartoonGAN: Generative Adversarial Networks for Photo Cartoonization

Link to paper: http://openaccess.thecvf.com/content_cvpr_2018/papers/Chen_CartoonGAN_Generative_Adversarial_CVPR_2018_paper.pdf

## Contributions:

- A dedicated GAN-based approach that effectively learns the mapping from real-world photos to cartoon images using unpaired image sets for training
- Able to generate high-quality stylized cartoons
- When cartoon images from individual artists are used for training, our method is able to reproduce their styles
- Generative network uses a semantic loss defined as an $\ell_1$ sparse regularization in the high-level feature maps of the VGG network while discriminator network uses an edge-promoting adversarial loss for preserving clear edges
- Initialization phase leads to faster convergence of the network to the target manifold



## CartoonGAN Design

A GAN framework consists of two CNNs:

- Generator $G$: trained to produce output that fools the discriminator
- Discriminator $D$: classifies whether the image is from the real target manifold or synthetic

Solve the min-max problem
$$
(G^*,D^*)=\text{arg}\min_G\max_D\mathcal{L}(G,D)
$$
[Recommended: An intuitive introduction to GAN (Chinese)](https://zhuanlan.zhihu.com/p/33752313)

Mapping function which maps the photo manifold $\mathcal{P}$ to the cartoon manifold $\mathcal{C}$

Training data:

- $S_{data}(p)=\{p_i|i=1...N\}\subset \mathcal{P}$
- $S_{data}(c)=\{c_i|i=1...N\}\subset \mathcal{C}$

Loss function:

$\mathcal{L}(G,D)=\mathcal{L}_{adv}(G,D)+\omega\mathcal{L}_{con}(G,D)$



To comply with the fact that cartoon images have clear images, the discriminator uses binary classification that tries to assign the correct label to the following:

1. The cartoon images output by generator $G(p_k)$ and cartoon images without clear edges $e_j$
2. Real cartoon images $c_i$



### Initialization

Since the GAN model is highly nonlinear, random initialization may cause the optimization to be trapped at local minimum.

Solution: add an initialization phase (instead of initializing randomly) by pre-training the generator network $G$ with only the semantic content loss

