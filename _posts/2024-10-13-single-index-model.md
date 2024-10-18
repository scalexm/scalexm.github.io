---
layout: post
title: Inconsistency of the Single Index Model
---

<span class="dropcap">R</span>eading through *Advanced Portfolio Management* by Giuseppe A.
Paleologo led me to give some thoughts to the so-called **Single Index Model** or **Diagonal
Model**, originally introduced by Sharpe[^1] in 1963, and the way it is typically exposed in
introductory texts or in online writings. I am not going to discuss the empirical validity of the
model too much, as there has been a large amount of literature on the subject and we now have
multi-factor models for a reason. Instead, I will focus on an oddity that initially made me
question the soundness of the model, how it can be resolved, and why it matters.

### A probabilistic model

There are $$n$$ risky assets with random returns $$R_1, \dots, R_n$$. In addition, the return of
the **market portfolio** is denoted by $$R_M$$. The real-life portfolio that corresponds best to
the market portfolio does not have to be specifically defined at this point, but it is at least
assumed that it contains each asset $$i$$ (or just a strict subset of the assets) in proportion
$$\omega_i > 0$$ with $$\sum \omega_i = 1$$. Finally, the dynamics of the random returns are given
by:

$$
\begin{equation}
R_i = \alpha_i + \beta_i R_M + \varepsilon_i, \quad 1 \leq i \leq n
\label{eq:model}
\end{equation}
$$

for some real numbers $$(\alpha_i, \beta_i)_{1 \leq i \leq n}$$, and
$$(\varepsilon_i)_{1 \leq i \leq n}$$ being a family of random **residuals** verifying

$$
\begin{align}
&\mathbf{E}({\varepsilon_i}) = 0 \nonumber \\
&\text{Cov}(\varepsilon_i, R_M) = 0 \label{eq:hypothesis} \\
&\text{Cov}(\varepsilon_i, \varepsilon_j) = 0, \quad i \neq j . \nonumber
\end{align}
$$

The residual standard deviation $$\sigma(\varepsilon_i) = \sqrt{\text{Var}(\varepsilon_i)}$$ is
also called the **idiosyncratic** volatility of asset $$i$$. The first two conditions in
$$\eqref{eq:hypothesis}$$ make the model equivalent to $$n$$ individual linear regressions where
observations of $$R_i$$ are regressed onto observations of $$R_M$$.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
By linearity, the notations naturally extend to any given portfolio formed from a subset
$$J \subseteq \{1,\cdots,n\}$$ of the $$n$$ assets with positive weights $$(\psi_j)_{j \in J}$$
verifying $$\sum \psi_j = 1$$. The **beta** of the portfolio is given by $$\sum \psi_j \beta_j$$,
its **alpha** by $$\sum \psi_j \alpha_j$$, and its residual by $$\sum \psi_j \varepsilon_j$$. We
can illustrate the power of diversification within the model by observing that, because the
$$(\varepsilon_j)$$ are uncorrelated, the idiosyncratic variance of such a portfolio verifies

$$
\begin{align}
\text{Var} \left( \sum\limits_{j \in J} \psi_j \varepsilon_j \right)
    &= \sum\limits_{j \in J} \psi_j^2 \mathop{\text{Var}}(\varepsilon_j) \nonumber \\
&\leq
\left( \sum\limits_{j \in J} \psi_j^2 \right)
    \mathop{\text{max}}\limits_{j \in J} \mathop{\text{Var}}(\varepsilon_j)
\label{eq:idio_ptf}
\end{align}
$$

and concluding that as the cardinal of $$J$$ increases, $$\sum \psi_j^2$$ is typically much smaller
than $$1$$ because of the relation $$\sum \psi_j = 1$$. This last approach is also used in
*Advanced Portfolio Management* to justify that the idiosyncratic volatility of the market
portfolio itself is comparatively small[^2]. However, another argument could be made for the market
portfolio: since we are regressing asset returns onto the market portfolio return, the beta of
the market portfolio is one and its residual is zero *by definition*, therefore its idiosyncratic
volatility is trivially zero. Thinking about this, I started to speculate about the presence of a
circular argument somewhere, and indeed we can observe more rigorously that

