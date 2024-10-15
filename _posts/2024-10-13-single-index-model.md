---
layout: post
title: Inconsistency of the Single Index Model
---

<span class="dropcap">R</span>eading through *Advanced Portfolio Management* by Giuseppe A.
Paleologo led me to give some thoughts to the so-called **Single Index Model** or **Diagonal
Model**, originally introduced by Sharpe[^1] in 1963, and the way it is typically exposed in
introductory texts or in online writings. I am not going to discuss the empirical validity of the
model too much, as there has been a ton of literature on the subject and we now have more general
factor models for a reason. Instead, I will focus on an oddity that made me question the actual
soundness of the model.

### A probabilistic model

The assumptions of the model are as follows: there are $$n$$ risky assets with random returns
$$R_1, \dots, R_n$$. In addition, the return of the **market portfolio** is denoted by $$R_M$$. The
real-life portfolio that corresponds best to the market portfolio does not have to be specifically
defined at this point, but it is at least assumed that it contains each asset $$i$$ (or just a
strict subset of the assets) in proportion $$\omega_i > 0$$ with $$\sum \omega_i = 1$$. Finally,
the dynamics of the random returns are given by:

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
$$\eqref{eq:hypothesis}$$ make the model equivalent to $$n$$ different linear regressions where
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
than $$1$$ because of the relation $$\sum \psi_j = 1$$. This is also the approach used in
*Advanced Portfolio Management* to justify that the idiosyncratic volatility of the market
portfolio itself is comparatively small[^2]. However, another argument could be made for the market
portfolio: since we are regressing asset returns onto the market portfolio return, the beta of
the market portfolio is one and its residual is zero *by definition*, therefore its idiosyncratic
volatility is trivially zero. I started to speculate about the presence of a circular argument
somewhere, and indeed we can observe more rigorously that

$$
\begin{align}
R_M &= \sum\limits_{i=1}^n \omega_i R_i \nonumber \\
    &= \sum\limits_{i=1}^n \omega_i (\alpha_i + \beta_i R_M + \varepsilon_i) \nonumber \\
    &= \sum\limits_{i=1}^n \omega_i \alpha_i
        + \left( \sum\limits_{i=1}^n \omega_i \beta_i \right) R_M
        + \sum\limits_{i=1}^n \omega_i \varepsilon_i .
\label{eq:circular}
\end{align}
$$

For a fixed $$j \in \{1, \cdots, n\}$$, applying the second condition of
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

where we used the fact that the covariance with a constant is zero, the second condition of
$$\eqref{eq:hypothesis}$$ once more, and the uncorrelation of the $$(\varepsilon_i)$$. Hence, we
obtain $$0 = \mathop{\text{Var}}(\varepsilon_j) = \mathop{\mathbf{E}}(\varepsilon_j^2)$$ because of
the first condition in $$\eqref{eq:hypothesis}$$, which is only possible if $$\varepsilon_j = 0$$
(almost surely), and this result holds for any $$j \in \{1, \cdots, n\}$$.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
By exploiting the assumptions of the model, we reduced it to a trivial case where every asset
return is an affine combination of the others. Surely this is *not* what Sharpe had in mind, and
yet after re-reading his original papers, the inconsistency was there from the beginning. Does that
mean that the model is irreparably flawed and that we can't just add up idiosyncratic volatilities
to compute the volatility of a portfolio, or hedge out the market risk common to all assets? The
empirical applications tend to say that yes, we can do these things at least as an approximation,
so the situation may be salvageable. The residuals cannot be uncorrelated, but the empirical
cross-correlations look to be small enough for the "natural" consequences of the model to hold
approximately, such as additivity of the idiosyncratic volatilities. Can we just replace the
uncorrelation hypothesis in $$\eqref{eq:hypothesis}$$ by "cross-correlations must be small"? Well,
this is not an appropriate way of describing a mathematical model, and it also prevents saying
anything quantitative about the cases where "small" is not actually small enough.

### The CAPM and the origins of the model

