---
title: 'glueformula: simply build regression formulas from vectors with variable names'
cover: null
date:   2020-01-09 21:00:00
categories: r
tags: [R]
---

The small new package [glueformula](https://github.com/skranz/glueformula) with a single function `gf`  facilitates constructing regression formulas from vectors with variable names. The syntax is similar to [glue](https://github.com/tidyverse/glue) strings. Here is an example:


```r
# Example: build a formula
# for ivreg with gf
library(glueformula)

# Contol variables and instruments
contr = c("x1","x2","x3","log(x4)")
instr = c("z1","z2",contr)

# Create formula for ivreg
gf(q ~ p + {contr} | {instr})
```

```
## q ~ p + x1 + x2 + x3 + log(x4) | z1 + z2 + x1 + x2 + x3 + log(x4)
```

There is no big benefit if one wants to estimate a single regression, but in econometrics one often specifies a lot of similar regressions (for robustness checks not p-hacking!) that share a large set of common control variables. Here `glueformula` can be handy.

You can install it from Github as explained here: [https://github.com/skranz/glueformula](https://github.com/skranz/glueformula)

There was also a discussion [here](https://github.com/tidyverse/glue/issues/108) whether a similar feature should be included into the `glue` package itself, but it looks as if that is not going to happen.
