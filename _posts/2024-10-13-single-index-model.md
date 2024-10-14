---
layout: post
title: Inconsistency of the Single Index Model
---

<span class="dropcap">R</span>eading through *Advanced Portfolio Management* by Giuseppe A.
Paleologo led me to give some thoughts to the so-called **Single Index Model** or **Diagonal
Model**, originally introduced by Sharpe[^1] in 1963, and the way it is typically exposed in
introductory texts or in online writings. I am not going to discuss the empirical validity of the
model too much, as there has been a ton of literature on the subject and it is not necessarily
useful to write more (and we now have more general factor models for a reason). Instead, I will
focus on an oddity that made me question the actual soundness of the model, at least in the first
place.

### A probabilistic model

The assumptions of the model are as follows: there are $$n$$ risky assets with random returns
$$R_1, \dots, R_n$$. In addition, the return of the **market portfolio** is denoted by $$R_M$$. The
real-life portfolio that corresponds best to the market portfolio does not have to be specifically
defined at this point, but it is at least assumed that it contains each asset $$i$$ (or even a
strict subset of the assets if you want it to be more general) in proportion $$\omega_i \geq 0$$
with $$\sum \omega_i = 1$$. Finally, the dynamics of the random returns are given by:

$$
\begin{equation}
R_i = \alpha_i + \beta_i R_M + \varepsilon_i, \quad 1 \leq i \leq n
\label{eq:model}
\end{equation}
$$

for some real numbers $$(\alpha_i)_{1 \leq i \leq n},\, (\beta_i)_{1 \leq i \leq n}$$, and
$$(\varepsilon_i)_{1 \leq i \leq n}$$ being a family of random **residuals** verifying

$$
\begin{align}
&\mathbf{E}({\varepsilon_i}) = 0 \nonumber \\
&\text{Cov}(\varepsilon_i, R_M) = 0 \label{eq:hypothesis} \\
&\text{Cov}(\varepsilon_i, \varepsilon_j) = 0, \quad i \neq j . \nonumber
\end{align}
$$

The residual standard deviation $$\sigma(\varepsilon_i) = \sqrt{\text{Var}(\varepsilon_i)}$$ is
also called the **idiosyncratic** volatility of asset $$i$$. By linearity, the notations naturally
extend to any given portfolio made of a subset of the $$n$$ assets, so that we can freely talk
about a portfolio's own **beta**, residual and idiosyncratic volatility.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
If we interpret $$\eqref{eq:model}$$ as a time-series model, then we can also approach it as a
standard linear regression. And indeed in *Advanced Portfolio Management*, each $$\beta_i$$ is
defined as the regression coefficient of the daily returns of a stock $$i$$ onto
$$R_M = R_\text{SPY}$$. A bit later in the book, the author wants to justify that the idiosyncratic
volatility of the market portfolio itself is comparatively small[^2]. They start by arguing that
the beta of the market portfolio is one *by definition*, because it is the result of a regression
with respect to the market portfolio return itself. Then for its idiosyncratic volatility, they
give the following argument: since the $$(\varepsilon_i)$$ are uncorrelated, the idiosyncratic
variance of the market portfolio is given by

$$
\begin{align}
\text{Var} \left( \sum\limits_{i=1}^n \omega_i \varepsilon_i \right)
    &= \sum\limits_{i=1}^n \omega_i^2 \mathop{\text{Var}}(\varepsilon_i) \nonumber \\
&\leq
\left( \sum\limits_{i=1}^n \omega_i^2 \right)
    \mathop{\text{max}}\limits_{1 \leq i \leq n} \mathop{\text{Var}}(\varepsilon_i) \nonumber
\end{align}
$$

and conclude by observing that $$\sum \omega_i^2$$ is much smaller than one because of the relation
$$\sum \omega_i = 1$$. Reading this, I started to sense the presence of a hidden circular argument
somewhere: if we say that the market portfolio beta is one by definition, then we could just as
well say that its residual is also *exactly zero* by definition, hence its idiosyncratic volatility
is trivially zero. And indeed, we can try to pursue this argument more rigorously by observing that

