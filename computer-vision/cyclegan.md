## CycleGAN

### Introduction

Image-to-image translation: converting an image from one representation of a given scene, x to another, y, e.g., grayscale to color, image to semantic labels, edge-map to photograph

Seek an algorithm that can learn to translate between domains without paired input-output examples. For a set of image in domain $X$ and a different set in domain $Y$, we may train a mapping $G$: $X\rightarrow Y$ such that the output $\hat{y}=G(x)$, $x\in X$, is distinguished from images $y\in Y$ by an adversary trained to classify $\hat{y}$ apart from $y$. The optimal $G$ thereby translates the domain $X$ to a domain $\hat{Y}$ distributed identically to $Y$.

To resolve [mode collapse](https://developers.google.com/machine-learning/gan/problems#mode-collapse), added a cycle consistency loss that encourages $F(G(x))\approx x, G(F(y))\approx y$.



### Formulation

- $D_X$: distinguish between images $x$ and translated images ${F(y)}$; $D_Y$: distinguish between images $y$ ${G(x)}$
- Adversarial loss: match the distribution of generated images to the data distribution in the target domain
- Cycle consistency loss: prevent the learned mappings $G$ and $F$ from contradicting each other



#### Adversarial Loss

$$
\mathcal{L}_{GAN}(G,D_Y,X,Y)=E_{y\sim p_{data}(y)}[log(D_X(y))]+E_{x\sim p_{data}(x)}[log(1-D_Y(G(x))]
$$

Similar for $\mathcal{L}_{GAN}(F,D_X,Y,X)$



#### Cycle Consistency Loss

To ensure both forward cycle consistency and backward cycle consistency:
$$
\mathcal{L}_{cyc}(G,F)=E_{x\sim p_{data}(x)}[||F(G(x))-x||_1]+E_{y\sim p_{data}(y)}[||G(F(y))-y||_1]
$$


#### Full Objective

$$
\mathcal{L}(G,F,D_X,D_Y)=\mathcal{L}_{GAN}(G,D_Y,Y,X)+\mathcal{L}_{GAN}(F,D_X,X,Y)+\lambda \mathcal{L}_{cyc}(G,F)
$$