$$
\begin{align}
R_M &= \sum\limits_{i=1}^n \omega_i R_i \nonumber \\
    &= \sum\limits_{i=1}^n \omega_i (\alpha_i + \beta_i R_M + \varepsilon_i) \nonumber \\
    &= \sum\limits_{i=1}^n \omega_i \alpha_i
        + \left( \sum\limits_{i=1}^n \omega_i \beta_i \right) R_M
        + \sum\limits_{i=1}^n \omega_i \varepsilon_i
\label{eq:circular}
\end{align}
$$

then notice that for a fixed $$j \in \{1, \cdots, n\}$$, applying the second condition of
$$\eqref{eq:hypothesis}$$ and substituting $$R_M$$ with the expression from $$\eqref{eq:circular}$$
yields

$$
\begin{align}
0 &= \mathop{\text{Cov}}(\varepsilon_j, R_M) \nonumber \\
  &= \left( \sum\limits_{i=1}^n \omega_i \beta_i \right) \mathop{\text{Cov}}(\varepsilon_j, R_M)
    + \sum\limits_{i=1}^n \omega_i \mathop{\text{Cov}}(\varepsilon_j, \varepsilon_i) \nonumber \\
  &= \omega_j \mathop{\text{Var}}(\varepsilon_j) \nonumber
\end{align}
$$

where I used the fact that the covariance with a constant is zero, the second condition of
$$\eqref{eq:hypothesis}$$ once more, and the uncorrelation of the $$(\varepsilon_i)$$. Hence, we
obtain $$0 = \mathop{\text{Var}}(\varepsilon_j) = \mathop{\mathbf{E}}(\varepsilon_j^2)$$ because of
the first condition in $$\eqref{eq:hypothesis}$$, which is only possible if $$\varepsilon_j = 0$$
(almost surely), and this result holds for any $$j \in \{1, \cdots, n\}$$.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
By exploiting the assumptions of the model, we reduced it to a trivial case where every asset
return is an affine combination of the others. Surely this is not what Sharpe had in mind, and yet
after re-reading his original papers, the inconsistency was there from the beginning. Does that
mean that the model is irreparably flawed and that we cannot just add up idiosyncratic variances
to compute the variance of a portfolio, or hedge out the market risk common to all assets? In fact,
the empirical applications tend to show that we can still do these things, at least as an
approximation, because the residual cross-correlations are small: the theoretical issue really lies
in amalgamating "small" and "exactly zero". Can we just replace the third hypothesis in
$$\eqref{eq:hypothesis}$$ by "cross-correlations must be small"? Aside from not being an
appropriate way of describing a mathematical model, it also prevents saying anything quantitative
about the cases where "small" is not actually small enough.

### The CAPM and the origins of the model

I took the time to revisit the first papers written by Sharpe, from which I got a better
understanding of how his Diagonal Model came to life. Oddly enough, it is often amalgamated with
the **CAPM** published as an independent paper one year later[^3]: Sharpe initially wrote a first
version of the CAPM in terms of the Diagonal Model in his 1961 PhD dissertation[^4], but it was
rewritten completely in the 1964 paper such that the two models are now in fact independent and use
different sets of assumptions.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Fundamentally, the CAPM is just a solution to the Mean Variance Optimization problem introduced by
Markowitz, when the market is in a specific equilibrium. MVO really only relates to *expectations*
of returns rather than their dynamics, and as such the CAPM formulation is also given in terms of
expectations:

$$
\begin{equation}
\mathbb{E}(R_i) = \beta_i \mathop{\mathbb{E}}(R_M),
    \quad \beta_i = \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)}
\label{eq:capm}
\end{equation}
$$

where this time $$R_M$$ is the return of a precisely defined portfolio containing all risky assets
in the universe (Sharpe speaks of a "combination" of assets in his 1964 paper, "for which every
asset enters" said combination once the equilibrium is reached). Note that I have omitted the
risk-free rate from $$\eqref{eq:capm}$$ as a simplification, but feel free to assume that we are
talking about excess returns if that triggers you. Of course it is straightforward to go from
expectations to dynamics, just define

$$
    \varepsilon_i \triangleq R_i - \beta_i R_M
$$

and you fall back on the familiar $$R_i = \beta_i R_M + \varepsilon_i$$ with
$$\mathbf{E}(\varepsilon_i)= 0$$. Also notice that

$$
\begin{align}
    \text{Cov}(\varepsilon_i, R_M) &= \mathop{\text{Cov}}(R_i - \beta_i R_M, R_M) \nonumber \\
        &= \mathop{\text{Cov}}(R_i, R_M) - \beta_i \mathop{\text{Var}}(R_M) \nonumber \\
        &= \mathop{\text{Cov}}(R_i, R_M) - \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)}
            \mathop{\text{Var}}(R_M) \nonumber \\
        &= 0 \nonumber
