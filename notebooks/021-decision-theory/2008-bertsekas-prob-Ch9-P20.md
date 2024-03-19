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



<!--
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
-->

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

3. Theorem 2: Addition of two i.i.d Gaussian random variables $X_1$ and
   $X_2$ with mean and variance $\mu_X$ and $\sigma_X^2$. Note that
   identically distributed means that they have the same distribution, and
   hence same mean and variance.

    The probability distribution of $Z = X_1 + X_2$ is a Gaussian (proof
    omitted here) with
    mean $\mu_Z = E[Z] = E[X_1 + X_2] = E[X_1] + E[X_2] = 2\mu_{X}$ 
    and the standard deviation (use $\sigma_Y^2 = \text{Var}[Y] = E[Y^2] - E[Y]^2$)
    $$\sigma_Z^2
    = E[(X_1+X_2)^2] - \mu_Z^2
    = E[X_1^2 + X_2^2 + 2X_1X_2] + \mu_Z^2
    = E[X_1^2] + E[X_2^2] + 2E[X_1X_2] - \mu_Z^2
    $$
    Note that for independent random variables $X_1$ and $X_2$, $E[X_1X_2] =
    E[X_1]E[X_2] = \mu_{X}^2$. Also, $E[X_1^2] = \sigma_{X}^2 + \mu_{X}^2 = E[X_2^2]$.
    We get 
    $$ \sigma_Z = \sqrt{2\sigma_{X}^2 + 2\mu_{X}^2 + 2\mu_X^2 - \mu_Z^2}
    = \sqrt{2\sigma_{X}^2 + 2\mu_{X}^2 + 2\mu_X^2 - 4\mu_X^2}
    = \sqrt{2}\sigma_X$$

    In general when we add $n$ i.i.d. Gaussian random variables, the mean
    of the sum $Z_n = X_1 + \dots + X_n$ gets multiplied by $n$ ($\mu(Z_n) = n\mu_X$) and standard deviation gets multiplied by $\sqrt{n}$ ($\sigma(Z_n) = \sqrt{n} \sigma_X$).

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
     > \log(\eta
    \frac{\sigma_1^2}{\sigma_0^2})
    $$

5. We can substitute the given values for this Example 9.13 problem, where
   $\mu_0 = 0$, $\mu_1 = 2$, $\sigma_0 = \sigma_1 = 1$ (unit variance). We
   pick the positive class $\hat{Y}(x) = 1$ if,
   $$ 
    - \frac{(x_1 - 2)^2}{2}
    - \frac{(x_2 - 2)^2}{2}
    + \frac{(x_1 )^2}{2}
    + \frac{(x_2 )^2}{2}
     > \log(\eta
    \frac{\sigma_1^2}{\sigma_0^2})
    $$

   $$ 
    - (x_1^2 - 2x_1 + 4)
    - (x_2^2 - 2x_2 + 4)
    + (x_1 )^2
    + (x_2 )^2
     > 2\log(\eta
    \frac{\sigma_1^2}{\sigma_0^2})
    $$
    
   $$ 
    + 2x_1 - 4
    + 2x_2 - 4
     > 2\log(\eta
    \frac{\sigma_1^2}{\sigma_0^2})
    $$
    
   $$ 
    + 2x_1 - 4
    + 2x_2 - 4
     > 2\log(\eta
    \frac{\sigma_1^2}{\sigma_0^2})
    $$

   $$ 
    x_1 + x_2
     > \log(\eta
    \frac{\sigma_1^2}{\sigma_0^2}) + 4
    $$
    
    Let $\gamma = \log(\eta \frac{\sigma_1^2}{\sigma_0^2}) + 4$.
    
    In summary,
    $$ \hat{Y}(x)
    = \begin{cases} 1 & \text{if } L(x) > \eta \\
    0 & \text{otherwise}
    \end{cases}
    = \begin{cases} 1 & \text{if } x_1 + x_2 > \gamma \\
    0 & \text{otherwise}
    \end{cases}
    $$

7. We are given the false positive rate:
    $$ P(\hat{Y}(x) = 1 | Y = 0) = 0.05 $$ 
    By liklihood ratio test, $\hat{Y}(x) = 1$ when $x_1 + x_2 > \gamma$.
    $$ P(X_1 + X_2 > \gamma | Y = 0) = 0.05 $$ 
    We swapped $x_1$ (and $x_2$) with $X_1$ (and $X_2$) to highlight that $X_1$ (and $X_2$) is a random variable.
    Use Theorem 2 to get $\mu_Z = 2 \mu_X$ and $\sigma_Z = \sqrt{2} \sigma_X$.
    Normal tables allow us to look values for zero mean unit normal
    distributions.
    $$ P\left(\frac{X_1 + X_2-\mu_Z}{\sigma_Z} > \frac{\gamma - \mu_Z}{\sigma_Z} \Big| Y = 0\right) = 0.05 $$ 
    $$ 1-P\left(\frac{X_1 + X_2-\mu_Z}{\sigma_Z} \le \frac{\gamma - \mu_Z}{\sigma_Z} \Big| Y = 0\right) = 0.05 $$ 
    $$ \Phi\left(\frac{\gamma - \mu_Z}{\sigma_Z}\right) = 1-0.05 = 0.95 $$ 

    Looking at the normal tables, $\Phi(1.65) = 0.95$, hence 
    $\frac{\gamma - \mu_Z}{\sigma_Z} = 1.65$ 
    or $\gamma =  1.65 \sigma_Z = 1.65 \sqrt{2} \sigma_X = 1.65 \sqrt{2}
    \sigma_0  = 2.331$.

    In summary, when $\gamma = 2.331$, the false positive rate is 0.05.

8. To find True positive rate, use $\gamma = 2.331$
   $$P(\hat{Y} = 1|Y=1) = P(X_1 + X_2 > \gamma | Y = 1)
= 1 - P\left(\frac{X_1 + X_2 - \mu_Z}{\sigma_Z } \le \frac{\gamma - \mu_Z}{\sigma_Z} | Y = 1\right)$$

   When $Y=1$, the $\mu_X = \mu_1 = 2$ and $\sigma_X = \sigma_1 = 1$. Hence
   $\mu_Z = 2\mu_X = 4$ and $\sigma_Z = \sqrt{2} \sigma_X = \sqrt{2}$.
   With these values, the True positive rate is,
   $$ P(\hat{Y} = 1|Y=1) = 1-\Phi\left(\frac{\gamma - \mu_Z}{\sigma_Z} \right). $$
   or 
   $$ P(\hat{Y} = 1|Y=1) = 1-\Phi\left(\frac{2.331- 4}{\sqrt{2}} \right) = 1-\Phi(-1.180) =
   1-(1-\Phi(1.180)) = \Phi(1.180) = 0.964$$

   In summary, when $\gamma$ is chosen so that false positive rate is $0.05$,
   then $\gamma = 2.331$ and true positive rate is $0.964$.
