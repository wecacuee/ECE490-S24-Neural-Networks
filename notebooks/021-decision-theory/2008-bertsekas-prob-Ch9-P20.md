---
title: ECE490 HW5
author: Vikas Dhiman
header-includes: |
    \usepackage[margin=1in]{geometry}
---

#### HW5 Problem 1
(20 marks)

By following the Example 9.13 given below, solve the following problem (equivalent to Problem 20 from Bersekas):

A random variable $X$ is characterized by a normal PDF with mean
$\mu_0 = 20$, and a variance that is either $\sigma_0 = 16$ (negative class  $Y = 0$) or $\sigma_1= 25$ (positive $Y=1$). We want to develop a likelihood test to distinguish between two classes, using three sample values $x_1$, $x_2$, $x_3$.
Show that the likelihood ratio test is equivalent to picking class,
$$ \hat{Y}(x) = \begin{cases} 1 & \text{ if } x_1 + x_2 + x_3 > \gamma \\
0 & \text{otherwise}
\end{cases} $$
for some scalar $\gamma$. Determine the value of $\gamma$ so that the probability of False positive rate is 0.05. What is the corresponding True positive rate?


#### Terminology translation 

1. Read [Section 9.3 from Bertsekas (Intro to Probability)](https://courses.maine.edu/d2l/le/content/330408/viewContent/8957198/View) (ungraded)

Bertsekas book uses different terminology than what was covered in the
class.

1. The two Hypothesis are equivalent to two classes. $H_1$ is the positive class
($Y = 1$) and $H_0$ is the negative class ($Y = 0$).

2. The Rejection region contains all the $x$ for which the Likelihood ratio test
predicts the class of $x$ to be $H_0$.

$$ R = \text{ all the } x \text{ such that } L(x) \le \eta $$ where $L(x) =
\frac{f_x(x | Y = 1)}{f_x(x | Y = 0)}$ is the likelihood ratio.



#### Similarities between Problem 20 and Example 9.13
1. Both the problems deal with multiple Gaussian random variables. Example 9.13
deals with two Gaussian random variables $X_1$ and $X_2$. Problem 20 tdeals with three
Gaussian random variables $x_1$, $x_2$, $x_3$. The samples of a random
variable have the same distribution.

2. Only the parameters (mean and variance ) of a Gaussian distributions in Example 9.13 and Problem 20 are different.

    a) Example 9.13 has mean = 0 and standard deviation = 1 (unit variance) under $Y=0$.
    b) Example 9.13 has mean = 2 and standard deviation = 1 (unit variance) under $Y=1$.
    c) Problem 20 has mean = 20 and standard deviation = $\sigma_0 = 16$ under $Y=0$.
    d) Problem 20 has mean = 20 and standard deviation = $\sigma_1 = 25$ under $Y=1$.

3. Both problems first find $\gamma$ from false rejection probability
   $P(\hat{Y}(x) = 1 | Y = 0)$ aka false positive rate. Both in Example 9.13 and
   Problem 20, the false rejection probability is 0.05.

4. Both problems use $\gamma$ to find false acceptance probability aka false
   negative rate $P(\hat{Y}(x) = 0 | Y = 1)$.

#### Example 9.13 (from Bertsekas Section 9.3)

We observe two i.i.d. (independent identically distributed) random variables
$X_1$ and $X_2$, with unit variance. Under negative class $Y=0$ the random
variables have common mean as 0; under positive class $Y=1$ their common mean
is 2. We fix the false positive rate to $\alpha = 0.05$.
Show that the likelihood ratio test is equivalent to picking class,
$$ \hat{Y}(x) = \begin{cases} 1 & \text{ if } x_1 + x_2 > \gamma \\
0 & \text{otherwise}
\end{cases} $$
for some scalar $\gamma$. Determine the value of $\gamma$. What is the corresponding True positive rate?

#### Solution to 9.13

1. Definition 1: Gaussian PDF with mean $\mu$ and standard deviation $\sigma$ is
   given by $f_X(X=x) = \frac{1}{\sigma\sqrt{2\pi}}\exp(-\frac{(x-\mu)^2}{2\sigma^2})$.

