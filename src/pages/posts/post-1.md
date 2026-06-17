---
layout: ../../layouts/MarkdownPostLayout.astro

title: "'Master's Thesis"
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

### Training Details

Two models were trained one with 5.6 degree resolution and another with 1.5 degree.

Different loss functions were tested to check the effects of various loss terms used in the graphcast and forecastnet paper, and finally based on the results a final loss function was used for 1.5 degree model training. 

The idea was to quickly train 5.6 model, infer the training dynamics and hyperparameter ranges and then use them as a starting point for the final 1.4 degree model train run.



#### 5.6

Add the train test dev table of dates,
Add the loss functions tested along with the results

MSE Loss
The MSE Loss lead to the 

After each loss function explain what is happening 
Add the list of variables used in training

#### 1.4
Explain the problems faced
Explain the dataloader part

$\degree$

### Infodump

Here, explain the zarr format and why efficient dataloading is important

Explain another motivation for doing this project.