As I said, I started by revisiting the old papers written by Sharpe, from which I got a better
understanding of how his Diagonal Model came to life. Oddly enough, it is often amalgamated with
the **CAPM** published as an independent paper one year later[^3]: Sharpe initially wrote a first
version of the CAPM in terms of the Diagonal Model in his 1961 PhD dissertation[^4], but it was
rewritten completely in the 1964 paper such that the two models are now in fact independent and use
different sets of assumptions.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Fundamentally, the CAPM is just a solution to the Mean Variance Optimization problem introduced by
Markowitz, when the market is in a specific equilibrium. MVO really only relates to *expectations*
of returns rather than their dynamics, and as such the original CAPM formulation is also given in
terms of expectations:

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
*extrapolation* of the CAPM in which $$R_M$$ doesn't have to be the return of the exact portfolio
from the CAPM, but rather any portfolio or **factor** that is believed to drive returns in a given
universe of assets: Sharpe does leave this door open in his 1963 paper (even though he only
proceeds to apply it with actual market portfolios), and this interpretation is what is going to
help us fix the theoretical soundness issue described earlier. Still, the model remains in a
sense *compatible* with the CAPM: this time as a consequence of the uncorrelatedness of
$$\varepsilon_i$$ and $$R_M$$, we obtain again

$$
    \beta_i = \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)}
$$

so that if the market portfolio is chosen to be the portfolio from the CAPM, both models assign the
same value to $$\beta_i$$. It is clear to me that this feature of the Diagonal Model may have
appealed to Sharpe.

### Fama to the rescue

After stumbling onto the issue exposed in the first section, I was initially left confused. It was
not the first time I came across the Single Index Model, but I had never spent much time thinking
about it deeply. My first reflex has been to search for answers on the internet, without a lot of
success. For example at the time of writing, [Wikipedia][wikipedia] defines the model more or less
exactly as I do in my first section. The Wikipedia page does acknowledge that *empirically*, the
residual cross-correlations are not exactly zero, but apparently without realizing that using
$$\text{Cov}(\varepsilon_i, \varepsilon_j) = 0$$ as an approximation of reality makes the model so
constrained as to render it trivial. I even asked ChatGPT, without luck of course (every time I ask
something to ChatGPT, I secretly hope that it gives me an incorrect answer, it makes me feel
better). I started to wonder whether people were aware of the issue and simply did not deem it
worthy of discussion.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
It turned out to be another explanation: writings about finance on the internet are just of low
quality in general. And after spending a few hours searching through old papers, I finally found
one written by Fama in 1968[^5] that was discussing precisely the issue that I describe. Fama also
notices that the three conditions in $$\eqref{eq:hypothesis}$$ cannot hold all at once. In fact, he
still makes a small mistake by stating that even the second condition cannot hold by itself[^6] (we
will revisit this case in the next and final section) but otherwise proposes a revised model that
stays in the spirit of Sharpe's original one: instead of regressing the $$R_i$$ directly onto
$$R_M$$, Fama postulates the existence of an unobservable random factor $$\widetilde{R}$$ and a new
family of residuals $$(\widetilde{\varepsilon}_i)$$ such that

$$
\begin{equation}
R_i = \alpha_i + \beta_i \widetilde{R} + \widetilde{\varepsilon}_i, \quad 1 \leq i \leq n
\label{eq:fama}
\end{equation}
$$

where the new residuals satisfy three conditions similar to the original ones:

$$
\begin{align}
&\mathbf{E}(\widetilde{\varepsilon}_i) = 0 \nonumber \\
&\text{Cov}(\widetilde{\varepsilon}_i, \widetilde{R}) = 0 \label{eq:hypothesis_fama} \\
&\text{Cov}(\widetilde{\varepsilon}_i, \widetilde{\varepsilon}_j) = 0, \quad i \neq j . \nonumber
\end{align}
$$

