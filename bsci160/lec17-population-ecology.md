# Population Ecology

## Forms of population growth

### Exponential

- A population's per-individual growth rate (r) stays the same regardless of population size
- Population size has no stopping point, so it continuously grows
- Equations:
  - $`\frac{dN}{dt} = r^*N`$
  - $`N_{t+1} = N_t+ (r*N_t)`$
  - $`N(t) =`$ population size after time "t" $`= N_0*(r+1)^t`$ where $`N_0`$ is starting population size, t = number of time steps/years
- Exponential growth is common for early growing populations, catastrophically reduced populations in the process of rebounding, introduced populations, and lab populations

### Logistic

- Occurs when the growth rate (r) decreases as the population reaches carrying capacity (a species' average population size in a particular habitat)
  - Carrying capacity can be limited by environmental factors like adequate food, water, environmental conditions, etc.
- Most populations grow quickly at low denity, but growth rate slows at higher desity
- Once the population gets large enough, the population will stop growing altogether (r becomes 0)
- Equation: 
  - $`\frac{dN}{dt} = r_{max} N \frac{K-N}{K}`$, where N is the population size and K is the carrying capacity
    - When N is 0, exponential growth occurs
    - When N = K, no growth occurs
    - (K-N)/K is a measure of unused space
  - $`N(t) = \frac{K*N_0e^{rt}}{K + N_0(e^{rt}-1)}`$
- Density-dependent factors:
  - Competition for food, shelter, space
  - Increased predation
  - Increased disease
  - Waste build-up
- Sometimes populations overshoot K, and there can be a delay in the effects of density dependence
- Density-independent factors
  - Seasonal changes (e.g. weather/climate, light availability)
  - Recurring catastrophes

### Cyclical

- Both density-dependent and density-independent factors affect population growth, often on regular cycles
- Population age structure: The distribution of individuals across different age groups
- Organismal senescence: Generally with increased age we see
  - Reduced fertility
  - Decline of physiological functions
  - Increased mortality
- Maximum lifespan varies widely between species, perhaps due to
  - DNA damage
  - Mutation accumulation
  - Shortening of telomeres every time cells divide until genes start to erode
  - The fact that evolution by natural selection is about living to reproduction
- Some organisms are ageless in the sense that they are able to regenerate and replace tissue, repair telomeres, or revert to an earlier stage of the life cycle
