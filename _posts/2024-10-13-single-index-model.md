---
layout: post
title: "[WIP] Unobservable factors: from CAPM to APT"
---

<span class="dropcap">R</span>ecent readings about the **Single Index Model**, originally
introduced by Sharpe[^1] in 1963, led me to give some thoughts to the the way it is typically
exposed in introductory texts. I am not going to discuss the empirical validity of the model, as
there has been a large amount of literature on the subject and we now have multi-factor models for
a reason. Instead, I will focus on a lightly discussed issue that made me question the soundness of
the model, how it can be resolved, and why it matters.

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
than $$1$$ because of the relation $$\sum \psi_j = 1$$. This argument is used in *Advanced
Portfolio Management* to justify that the idiosyncratic volatility of the market portfolio itself
is comparatively small[^2]. However, another argument could be made for the market portfolio: since
we are regressing asset returns onto the market portfolio return, the beta of the market portfolio
is one and its residual is zero *by definition*, therefore its idiosyncratic volatility is
trivially zero. We can observe more rigorously that

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
Surely, a trivial model where every asset return is an affine combination of the others was not
what Sharpe had in mind when he wrote his seminal papers, yet this inconsistency was present from
the beginning. From there, how can we be sure whether anything we derive from the model is not
contradictory? In fact, empirical applications tend to show that most of the familiar consequences,
such as additivity of idiosyncratic variances, still hold approximately: as we will see, the
theoretical issue really lies in the imprudent approximation of "small" residual cross-correlations
with "exactly zero" in $$\eqref{eq:hypothesis}$$.

### The CAPM and the origins of the model

The Single Index Model is often amalgamated with the **CAPM** published as an independent article
one year later[^3]: Sharpe initially wrote a first version of the CAPM in terms of what he also
called the **Diagonal Model** in his 1961 PhD dissertation[^4], but it was rewritten completely in
the 1964 article such that the two models are now in fact independent and use different sets of
assumptions. I will briefly highlight the differences between them.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Fundamentally, the CAPM is a solution to the Mean Variance Optimization problem introduced by
Markowitz, under certain economic assumptions such as asset prices being in equilibrium. MVO really
only relates to *expectations* of returns rather than their dynamics, and as such the CAPM
formulation is also given in terms of expectations:

$$
\begin{equation}
\mathbf{E}(R_i) = \beta_i \mathop{\mathbf{E}}(R_M),
    \quad \beta_i = \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)}
\label{eq:capm}
\end{equation}
$$

where this time $$R_M$$ is the return of a precisely defined portfolio containing all risky assets
in the universe. Note that I have omitted the risk-free rate from $$\eqref{eq:capm}$$ as a
simplification, but feel free to assume that we are talking about excess returns if that triggers
you. Of course it is straightforward to go from expectations to dynamics, just define

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

such that in the CAPM, $$\varepsilon_i$$ is indeed uncorrelated with $$R_M$$. However, this is a
*consequence* of the value of $$\beta_i$$ rather than the other way around: as shown in a footnote
of the 1964 paper (*almost* reminds me of [something else][fermat]), the expression for
$$\beta_i$$ directly stems from the CAPM framework without ever talking about $$\varepsilon_i$$. If
you are more used to Bourbaki-style mathematics like I am, you might find
[this proof][capm_beta_proof] to be more readable. Hence, the CAPM verifies the first two
conditions of $$\eqref{eq:hypothesis}$$, which enables deriving the usual risk decomposition

$$
\begin{equation}
\text{Var}(R_i) = \beta_i^2 \mathop{\text{Var}}(R_M) + \mathop{\text{Var}}(\varepsilon_i)
\label{eq:decomposition}
\end{equation}
$$

with $$\beta_i^2 \mathop{\text{Var}}(R_M)$$ being the **systematic** risk component common to all
returns and $$\text{Var}(\varepsilon_i)$$ the **unsystematic** component.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
From the equation of returns of the CAPM and its consequences in terms of risk decomposition, it
really takes two leaps to arrive to the Diagonal Model:

* abandon the market equilibrium idea and instead focus on an abstract probabilistic
model describing the dynamics of returns  
* embrace the systematic *vs* idiosyncratic decomposition by adding the third condition of
$$\eqref{eq:hypothesis}$$.

The Diagonal Model now becomes a completely independent model that simply attempts to encode the
empirical observation that market risk is a common driver of returns, free of any other economic
assumptions. In fact, $$R_M$$ does not have to be the return of the exact portfolio from the CAPM,
but rather any portfolio or **factor** that is believed to drive returns in a given universe of
assets: Sharpe does leave this door open in his 1963 paper (even though he only proceeds to apply
it with actual portfolios), and this interpretation will help us fix the soundness issue described
earlier. Still, the model remains in a sense *compatible* with the CAPM: this time as a consequence
of the uncorrelatedness of $$\varepsilon_i$$ and $$R_M$$, we obtain again