\end{align}
$$

such that in the CAPM, the $$\varepsilon_i$$ are indeed uncorrelated with $$R_M$$. However, this is
purely a *consequence* of the value of $$\beta_i$$ rather than the other way around: if you read
Sharpe's paper very thoroughly, you will find that the value of $$\beta_i$$ directly stems from the
CAPM framework without ever talking about $$\varepsilon_i$$. If you are more used to Bourbaki-style
mathematics like I am, you might find [this proof][capm_beta_proof] to be easier to read (the proof
in Sharpe's paper is literally written in a footnote... *almost* reminds me of
[something else][fermat]). From there, we can derive the usual risk decomposition

$$
\begin{equation}
\text{Var}(R_i) = \beta_i^2 \mathop{\text{Var}}(R_M) + \mathop{\text{Var}}(\varepsilon_i)
\label{eq:decomposition}
\end{equation}
$$

with $$\beta_i^2 \mathop{\text{Var}}(R_M)$$ being the **systematic** risk component common to all
returns and $$\text{Var}(\varepsilon_i)$$ the **unsystematic** component. At this point, I insist
on the term "unsystematic" rather than "idiosyncratic" because nothing prevents the
$$(\varepsilon_i)$$ to be correlated *a priori*, even though they satisfy the first two conditions
in $$\eqref{eq:hypothesis}$$.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
From the equation of returns of the CAPM and its consequences in terms of risk decomposition, it
really only takes two natural leaps to go from the CAPM to Sharpe's Diagonal Model:  
* abandon the market equilibrium idea and instead focus on a general probabilistic model describing
  the dynamics of returns
* embrace the systematic *vs* idiosyncratic decomposition by adding the uncorrelation hypothesis
  of $$\eqref{eq:hypothesis}$$.

The Diagonal Model now becomes a completely independent model that simply attempts to encode the
empirical observation that market risk is the common driver of returns, free of any economic
assumptions such as investors acting rationally or asset prices being in equilibrium. It is an
*extrapolation* of the CAPM in which $$R_M$$ does not have to be the return of the exact portfolio
from the CAPM, but rather any portfolio or **factor** that is believed to drive returns in a given
universe of assets: Sharpe does leave this door open in his 1963 paper (even though he only
proceeds to apply it with actual portfolios), and this interpretation is what is going to help us
fix the theoretical soundness issue described earlier. Still, the model remains in a sense
*compatible* with the CAPM: this time as a consequence of the uncorrelatedness of $$\varepsilon_i$$
and $$R_M$$, we obtain again

$$
    \beta_i = \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)}
$$

so that if the market portfolio is chosen to be the portfolio from the CAPM, both models assign the
same value to $$\beta_i$$. It is clear to me that this feature of the Diagonal Model may have
appealed to Sharpe.

### Fama to the rescue

After stumbling onto the issue exposed in the first section, my first reflex has been to search
for answers on the internet, without a lot of success. For example at the time of writing,
[Wikipedia][wikipedia] defines the model more or less exactly as I do in my first section. The
Wikipedia page does acknowledge that *empirically*, the residual cross-correlations are not exactly
zero, but apparently without realizing that using $$\text{Cov}(\varepsilon_i, \varepsilon_j) = 0$$
as an approximation of reality makes the model so constrained as to render it trivial. I even asked
ChatGPT but needless to say, the result was disappointing (every time I ask something to ChatGPT, I
secretly hope that it gives me an incorrect answer: it makes me feel a bit less concerned about
the imminent AI takeover prophetized by X/Twitter experts).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Trying my luck at searching through old papers instead, I finally found one written by Fama in
1968[^5] that precisely discusses the issue. Fama also notices that the three conditions in
$$\eqref{eq:hypothesis}$$ cannot hold all at once. In fact, he still makes a small mistake by
stating that even the second condition cannot hold by itself[^6] (we will revisit this case in the
next and final section) but otherwise proposes a revised model that stays in the spirit of
Sharpe's: instead of regressing the $$R_i$$ directly onto $$R_M$$, Fama postulates the existence
of an unobservable random factor $$\widetilde{R}$$ and a new family of residuals
$$(\widetilde{\varepsilon}_i)$$ such that