How is it different from the original model? The key point is that the regressor $$\widetilde{R}$$
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
$$\beta_M = 1$$ without loss of generality.  We can now translate $$\eqref{eq:fama}$$ into a
regression similar to the one in Sharpe's model:

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
With the formulation from $$\eqref{eq:new_regression}$$, we can now in theory estimate the
$$\beta_i$$ exactly as in the original model by regressing the observations of $$R_i$$ onto
$$R_M$$. However, this regression is not *well-posed* in the sense that $$\varepsilon_i$$ and
$$R_M$$ are no longer necessarily uncorrelated. Hence, the OLS estimator of $$\beta_i$$ is no
longer consistent and instead converges to

$$
    \frac{\beta_i}{1 +
        \frac{
            \mathop{\text{Var}(\widetilde{\varepsilon}_M)}
        }{
            \mathop{\text{Var}(\widetilde{R})}
        } 
    }
$$

but using the same argument as developed in the first section in $$\eqref{eq:idio_ptf}$$, we can
expect the idiosyncratic variance of $$R_M$$ to be small, and hopefully small enough in comparison
to $$\mathop{\text{Var}(\widetilde{R})}$$ so that our estimator converges to a value close to the
real $$\beta_i$$. We can also give a few other properties of the "proxy" residuals
$$(\varepsilon_i)$$, for example the non-diagonal elements of the covariance matrix are given by

$$
\begin{align}
\text{Cov}(\varepsilon_i, \varepsilon_j) &=
    \beta_i \beta_j \mathop{\text{Var}}(\widetilde{\varepsilon}_M)
    - \omega_i \beta_j \mathop{\text{Var}}(\widetilde{\varepsilon}_i)
    - \omega_j \beta_i \mathop{\text{Var}}(\widetilde{\varepsilon}_j)
\end{align}
$$

which is indeed different from zero: assets with a large beta and a large weight in the market
portfolio would typically have their proxy residuals be more correlated to other proxy residuals.
And when it comes to reformulating the risk decomposition of asset returns, we obtain

$$
\text{Var}(R_i) = \beta_i^2 \left[
        \mathop{\text{Var}}(R_M) - 2 \mathop{\text{Var}}(\widetilde{\varepsilon}_M)
    \right]
    + \left[
        \mathop{\text{Var}}(\varepsilon_i)
        + 2 \omega_i \beta_i \mathop{\text{Var}}(\widetilde{\varepsilon}_i)
    \right]
$$

which is a decomposition similar to the one from $$\eqref{eq:decomposition}$$ but with a decreased
contribution of the systematic component and an increased contribution of the unsystematic one.
Other elementary properties can be derived in the same way, and can be used to assess whether some
approximations are likely to hold empirically or not.

### Digression about Model Theory from formal logic

Abcd.

[^1]: William F. Sharpe. *A Simplified Model for Portfolio Analysis*. Management Science. Vol. 9, No. 2 (Jan 1963), pp. 277-293.
[^2]: Chapter 4, "An Introduction to Multi-Factor Models", p. 39.
[^3]: William F. Sharpe. *Capital Asset Prices: A Theory Of Market Equilibrium Under Conditions Of Risk*. Journal of Finance. Vol. 19, No. 3 (Sep 1964), pp. 425-442.
[^4]: William F. Sharpe. *Portfolio Analysis Based On A Simplified Model Of The Relationships Among Securities*. University of California. PhD Dissertation (Jun 1961).
[^5]: Eugene F. Fama. *Risk, Return and Equilibrium: Some Clarifying Comments*. Journal of Finance. Vol. 23, No. 1 (Mar 1968), pp. 29-40.
[^6]: See the last paragraph in p. 38 of the citation just above, "(16c) is inconsistent with the remaining assumptions of the market model".
[capm_beta_proof]: http://www.columbia.edu/~ks20/FE-Notes/4700-07-Notes-CAPM.pdf
[fermat]: https://en.wikipedia.org/wiki/Fermat's_Last_Theorem
[wikipedia]: https://en.wikipedia.org/wiki/Single-index_model