$$
    \beta_i = \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)}
$$

so that if the market portfolio is chosen to be that of the CAPM, both models assign the same value
to $$\beta_i$$. It is clear to me that this feature may have appealed to Sharpe.

### Fama to the rescue

The internet is pretty sparse when it comes to acknowledging the inconsistency exposed in the
first section. For example at the time of writing, [Wikipedia][wikipedia] defines the model more or
less exactly as I do. The Wikipedia page does acknowledge that *empirically*, the residual
cross-correlations are not exactly zero, but apparently without realizing that using
$$\text{Cov}(\varepsilon_i, \varepsilon_j) = 0$$ as an approximation of reality makes the model so
constrained as to render it trivial. I even asked ChatGPT but needless to say, the result was
disappointing (every time I ask something to ChatGPT, I secretly hope that it gives me an incorrect
answer: it makes me feel a bit less concerned about the imminent AI takeover prophetized by
X/Twitter experts).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Trying my luck at old papers instead, I finally found that Fama first described the issue in a
1968 article[^5], where he also notices that the three conditions in $$\eqref{eq:hypothesis}$$
cannot hold all at once, and proposes a revised model that stays in the spirit of Sharpe's. Jensen
also reviewed Fama's new model in an exhaustive study published one year later[^6], and gave some
justifications as for why the Diagonal Model is a decent approximation. Instead of regressing the
$$R_i$$ directly onto $$R_M$$, Fama postulates the existence of an unobservable random factor
$$\widetilde{R}$$ and a new family of residuals $$(\widetilde{\varepsilon}_i)$$ such that

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
\text{Cov}(\varepsilon_i, \varepsilon_j) =
    \beta_i \beta_j \mathop{\text{Var}}(\widetilde{\varepsilon}_M)
    - \omega_i \beta_j \mathop{\text{Var}}(\widetilde{\varepsilon}_i)
    - \omega_j \beta_i \mathop{\text{Var}}(\widetilde{\varepsilon}_j)
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

which is a decomposition similar to $$\eqref{eq:decomposition}$$, but with a slightly decreased
contribution of the systematic component and an increased contribution of the unsystematic one.
Other elementary properties can be derived in the same way, and can be used to assess whether some
approximations are likely to hold empirically or not.

### On the meaning of consistency

One question that may arise is why even bother with this theoretical issue if the consequences of
the Single Index Model still hold approximately in an empirical setting. The most important aspect
lies in the precise definition of the word "consequences", especially in the context of a
contradictory model, as I will illustrate with simplified concepts drawn from formal logic.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
In mathematical logic, the concept of **inconsistent** or **contradictory** theory is used to
describe a set of assumptions or **axioms** from which we can derive both one thing and its
contrary. It can easily be shown that an inconsistent theory also proves *every single sentence*:
not only can you prove one thing and its contrary, but you can in fact literally prove anything you
want. Our model as laid out in the first section was not inconsistent in the strict sense, we just
proved that it is equivalent to a much simpler one where $$\varepsilon_i$$ is always zero. However
if we start assuming that $$\varepsilon_i \neq 0$$, then it becomes *truly* inconsistent, because
it proves both $$\varepsilon_i = 0$$ and $$\varepsilon_i \neq 0$$. And in general, it is very easy
to *implicitly* add simple assumptions like $$\varepsilon_i \neq 0$$ in a reasoning: for example,
just observe that I assumed $$\text{Var}(R_M) \neq 0$$ throughouht a majority of the previous
sections without ever stating it. This ties back to the question asked as a conclusion of the
first section: we can't trust any consequence of the model when it is inconsistent. The reason why
some of the consequences are still reasonable is because they stem from *another* model which
happens to be consistent: for example Fama's revised model, but it is impossible to know that until
we uncover said model.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Is there a way to make sure that Fama's model is not contradictory either though? One of the areas
of mathematical logic contains a theorem stating that it is sufficient (and necessary) to produce a 
concrete example that verifies all the axioms in order to have consistency: this is known as
*Gödel's completeness theorem*. We can apply this theorem in the context of Fama's model by letting
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