$$
\begin{align}
R_M &= \sum\limits_{i=1}^n \omega_i R_i \nonumber \\
    &= \sum\limits_{i=1}^n \omega_i (\beta_i R_M + \varepsilon_i) \nonumber \\
    &= \left( \sum\limits_{i=1}^n \omega_i \beta_i \right) R_M
        + \sum\limits_{i=1}^n \omega_i \varepsilon_i .
\label{eq:circular}
\end{align}
$$

Taking expectations in $$\eqref{eq:circular}$$, we get

$$
\mathbf{E}(R_M) = \left( \sum\limits_{i=1}^n \omega_i \beta_i \right) \mathop{\mathbf{E}}(R_M)
$$

which implies $$\sum \omega_i\beta_i = 1$$. Plugging back in $$\eqref{eq:circular}$$, we
eventually get $$\sum \omega_i \varepsilon_i=0$$, which implies that
$$\sum \omega_i^2 \mathop{\text{Var}}(\varepsilon_i)=0$$ because of uncorrelation, which is only
possible if $$\varepsilon_i = 0$$ (almost surely) for all $$i$$.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
By exploiting the assumptions of the model, we reduced it to a trivial case where every asset
return is a linear combination of the others. Surely this is *not* what Sharpe had in mind, and
yet after re-reading his original papers, the inconsistency was indeed there from the beginning.
Does that mean that the model is irreparably flawed and that we can't just add up idiosyncratic
volatilities to compute the volatility of a portfolio, or hedge out the market risk common to all
assets? The empirical applications of the model tend to say that yes, we can actually do these
things, at least as an approximation, so the situation may be salvageable. The residuals cannot be
uncorrelated, but the empirical cross-correlations look to be small enough that some of the
"natural" consequences of the model, such as additivity of the idiosyncratic volatilities, seem to
hold approximately. Can we just replace the uncorrelation hypothesis in $$\eqref{eq:hypothesis}$$
by "cross-correlations must be small"? Well, this is not an appropriate way of describing a
mathematical model, and it also prevents saying anything quantitative about the cases where "small"
is not actually small enough.

### The CAPM and the origins of the model

As I said, I started by revisiting the old papers written by Sharpe, from which I got a better
understanding of how his Diagonal Model came to life. Oddly enough, it is often amalgamated with
the **CAPM** published as an independent paper one year later[^3]. Yes, they *are* related and
Sharpe initially wrote a first version of the CAPM in terms of the Diagonal Model in his 1961 PhD
dissertation[^4]. However, it was rewritten completely in the 1964 paper such that the two models
are now in fact independent and use different sets of assumptions.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Fundamentally, the CAPM is just a solution to the Mean Variance Optimization problem introduced by
Markowitz, when the market is in a specific equilibrium. MVO really only relates to *expectations*
of returns rather than their dynamics, and as such the original CAPM formulation is also given in
terms of expectations:

$$
\begin{equation}
\mathbb{E}(R_i) = \beta_i \mathop{\mathbb{E}}(R_M),
    \quad \beta_i = \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)} .
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

so that in the CAPM, the $$\varepsilon_i$$ are indeed uncorrelated with $$R_M$$. However, this is
purely a *consequence* of the value of $$\beta_i$$ rather than the other way around: if you read
Sharpe's paper very thoroughly, you will see that the value of $$\beta_i$$ directly stems from the
CAPM framework without ever talking about $$\varepsilon_i$$. If you are more used to Bourbaki-style
mathematics like I am, you might find [this proof][capm_beta_proof] to be easier to read (the proof
in Sharpe's paper is literally written in a footnote... *almost* reminds me of
[something else][fermat]). From there, we can derive the usual risk decomposition

$$
\text{Var}(R_i) = \beta_i^2 \mathop{\text{Var}}(R_M) + \mathop{\text{Var}}(\varepsilon_i)
$$

with $$\beta_i^2 \mathop{\text{Var}}(R_M)$$ being the **systematic** risk component common to all
returns and $$\text{Var}(\varepsilon_i)$$ the **unsystematic** component. At this point, I insist
on the term "unsystematic" rather than "idiosyncratic" because nothing prevents the
$$(\varepsilon_i)$$ to be correlated *a priori*, even though they satisfy the first two hypotheses
of $$\eqref{eq:hypothesis}$$.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
From the equation of returns of the CAPM and its consequences in terms of risk decomposition, it
really only takes two natural leaps to go from the CAPM to Sharpe's Diagonal Model:  
* abandon the market equilibrium idea and instead focus on a general probabilistic model describing
  the dynamics of returns
