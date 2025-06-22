# Survey Analysis using Item Response Theory

## Overview / Goals

Through my research position, I got access to data from a survey that quizzed 1,114 high school freshmen on content related to college preparedness. A recreation of the survey can be found [here](https://docs.google.com/forms/d/1qQ4x7E3lhg1lnDmDcqt0PZ0bHI6XLBO1VC2TfCulyos/edit) for context. 

My goal was to measure exactly how well the survey performs at measuring student preparedness and how it can be improved.

## Skills / Tools Used

- R
- Latent Variable Modelling

## Steps

### The IRT Model Explained

Preparedness was modelled using latent variable. This is an unsupervised machine learning technique which imagines binary responses, $x_i$ (in this case, correct or incorrect) as tied to a standard normally distributed latent variable, $z_n$ (in this case, preparedness of a student). The probability of answering correctly is modelled by the following equation
```math
P(x_i=1|z_n)=c_i + (1-c_i)g(a_i(z_n-\beta_i))
```
The other parameters here are queston specfiic (indexed by $i$). $c_i$ is the probability of guessing a question correctly, which sets a floor on the probability. For example, a 4 option multiple choice would have $c_i=0.25$. $\beta_i$ is the difficulty of a question, here a very low difficulty (ex: $-20$) indicates a very easy question. $a_i$ is called the discrimination parameter, which ideally should be high for all questions. Lastly, the function $g$ maps $a_i(z_n-\beta_i) \in \mathbb{R}$ to a probability in $[0,1]$. This can be many functions but the sigmoid is usually chosen.

The this probability function for each question is called the Item Characteristic Curve (ICC).

![ICC](https://github.com/user-attachments/assets/eab64a4f-6f35-4cdc-8db6-6d2a7062e910)

Isolating for a single question helps build intuition for these functions. The x-axis represents the latent variable of ability, $z_i$. Respondents with low values of $z_i$ as well as the average respondent at 0 are only going to answer correctly based on the guessing parameter of 25%. The far right side represents a respondent with exceptional ability, who is almost guarenteed to answer correctly. Around the middle is the interesting area, where an individual at around 1.5 standard deviations above the average ability will answer correctly about 50% of the time. Around this ability level is where the question will be most informative to the respondent's true ability.

### Method

All code used for modelling is in [irt.rmd](irt.Rmd)
1. Check the unidimensionality assumption. The chosen IRT model assumes that responses are based on a singular latent variable, so this is checked by fitting a different IRT model with 2 latent variables and comparing it using Anova to a single variable model.
2. Fit the three-parameter model with the guessing parameter locked based on the number of choices for each question.
3. Analyze the estimated parameter coefficients and the IICs for each question.

## Results

The main result of this analysis is the table below

![table](https://github.com/user-attachments/assets/3204c4e8-6600-49a2-b97b-7bd03ea0c49d)

Looking into some of the better discriminating questions, you can get a visual of *where* they disciminate with the ICC.

![disc_qs_icc](https://github.com/user-attachments/assets/559ac0dd-37c1-4cab-a125-4ddecd3861d0)

This allows me to make the following recommendations for future quizzes:
- qz_tf_ag is far too easy with the average student answering it correctly 95% of the time. This isn’t necessarily bad, but it also discriminates very poorly between below and above-average preparedness. While it could be removed from the quiz, changing it to a 4-option multiple choice would reduce the success of guessing and improve discrimination.
- qz_coll_prep should also be removed or modified since it is drastically harder than any other question in the quiz, and of the information it provides, only 2.5% is for students within 2 standard deviations of the mean preparation.
- qz_art has exactly 25% correct answers and its lack of discrimination indicates that this is because most students are just guessing. This question should be removed.

## Sources

- Rizopoulos, D. (2006). ltm: An R package for latent variable modelling and item response theory analyses. Journal of Statistical Software, 17(5), 1–25. https://doi.org/10.18637/jss.v017.i05
- Phil Chalmers (2021). mirt: A Multidimensional Item Response Theory Package for the R Environment. Version 1.44.0. https://doi.org/10.18637/jss.v048.i06
- Bock, R. D., & Gibbons, R. D. (2021). Item response theory (First edition.). John Wiley & Sons, Incorporated.