then defining $$R_i \triangleq \alpha_i + \beta_i \widetilde{R} + \widetilde{\varepsilon}_i$$ for
$$i \in \{1, \cdots, n\}$$. It is easily verified that the $$(\widetilde{\varepsilon}_i$$) and
$$\widetilde{R}$$ satisfy the three conditions of $$\eqref{eq:hypothesis_fama}$$, while each
$$R_i$$ obviously satifies $$\eqref{eq:fama}$$. In what precedes, $$r \in \mathbb{R}$$ and
$$\sigma_1, \cdots, \sigma_{n+1} > 0$$ are arbitrary parameters, so that we really produced a
collection of examples for which Fama's axioms are true. A theory being consistent does not
mean that is is universally true though, *i.e.* there may still be interpretations where the axioms
do not hold: it would be very easy to produce an example of random returns for which Fama's axioms
are false, and we also have empirical evidence that the axioms do not really hold in the real world
either, providing us with a very concrete example.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
What about the CAPM, is it consistent? Making abstraction of the economic arguments that lead to
its formulation, we need to produce an example of random returns that verify $$\eqref{eq:capm}$$,
and we also saw that $$\beta_i = \mathop{\text{Cov}}(R_i, R_M) / \mathop{\text{Var}}(R_M)$$ is
equivalent to $$\text{Cov}(\varepsilon_i, R_M) = 0$$. Fama believed this last condition to be
contradictory just by itself[^7], because $$\varepsilon_i$$ appears in the expression of $$R_i$$
which is itself a term in the expression of $$R_M$$, hence the two could not be uncorrelated.
However, he made a mistake in that what really appears in $$R_M$$ is
$$\sum \omega_i \varepsilon_i$$ and this sum may be equal to zero. If we start from random returns
that satisfy Fama's axioms, such as the example we produced in the previous paragraph, then it is
indeed the case since

$$
\begin{align}
\sum\limits_{i=1}^n \omega_i \varepsilon_i &= \sum\limits_{i=1}^n \omega_i \left(
    \widetilde{\varepsilon}_i - \beta_i \widetilde{\varepsilon}_M \right
) \nonumber \\
&= \sum\limits_{i=1}^n \omega_i \widetilde{\varepsilon}_i
    - \left( \sum\limits_{i=1}^n \omega_i \beta_i \right) \widetilde{\varepsilon}_M \nonumber \\
&= \widetilde{\varepsilon}_M - \widetilde{\varepsilon}_M \nonumber \\
&= 0. \nonumber
\end{align}
$$

It can be easily verified that $$\eqref{eq:capm}$$ holds if we set $$\alpha_i = 0$$ and

$$
\beta_i = \frac{
    \omega_i \mathop{\text{Var}}(\widetilde{\varepsilon}_i)
}{
    \sum\limits_{j=1}^n \omega_j^2 \mathop{\text{Var}}(\widetilde{\varepsilon}_j)
}.
$$

So while Fama's model is consistent for arbitrary values of $$\alpha_i$$ and $$\beta_i$$, the CAPM
needs to restrict the weights of the market portfolio in order to be consistent for arbitrary
values of $$\beta_i$$, which I suppose ties back into the economic assumptions that impose a
certain form to the market portfolio, although I did not take the time to run the numbers. Still,
this shows that only the third condition of $$\eqref{eq:hypothesis}$$ crosses the frontier between
consistency and inconsistency. Models written in terms of unobservable factors, beyond their
increased empirical power, are a necessity for theoretical soundness.

#### References

[^1]: William F. Sharpe. *A Simplified Model for Portfolio Analysis*. Management Science, Vol. 9, No. 2 (Jan 1963), pp. 277-293.
[^2]: Giuseppe A. Paleologo. *Advanced Portfolio Management*. Wiley (Aug 2021), Chapter 4, p. 39.
[^3]: William F. Sharpe. *Capital Asset Prices: A Theory Of Market Equilibrium Under Conditions Of Risk*. Journal of Finance, Vol. 19, No. 3 (Sep 1964), pp. 425-442.
[^4]: William F. Sharpe. *Portfolio Analysis Based On A Simplified Model Of The Relationships Among Securities*. Unpublished PhD Dissertation, University of California (Jun 1961).
[^5]: Eugene F. Fama. *Risk, Return and Equilibrium: Some Clarifying Comments*. Journal of Finance, Vol. 23, No. 1 (Mar 1968), pp. 29-40.
[^6]: Michael C. Jensen. *Risk, The Pricing of Capital Assets, and The Evaluation of Investment Portfolios*. Journal of Business, Vol. 42, No. 2 (Apr 1969), pp. 167-247.
[^7]: See the last paragraph in p. 38 of [5], "(16c) is inconsistent with the remaining assumptions of the market model".
[capm_beta_proof]: http://www.columbia.edu/~ks20/FE-Notes/4700-07-Notes-CAPM.pdf
[fermat]: https://en.wikipedia.org/wiki/Fermat's_Last_Theorem
[wikipedia]: https://en.wikipedia.org/wiki/Single-index_model