* embrace the systematic *vs* idiosyncratic decomposition by adding the uncorrelation hypothesis
  of $$\eqref{eq:hypothesis}$$.

In that way, the Diagonal Model becomes a completely independent model that simply attempts to
encode the empirical observation that market risk is the common driver of returns, free of any
economic assumptions such as investors acting rationally or asset prices being in equilibrium. More
than a *generalization* of the CAPM, it is rather an *extrapolation* in which $$R_M$$ doesn't have
to be the return of the exact portfolio from the CAPM, but rather any portfolio or **factor** that
is believed to drive returns in a given universe of assets: Sharpe does leave this door open in his
1963 paper (even though he only proceeds to apply it with actual market portfolios), and this
interpretation is what is going to help us fix the theoretical soundness issue described earlier.
Still, the model remains in a sense *compatible* with the CAPM: this time as a consequence of the
uncorrelatedness of $$\varepsilon_i$$ and $$R_M$$, we obtain again

$$
    \beta_i = \frac{\text{Cov}(R_i, R_M)}{\text{Var}(R_M)}
$$

so that if the market portfolio is chosen to be the portfolio from the CAPM, both models assign the
same value to $$\beta_i$$. It is clear to me that this feature of the Diagonal Model may have
appealed to Sharpe.

### Fama to the rescue

After stumbling onto the issue exposed in the first section, I was initially left confused. It was
not the first time I came across the Single Index Model, as I had learnt about it some years ago
already, but I had never spent much time thinking about it deeply. My first reflex was to search
for answers on the internet, without a lot of success. For example at the time of writing,
[Wikipedia][wikipedia] defines the model more or less exactly as I do in my first section. The
Wikipedia page does acknowledge that *empirically*, the residual cross-correlations are not exactly
zero, but apparently without realizing that using $$\text{Cov}(\varepsilon_i, \varepsilon_j) = 0$$
as an approximation of reality makes the model so constrained as to render it trivial. I even asked
ChatGPT, without luck of course (every time I ask something to ChatGPT, I secretly hope that it
gives me an incorrect answer so that I can tell "See? It's *still* not intelligent in any way!").
I started to wonder whether I had fundamentally misunderstood something, or whether people were
aware of the issue but did not deem it worthy of discussion (in my opinion this *is* an important
issue, but I will elaborate in the next section).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Turns out that it was yet another explanation: writings about finance on the internet are just of
low quality in general. And after spending a few hours searching through old papers, I finally
found one written by Fama in 1968[^5] that was discussing precisely the issue that I describe. Fama
also notices that the three conditions in $$\eqref{eq:hypothesis}$$ cannot hold all at once, and
proposes a revised model that stays in the spirit of Sharpe's original one. Instead of regressing
the $$R_i$$ directly onto $$R_M$$, Fama postulates the existence of an unobservable random factor
$$\widetilde{R}$$ and a new family of residuals $$(\widetilde{\varepsilon}_i)$$ such that

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

[^1]: *A Simplified Model for Portfolio Analysis*, William F. Sharpe, 1963.
[^2]: Chapter 4, "An Introduction to Multi-Factor Models", p. 39.
[^3]: *Capital Asset Prices: A Theory Of Market Equilibrium Under Conditions Of Risk*, William F. Sharpe, 1964.
[^4]: *Portfolio Analysis Based On A Simplified Model Of The Relationships Among Securities*, William F. Sharpe, 1961.
[^5]: *Risk, Return and Equilibrium: Some Clarifying Comments*, Eugene F. Fama, 1968.
[capm_beta_proof]: http://www.columbia.edu/~ks20/FE-Notes/4700-07-Notes-CAPM.pdf
[fermat]: https://en.wikipedia.org/wiki/Fermat's_Last_Theorem
[wikipedia]: https://en.wikipedia.org/wiki/Single-index_model
