---
layout: ../../layouts/MarkdownPostLayout.astro

title: 'My First Blog Post'
pubDate: 2022-07-01
description: 'This is the first post of my new Astro blog.'
author: 'Vishnu'
image: 'https://docs.astro.build/assets/rose.webp'

tags: ["astro", "blogging", "learning in public"]
---


---


Weather prediction is an incredibly complex and computationally heavy problem. In order to predict future states we first need to know the current weather state in detail along with all the underlying physics and the constraints it enforces on the variables. Both of these -the states as well as the physical models - aren't available to us in their entirety.  So, we have to use this incomplete information and still distribute forecasts that are reliable and can be used to make decisions.

Traditionally, this problem is solved used Numerical weather prediction and data assimilation. We use data assimilation techniques to fill in the incomplete weather information and generate a complete climate state of earth, followed by numerically solving various physics models to produce an actionable forecast. As mentioned above, this process has an enormous computational cost and is only possible on supercomputer clusters.

---

### DLWP
Deep learning based weather prediction aims to solve this task using all the tools and architectures developed by the deep learning community in the last decade. The central focus of this paradigm has always been to using enormous high quality data to solve the problems at hand. Weather is a good match for such approach as various high quality datasets like ERA5 already exist.  So now the architecture becomes an important variable that should be experimented with.  Various DLWP models exist each with their own design choices and inductive biases. Graphcast from Google deepmind uses a hierarchical graph neural network to gather data and form association of various locations on earth for prediction. Neural operators based approaches takes this idea a bit further by implicitly learning a kernel to compute associations.

In my Master's thesis I trained and benchmarked the Spherical Fourier Neural Operator model for regional weather forecasting. I developed a complete training and evaluation pipeline, including data preprocessing, dataset construction, model training, curriculum training and evaluation calculation.

### Model architecture

```python
model = SFNO(
    operator_type = 'driscoll-healy',  
    img_size      = (nlat, nlon),      
    num_layers    = 8,
    embed_dim     = 384,
    scale_factor  = 2,
    pos_embed     = "latlon",          
    use_mlp       = True,
    activation_function   = "gelu",
    normalization_layer   = "layer_norm",
    hard_thresholding_fraction = 1,    
)
```

### Model hyperparameters

| Hyperparameter | Value | Role |
|---|---|---|
| `num_layers` | 8 | Depth; more layers = more complex spatial interactions |
| `embed_dim` | 384 | Width of the internal feature representation |
| `scale_factor` | 2 | Channel expansion inside each layer's MLP block |
| `hard_thresholding_fraction` | 1.0 | Fraction of spherical harmonic modes retained (1 = all) |

### Training dynamics

I trained two different models one on $5.6$ and another on $1.5$.

#### 5.6



### Infodump