$$
\begin{equation}
R_i = \alpha_i + \beta_i \widetilde{R} + \widetilde{\varepsilon}_i, \quad 1 \leq i \leq n
\label{eq:fama}
\end{equation}
$$

where the new residuals satisfy three conditions similar to $$\eqref{eq:hypothesis}$$:

$$
\begin{align}
&\mathbf{E}(\widetilde{\varepsilon}_i) = 0 \nonumber \\
&\text{Cov}(\widetilde{\varepsilon}_i, \widetilde{R}) = 0 \label{eq:hypothesis_fama} \\
&\text{Cov}(\widetilde{\varepsilon}_i, \widetilde{\varepsilon}_j) = 0, \quad i \neq j . \nonumber
\end{align}
$$

How is it different from the Diagonal Model? The key point is that the regressor $$\widetilde{R}$$
is not defined in terms of the $$R_i$$ anymore, it is instead an external force that drives returns
rather than an actual portfolio: it is an early example of a factor model where the underlying
factor cannot be observed directly in the market. Within Fama's model, $$R_M$$ now has a beta
*a priori* different from one, as well as non-zero alpha and residual:

$$
R_M = \underbrace{\sum\limits_{i=1}^n \omega_i\alpha_i}_{\alpha_M}
    +\underbrace{\left( \sum\limits_{i=1}^n \omega_i\beta_i \right)}_{\beta_M} \widetilde{R}
    +\underbrace{\sum\limits_{i=1}^n \omega_i\widetilde{\varepsilon}_i}_{\widetilde{\varepsilon}_M}
    .
$$

However, it can be noted that applying an affine transformation to $$\widetilde{R}$$ gives
another equivalent formulation of Fama's model, so that we can choose to have $$\alpha_M = 0$$ and
$$\beta_M = 1$$ without loss of generality and simplify the previous relation to
$$R_M = \widetilde{R} + \widetilde{\varepsilon}_M$$. Using the same argument as in
$$\eqref{eq:idio_ptf}$$, we can expect the idiosyncratic variance
$$\text{Var}(\widetilde{\varepsilon}_M)$$ to be small relative to $$\text{Var}(\widetilde{R})$$,
such that $$R_M \approx \widetilde{R}$$ illustrates the concept of **factor-mimicking portfolios**,
which are tradeable portfolios designed to have a performance approximating that of an unobservable
factor. It then makes sense to translate $$\eqref{eq:fama}$$ into a regression onto $$R_M$$ as in
Sharpe's model:

$$
\begin{align}
R_i &= \alpha_i + \beta_i \widetilde{R} + \widetilde{\varepsilon}_i \nonumber \\
    &= \alpha_i + \beta_i \left( \widetilde{R} + \widetilde{\varepsilon}_M \right)
        + \widetilde{\varepsilon}_i - \beta_i \widetilde{\varepsilon}_M \nonumber \\
    &= \alpha_i + \beta_i R_M + \varepsilon_i
\label{eq:new_regression}
\end{align}
$$

after having defined

$$
\varepsilon_i \triangleq \widetilde{\varepsilon}_i - \beta_i \widetilde{\varepsilon}_M .
$$

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
With the formulation from $$\eqref{eq:new_regression}$$, we can in theory estimate $$\beta_i$$
exactly as in the original model by regressing observations of $$R_i$$ onto $$R_M$$. However, this
regression is no longer *well-posed* in the sense that $$\varepsilon_i$$ and $$R_M$$ are not
necessarily uncorrelated. Hence, the OLS estimator of $$\beta_i$$ is no longer consistent and
instead converges to

