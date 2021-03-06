# Priors in Bayesian analysis
FlorianHartig  
9 May 2015  






## Overview priors

The choice of prior (prior eliciation) is key to Bayesian analysis, and it is arguably the most contentious step in the whole procedure, as it supposedly contains "subjective" judgements. I disagree with this notion. The choice of a prior is not neccessarily subjective. It simply means that, unlike in a frequentist analysis, we should generally collect everything that is known about a parameter in advance, which may be done in an objective way. Also, we can try to avoid the inclusion of prior knowledge by choosing so-called uninformative (aka vague, reference) priors. So, a first thing to note about priors is that we have 

* Informative priors that express prior knowledge about an inferential question
* Uninformative priors that express no prior knowledge about an inferential question

More about the choice of uninformative priors below. But first some other statements:

* In the limit if infinitely many data, the likelihood gets infintely sharp, and therefore the prior choice irrelevant (as long as the prior is not 0 anywhere there is likelihood)
* Priors are therefore most important if you have a small dataset 
* Priors are changed by rescaling parameters (see below)
* Uninformative priors are not always flat (see below). For common problems, people have developed recommendations for which priors should be used in an uninformative setting 


## Scaling and scale-invariance of prior choices 

Scaling is key to understand why uninformative priors can't always be flat. Imagine the following situation: we have a dataset on average tree diameters, and we want to infer the average with a Bayesian method. We shouldn't really look at the data before we specify our prior, so let's just specify the prior, and assume we choose a flat prior between 1 and 10 because we don't want to bias our data in any way


```r
values = 1:5
priorWeight = rep(1/5, 5)
barplot(priorWeight, names.arg = values, xlab = "size [cm]", ylab = "priorProbability", col = "darkseagreen")
```

![](priors_files/figure-html/unnamed-chunk-2-1.png) 

Now, let's assume that we decide do change the analysis slightly, and measure average size in the basal area, which scales to diameter as x^2. We have already specified our prior knowledge about diameter, so for each cm of diameter we have specified the same weight. 

If we rescale the x-axis to basal area, the length of each bar on the x-axis changes - large values are getting broader, short values are getting more narrow. If the probability weight is to stay the same, we get the following picture:


```r
barplot(priorWeight/values^2, width = values^2, names.arg = values^2, xlab = "size [cm^2]", ylab = "priorProbability", col = "darkseagreen")
```

![](priors_files/figure-html/unnamed-chunk-3-1.png) 

The message here is that if we are free to rescale predictors as we want (which is generally true), the prior cannot be flat for all possible parameter transformations. 

A key for any rule about finding uninformative priors is therefore that the rule must be invariant under parameter transformation. 

A second message is that in Bayesian statistics, you have to be a bit careful about parameter transformations, because we don't just look at one value, but at a whole distribution, and the shape of this distribution will change of we reparameterize. 


## Default choices for uniformative priors 

So, what is the right choice for uninformative priors? The somewhat disturbing answer is that there is no generally accepted solution for this problem. One famous proposal that contains many of the desirable properties is [Jeffrey's prior](http://en.wikipedia.org/wiki/Jeffreys_prior) which is defined as 

p(phi) ~ sqrt ( det ( F(phi)))

where F(phi) is the [Fisher information matrix](http://en.wikipedia.org/wiki/Fisher_information), which basically tells you how strongly the likelihood changes if parameters change. It is easy to see that the prior choice will then be

* invariant under rescaling parameters
* proportional to how strongly parameters affect the likelihood

To me, this seems to cover the main agreements about prior choice. Unfortunately, Jeffrey's prior seems to have some problems for multivariate and hierarchical models, so it's not a general panacea.

## Conjugacy

Another issue that is often important is [conjugacy](http://en.wikipedia.org/wiki/Conjugate_prior). In Bayesian statistics, if the posterior distributions p(θ|x) are in the same family as the prior probability distribution p(θ), the prior and posterior are then called conjugate distributions, and the prior is called a conjugate prior for the likelihood function. 

Conjugacy has two main advantages:

* The shape of the posterior is known, which allows approximating it parameterically
* Many sampling methods work more efficiently

One therefore usually tries to specify conjugate priors if possible. 


## Typical uninformative choices 



1. For **scale parameters** (something that affects the output linearly, like slope or intercept in a regression), use flat or quasi flat priors such as
  * A bounded uniform distribution
  * A wide normal distribution
  
2. It is possible to put a bit tighter priors around scale parameters to get the Bayesian analogue of Lasso or Ridge regression, see 
  * Park, T. & Casella, G. (2008) 
  * Kyung, M.; Gill, J.; Ghosh, M.; Casella, G. et al. (2010) Penalized regression, standard errors, and Bayesian lassos. Bayesian Analysis, 5, 369-411.
  * http://stats.stackexchange.com/questions/95395/ridge-regression-bayesian-interpretation?rq=1


2. For **variance parameters** (something like the standard deviation in a linear regression), use decaying parameters such as
  * 1/x (standard choice according to Jeffrey's prior)
  * inverse-gamma
  
3. For **variance hyperparameters in hierarchical models**, use
  * inverse-gamma
  * half-t family (suggested by Gelman, 2006)
  

4. Other than that, in doubt, people tend to choose conjugate prior distributions


## Readings

### Uninformative priors 

Kass, R. E. & Wasserman, L. (1996) The selection of prior distributions by formal rules. J. Am. Stat. Assoc., American Statistical Association, 91, 1343-1370.

Jeffreys, H. (1946) An Invariant Form for the Prior Probability in Estimation Problems. Proceedings of the Royal Society of London. Series A, Mathematical and Physical Sciences, The Royal Society, 186, 453-461.

Jaynes, E. (1968) Prior probabilities. Systems Science and Cybernetics, IEEE Transactions on, IEEE, 4, 227-241.

Tibshirani, R. (1989) Noninformative priors for one parameter of many. Biometrika, 76, 604-608.

Park, T. & Casella, G. (2008) The Bayesian Lasso. Journal of the American Statistical Association, 103, 681-686.

Irony, T. Z. & Singpurwalla, N. D. (1997) Non-informative priors do not exist -- a dialogue with José M. Bernardo. J. Stat. Plan. Infer., 65, 159-177.

Gelman, A.; Jakulin, A.; Pittau, M. G. & Su, Y.-S. (2008) A weakly informative default prior distribution for logistic and other regression models. The Annals of Applied Statistics, JSTOR, , 1360-1383.

Gelman, A. (2006) Prior distributions for variance parameters in hierarchical models. Bayesian Analysis, Citeseer, 1, 515-533.

Fong, Y.; Rue, H. & Wakefield, J. (2010) Bayesian inference for generalized linear mixed models. Biostatistics, 11, 397-412.

Ferguson, T. (1974) Prior distributions on spaces of probability measures. The Annals of Statistics, JSTOR, 2, 615-629.


### Informative priors 

Choy, S. L.; O'Leary, R. & Mengersen, K. (2009) Elicitation by design in ecology: using expert opinion to inform priors for Bayesian statistical models. Ecology, 90, 265-277




---
**Copyright, reuse and updates**: By Florian Hartig. Updates will be posted at https://github.com/florianhartig/LearningBayes. Reuse permitted under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License

