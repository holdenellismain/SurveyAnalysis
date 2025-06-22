# Survey Analysis using Item Response Theory

## Overview / Goals

Through my research position, I got access to data from a survey that quizzed 1,114 high school freshmen on content related to college preparedness. A recreation of the survey can be found [here](https://docs.google.com/forms/d/1qQ4x7E3lhg1lnDmDcqt0PZ0bHI6XLBO1VC2TfCulyos/edit) for context. 

My goal was to measure exactly how well the survey performs at measuring student preparedness and how it can be improved.

### Latent Variable Modelling Explained

Preparedness was modelled using latent variable. This is an unsupervised machine learning technique which imagines binary responses, $x_i$ (in this case, correct or incorrect) as tied to a normally distributed latent variable, $z_n$ (in this case, preparedness of a student). The probability of answering correctly is modelled by the following equation
```math
P(x_i=1|z_n)=c_i + (1-c_i)g(a_i(z_n-\beta_i))
```
The other parameters here are queston specfiic (indexed by $i$). $c_i$ is the probability of guessing a question correctly, which sets a floor on the probability. For example, a 4 option multiple choice would have $c_i=0.25$. $\beta_i$ is the difficulty of a question, here a very low difficulty (ex: $-20$) indicates a very easy question. $a_i$ is called the discrimination parameter, which ideally should be high for all questions. Lastly, the function $g$ maps $a_i(z_n-\beta_i) \in \mathbb{R}$ to a probability in $[0,1]$

## Skills / Tools Used

- R
- Latent Variable Modelling

## Steps

## Results



## Sources