$$
    \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)} = \frac{
        \beta_i + \omega_i \frac{
            \mathop{\text{Var}(\widetilde{\varepsilon}_i)}
        }{
            \mathop{\text{Var}(\widetilde{R})}
        }
    }{1 +
        \frac{
            \mathop{\text{Var}(\widetilde{\varepsilon}_M)}
        }{
            \mathop{\text{Var}(\widetilde{R})}
        }
    } .
$$

Assuming $$\text{Var}(\widetilde{\varepsilon}_M) \ll \mathop{\text{Var}(\widetilde{R})}$$ and
$$\text{Var}(\widetilde{\varepsilon}_i) \approx \mathop{\text{Var}(\widetilde{R})}$$, the OLS
estimator would in general still converge to a value close to the real $$\beta_i$$, but would
nevertheless tend to overshoot for assets having a large weight in the chosen portfolio. We can
also give a few other properties of the "proxy" residuals $$(\varepsilon_i)$$, for example the
non-diagonal elements of the covariance matrix are given by

$$
\begin{align}
\text{Cov}(\varepsilon_i, \varepsilon_j) &=
    \beta_i \beta_j \mathop{\text{Var}}(\widetilde{\varepsilon}_M)
    - \omega_i \beta_j \mathop{\text{Var}}(\widetilde{\varepsilon}_i)
    - \omega_j \beta_i \mathop{\text{Var}}(\widetilde{\varepsilon}_j)
\end{align}
$$

which is indeed different from zero: assets with a large beta and a large weight would typically
have their proxy residuals be more correlated to other proxy residuals. And when it comes to
reformulating the risk decomposition of asset returns, we obtain

$$
\text{Var}(R_i) = \beta_i^2 \left[
        \mathop{\text{Var}}(R_M) - 2 \mathop{\text{Var}}(\widetilde{\varepsilon}_M)
    \right]
    + \left[
        \mathop{\text{Var}}(\varepsilon_i)
        + 2 \omega_i \beta_i \mathop{\text{Var}}(\widetilde{\varepsilon}_i)
    \right]
$$

which is a decomposition similar to $$\eqref{eq:decomposition}$$, but with a decreased contribution
of the systematic component and an increased contribution of the unsystematic one. Other elementary
properties can be derived in the same way, and can be used to assess whether some approximations
are likely to hold empirically or not.

### Digression on Model Theory from formal logic

If you have read until here, you might be wondering why we even bothered with this theoretical
issue if the consequences of the Single Index Model still hold approximately in an empirical
setting. The most important aspect lies in the precise definition of the word "consequences", as I
will illustrate using analogies to simplified concepts drawn from formal logic.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
In formal logic, the concept of **theory** describes a set of elementary assumptions (called
**axioms**) about unspecified objects (called **variables**), and the derivation of new
consequences (also called **theorems**) about said objects using only the axioms. A key point is
that when deriving or proving theorems, the true nature of the variables never needs to be
discussed, the only thing we know is that they verify the axioms of the theory. In our analogy, a
theory describing the Single Index Model would have for variables the asset returns
$$R_1, \cdots, R_n$$ and the residuals $$\varepsilon_1, \cdots, \varepsilon_n$$, while the axioms
would be the respective relations between $$R_i$$ and $$\varepsilon_i$$ given by
$$\eqref{eq:model}$$ as well as the three conditions in $$\eqref{eq:hypothesis}$$. An example of
theorem within this theory would be that the idiosyncratic variance of a portfolio is equal to
the weighted sum of the idiosyncratic variances of its constituents. We did not discuss the role
of the $$(\alpha_i, \beta_i)_{1 \leq i \leq n}$$ and $$(\omega_i)_{1 \leq i \leq n}$$, but we can
simply consider them as free parameters so that we are really facing an entire *collection* of
theories, each one being defined by specific values of the parameters.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
An **inconsistent** or **contradictory** theory is a special, degenerate case of theory in which it
is possible to prove both one thing and its contrary from the axioms. For example if you make a
theory of stock prices where prices are always positive along with a bunch of other assumptions,
and later build an example within the theory of a stock whose price can become negative, then your
theory is inconsistent. It can easily be shown that an inconsistent theory also proves *every
single sentence*: not only can you prove one thing and its contrary, but you can in fact literally
prove anything you want. Our theory of the Single Index Model as laid out in the first section was
not inconsistent in the strict sense, we just proved that it was equivalent to a much simpler
theory where $$\varepsilon_i$$ is always zero. However if we start assuming that
$$\varepsilon_i \neq 0$$, then our theory becomes *truly* inconsistent, because it proves both
$$\varepsilon_i = 0$$ and $$\varepsilon_i \neq 0$$ (the latter is now another axiom). And in
general, it is very easy to *implicitly* add simple and reasonable assumptions like
$$\varepsilon_i \neq 0$$ in a reasoning: for example, just observe that I implicitly assumed
$$\text{Var}(R_M) \neq 0$$ throughouht a majority of the previous sections without ever stating it
explicitly. The conclusion is that, because the theory is inconsistent, we cannot trust any of its
consequences, and the reason some of them are still reasonable is because they stem from *another*
theory which happens to be consistent: for example a theory describing Fama's revised model, but we
cannot know that until we uncover said theory.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
But how can we be sure that Fama's theory is consistent and does not lead to contradictions like
the Diagonal Model? Within formal logic, we would try to find an **intepretation** of the theory in
which the axioms hold. An interpretation means that we produce concrete objects in place of the
abstract variables, and are now allowed to rely on specific properties about these objects (*i.e.*
their "true nature") to verify the axioms. Such an interpretation is then called a **model** of the
theory, and a theory having a model is necessarily consistent (this is one direction of what is
known as *Gödel's completeness theorem*, the reciprocal direction being that every consistent
theory has a model). In order to show that Fama's theory is consistent, we are going to produce
actual random variables that verify all of its axioms: let
$$(\widetilde{\varepsilon}_1, \cdots, \widetilde{\varepsilon}_n, \widetilde{R})$$ follow a
$$(n+1)$$-variate normal distribution with mean vector $$(0, \cdots, 0, \widetilde{r})$$ and
diagonal covariance matrix