2. Theorem 1: Joint Gaussian PDF for two __independent__ random variables $X_1$ and $X_2$
   is the product of the two distributions,

    $$f_{X_1, X_2}(x_1, x_2) = f_{X_1}(x_1)f_{X_2}(x_2)$$
    $$f_{X_1, X_2}(x_1, x_2) = 
    \frac{1}{\sigma_1\sqrt{2\pi}}\exp\left(-\frac{(x_1-\mu_1)^2}{2\sigma_1^2}\right) 
    \frac{1}{\sigma_2\sqrt{2\pi}}\exp\left(-\frac{(x_2-\mu_2)^2}{2\sigma_2^2}\right)
    $$

    Multiplication of two exponents causes addition in the radix
    $\exp(a)\exp(b) = \exp(a + b)$.


    $$f_{X_1, X_2}(x_1, x_2) = 
    \frac{1}{\sigma_1\sigma_2 2\pi}\exp\left(
    -\frac{(x_1-\mu_1)^2}{2\sigma_1^2}-\frac{(x_2-\mu_2)^2}{2\sigma_2^2}
    \right)
    $$

3. Let the mean and standard deviation of i.i.d (independent and identically
   distributed) random variables $X_1$ $X_2$ under positive class $Y=1$ be $\mu_1$
   and $\sigma_1$ respectively. Same be $\mu_0$ and $\sigma_0$ under
   negative class $Y=0$. The the Likelihood ratio is given by:

   $$ L(x) = \frac{f_{X_1,X_2}(x_1,x_2 | Y=1)}{f_{X_1, X_2}(x_1, x_2 | Y=0)} $$


   Under positive class $Y=1$ the joint distribution is,

    $$ 
    f_{X_1,X_2}(x_1, x_2 | Y=1) = f_{X_1}(x_1 | Y=1)f_{X_2}(x_2 | Y=1)
    = \frac{1}{\sigma_1^2 (2\pi)}
    \exp\left(
    - \frac{(x_1 - \mu_1)^2}{2\sigma_1^2}
    - \frac{(x_2 - \mu_1)^2}{2\sigma_1^2}
    \right)
    $$
    
    Under Hypothesis $Y=0$ the joint distribution is,
    
    $$ 
    f_{X_1,X_2}(x_1, x_2 | Y=0) = f_{X_1}(x_1 | Y=0)f_{X_2}(x_2 | Y=0)
    = \frac{1}{\sigma_0^2 (2\pi)}
    \exp\left(
    - \frac{(x_1 - \mu_0)^2}{2\sigma_0^2} 
    - \frac{(x_2 - \mu_0)^2}{2\sigma_0^2}
    \right)
    $$

    The Likelihood ratio is (again using $\frac{\exp(a)}{\exp(b)} = \exp(a -
    b)$),

    $$
    L(x) = \frac{f_{X_1,X_2}(x_1,x_2 | Y=1)}{f_{X_1, X_2}(x_1, x_2 | Y=0)}
    = \frac{\sigma_0^2}{\sigma_1^2}
    \exp\left(
    - \frac{(x_1 - \mu_1)^2}{2\sigma_1^2}
    - \frac{(x_2 - \mu_1)^2}{2\sigma_1^2}
    + \frac{(x_1 - \mu_0)^2}{2\sigma_0^2}
    + \frac{(x_2 - \mu_0)^2}{2\sigma_0^2}
    \right)
    $$

4. Likelihood ratio test is the comparison of the Likelihood ration $L(x)$
   with some scalar $\eta$. We pick the positive class if $L(x) > \eta$.
   Equivalently, we pick positive class $\hat{Y}(x) = 1$ if,
    $$
    L(x) 
    = \frac{\sigma_0^2}{\sigma_1^2}
    \exp\left(
    - \frac{(x_1 - \mu_1)^2}{2\sigma_1^2}
    + \frac{(x_1 - \mu_0)^2}{2\sigma_0^2}
    - \frac{(x_2 - \mu_1)^2}{2\sigma_1^2}
    + \frac{(x_2 - \mu_0)^2}{2\sigma_0^2}
    \right) > \eta.
    $$

    Since $\frac{\sigma_0^2}{\sigma^1}$ is positive, we can move this to other
    side of inequality,
    $$
    \exp\left(
    - \frac{(x_1 - \mu_1)^2}{2\sigma_1^2}
    - \frac{(x_2 - \mu_1)^2}{2\sigma_1^2}
    + \frac{(x_1 - \mu_0)^2}{2\sigma_0^2}
    + \frac{(x_2 - \mu_0)^2}{2\sigma_0^2}
    \right) > \eta
    \frac{\sigma_1^2}{\sigma_0^2}.
    $$

    Because $\log(z)$ is an increasing function, we can take $\log$ on both
    sides,
    $$
    - \frac{(x_1 - \mu_1)^2}{2\sigma_1^2}
    - \frac{(x_2 - \mu_1)^2}{2\sigma_1^2}
    + \frac{(x_1 - \mu_0)^2}{2\sigma_0^2}
    + \frac{(x_2 - \mu_0)^2}{2\sigma_0^2}
     > \log(\eta) + \log(\sigma_1^2) - \log(\sigma_0^2)
    $$
