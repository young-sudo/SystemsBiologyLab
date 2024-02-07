# Systems Biology - Fisher Model

Younginn Park

# Introduction

**Individual**:
- Individuals are diploid and reproduce sexually
- They possess a Genotype - a set of genes
- $n > 2$, the number of traits/genes per individual
- Gene representation - $[0,1]$ in two copies per individual, in undefined arbitrary units
- Phenotype - expression of an individual's genes (what the Environment will "see" and based on which it will select)
- Representation of phenotype coded by a given gene - $o \in [0,1]$
- Genotype influences Phenotype (through a defined function, here: arithmetic mean: $gen \in\mathbb{R}^{n\times 2}$ $\rightarrow fen \in\mathbb{R}^{n}$, where 2 indicates diploidy)

**Population**:
- $N_0$, initial population size
- $\mu$, probability of mutation occurrence, here: for simplicity, mutations encompass the entire genome at once, not a single gene
- $\sigma$, mutation effect, standard deviation of a mutation event, $mutation \sim \mathcal{N}(0, \sigma)$

**Environment**:
- The environment has an Optimal Phenotype, $\alpha\in\mathbb{R}^n$, i.e., a set of traits that the Environment "expects" in an Individual for it to be visible if the individual intends to reproduce
- The fitness function (adaptation/fitness) of an individual's phenotype will be calculated as the distance between the Optimal Phenotype vector and the Individual's Phenotype. This value is then transformed to be within the range $[0,1]$, where: 0 - no adaptation, 1 - perfect adaptation. The function $\phi = exp(-||\alpha - o||_2)$ guarantees such value range.

**Reproduction**:
- Reproduction occurs by "passing through" the female population, randomly selecting a male individual from the male population, randomly choosing the gender of the offspring, randomly selecting one set of genes from both parents, and slightly perturbing it, $slightOffspringGenePerturbation \sim \mathcal{N}(0, \delta)$, $\delta = 10^{-4}$
- The number of offspring per parents follows a Poisson distribution, $numberOffspring \sim \mathrm{Poisson}(2.1 \times \frac{(N_0 - N)}{N_0})$, where $N_0$ is treated as the environmental carrying capacity. For simplicity, in the code, only the female population is considered, and the replacement-level fertility coefficient 2.1 is multiplied by 2.
- Only one reproduction "session" occurs per generation in the simulation

**Additional Simulation Assumptions**:
- One generation goes through the steps in the following order: mutation $\rightarrow$ selection $\rightarrow$ reproduction
- The entire old population is replaced with a new one - each individual dies after one generation

**Project Objectives**:
- Illustrating the dynamics of population size, genetic diversity within the population, and the process of natural selection
- Investigating the impact of mutation frequency and effect on survival and genetic diversity within the population
- The influence of climate change and catastrophes on population size dynamics and genetic diversity within the population

After the catastrophe, the variance of adaptation typically decreased drastically. In some cases, the population almost completely homogenized.

# Analysis

Many results obtained from the above simulations can be traced back to the specifics of the population model implementation, but some observations align with those that can be noticed "in real life".

The model reflected a gradual decrease in the variance of genetic diversity within the population over time in the absence of selective pressure. However, a somewhat perplexing behavior was the clustering of individuals into clusters with different adaptations. PCA plots of later steps also suggest that populations clustered into separate "cliques".

Another observation was that mutation probability above $10^{-2}$ most commonly resulted in population extinction (here working with a population of 500). A possible explanation for such results is the specific adjustment of parameters and constants within the model; for instance, too large mutations relative to changes in the optimum might have caused the offspring's genome to more easily "fly off" from the vicinity of the optimum. The more frequent such mutations, the more individuals poorly adapted to the environment.

Under constant environmental conditions, the population size stabilized on its own, reflecting the organisms' reproduction process in the real world, e.g., under continuous _Daphnia_ pressure, they increase the number of offspring at the expense of growth. However, an interesting observation was that in the case of catastrophes, despite using a formula to "bounce back" the population number in case of its decrease, populations failed to return to the initial state, regardless of the length of the conducted simulations. The above plots suggest that this could be related to the specific implementation of gene inheritance, where instead of independent distribution of individual genes, the model assumed the inheritance of the entire parent genome, which, coupled with a small mutation probability ($10^{-4}$), could result in a decrease in the gene pool, leading to a decrease in variance and the possibility of "improving" adaptation, especially after a catastrophe.

# Evaluation

There are a few suggestions for future implementations of such models:

- Utilize a multidimensional normal distribution, N(mu, Sigma[covariance matrix]), to diversify the behaviors of different genes, instead of treating them equally

- Implement chromosomes and their recombination

- Implement a spatial model - distribution of individuals in space. This would allow illustrating, for example, spacing caused by a geographical barrier.