$$
\begin{pmatrix}
\sigma_1^2 &  & & \\
 & \ddots & & \\
 &  & \sigma_n^2 & \\
 &  &  & \sigma_{n+1}^2
\end{pmatrix}
$$

then define $$R_i \triangleq \alpha_i + \beta_i \widetilde{R} + \widetilde{\varepsilon}_i$$ for
$$i \in \{1, \cdots, n\}$$. It is easily verified that the $$(\widetilde{\varepsilon}_i$$) and
$$\widetilde{R}$$ satisfy the three conditions of $$\eqref{eq:hypothesis_fama}$$, while each
$$R_i$$ obviously satifies $$\eqref{eq:fama}$$. In what precedes, $$r \in \mathbb{R}$$ and
$$\sigma_1, \cdots, \sigma_{n+1} > 0$$ are arbitrary parameters, so that we really produced a
collection of interpretations in which Fama's axioms are true. A theory being consistent does not
mean that is is universally true, *i.e.* there may be interpretations where the axioms do not hold:
it would be very easy to produce a mathematical interpretation of Fama's theory in which the axioms
are false, but we also have empirical evidence that the real world is not really a model of Fama's
theory.

[^1]: William F. Sharpe. *A Simplified Model for Portfolio Analysis*. Management Science. Vol. 9, No. 2 (Jan 1963), pp. 277-293.
[^2]: Chapter 4, "An Introduction to Multi-Factor Models", p. 39.
[^3]: William F. Sharpe. *Capital Asset Prices: A Theory Of Market Equilibrium Under Conditions Of Risk*. Journal of Finance. Vol. 19, No. 3 (Sep 1964), pp. 425-442.
[^4]: William F. Sharpe. *Portfolio Analysis Based On A Simplified Model Of The Relationships Among Securities*. University of California. PhD Dissertation (Jun 1961).
[^5]: Eugene F. Fama. *Risk, Return and Equilibrium: Some Clarifying Comments*. Journal of Finance. Vol. 23, No. 1 (Mar 1968), pp. 29-40.
[^6]: See the last paragraph in p. 38 of the citation just above, "(16c) is inconsistent with the remaining assumptions of the market model".
[capm_beta_proof]: http://www.columbia.edu/~ks20/FE-Notes/4700-07-Notes-CAPM.pdf
[fermat]: https://en.wikipedia.org/wiki/Fermat's_Last_Theorem
[wikipedia]: https://en.wikipedia.org/wiki/Single-index_model
