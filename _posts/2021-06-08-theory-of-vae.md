---
layout: post
title: Theory of variational inference
date: 2021-06-08
description: Working out the math of variational inference
usemathjax: true
giscus_comments: true
toc:
  sidebar: left
tags:
  - machine_learning
  - deep_leaning
  - derivarions
---



## Theory of variational inference

### Premise

Given a set of $$N$$ observed variables $$X$$ , the bayesian framework of model fitting aims to
find out the most likely parameters $$\theta$$ that maximizes the likelihood of observing $$X$$ .
To put it in probablistic terms we try to maximize the function $$P(\theta|X)$$.

$$
\mathop{argmax}_\theta \Big\{P(\theta|X)\Big\}
$$

This is known as MAP estimate.

Another formulation of the problem is to find a $$\theta$$ that maximizes the probability of observing $$X$$,

$$
\mathop{argmax}_{\theta}\Big\{ P(X|\theta) \Big\}
$$

The actual process behind an observed variable can be very complex. Therefore we often resort to a bayesian network that leads to the
that observation. This kind of model has more expressability since we can make objective claims about the generative process itself.

This more complex formalism involves a set intermediate variables, that we don't directly estimate, but, helps us to put more constraint on the model. These variables are commonly denoted by $$Z$$ (termed as latents).

![](https://i.imgur.com/wEJjt9L.png)

Here we don't show $$\theta$$ in the model, but they are there.

###  Formulation

The goal is still the same, to maximize the log-likelihood of the observation.

$$
\begin{align}
\log P(X|\theta) &= \log \int_Z P(X,Z|\theta)dz \\
&= \log \int_Z P(X|Z,\theta) P(Z|\theta) dz \\
&= \log \int_Z q(Z|\theta) \Big[ \frac{P(X|Z,\theta)P(Z|\theta)}{q(Z|\theta)} \Big]dz\\
&= \log \mathbb{E}_{q(Z|\theta)}\Big[ \frac{P(X|Z,\theta) P(Z|\theta)}{q(Z|\theta)} \Big] \\
\end{align}
$$


By using **Jensen's** inequality which says when we have concave function then, any point on the straight-line connecting two points on the concave curve (i.e. $$\mathbb{E}[f(x)]$$) is always lower than the actual mapped point on the curve (i.e. $$f(\mathbb{E}[x])$$), therefore $$\mathbb{E}[f(x)] \leq f(\mathbb{E}[x])$$.

Replacing $$f$$ with $$\log$$ (which is a concave function ), we have

$$
\log{P(X|\theta)} \geq \mathbb{E}_{q(Z|\theta)}\Big[ \log \frac {P(X|Z,\theta) P(Z|\theta)}{q(Z|\theta)} \Big] \label{elbogap}
$$

The numerator factorization is useless, so for brevity

$$
\mathbb{E}_{q(Z|\theta)}\Big[\log\frac{P(X,Z|\theta)}{q(Z|\theta)}\Big] \label{elbo1}

$$


There are many terms that are used to denote equation (\ref{elbo1}), such as **variational lower bound**, **E**vidence **L**ower **Bo**ound (**ELBO**) etc. Intuitively this is average (with respect to a distribution $$q$$) of log-fold-change of joint likelihood of $$(X,Z)$$ and a fictitious variational distribution $$q(Z)$$. There is another way of writing (\ref{elbo1}) if we take denominator out,

$$
\begin{align}
\log P(X|\theta) &\geq \mathbb{E}_{q(Z|\theta)} \Big[ \log P(X,Z|\theta) \Big]- \mathbb{E}_{q(Z|\theta)}[\log q(Z|\theta)] \label{elbo2}
\end{align}
$$
Negative expectation of a log (i.e. $$\int \log (x) p(x)dx$$) is also known as shanon's entropy.


###  Basic EM algorithm

So we see that there is a gap between LHS and RHS in equation (\ref{elbogap}). We can examine the gap further,

$$
\begin{aligned}
\log P(X|\theta) - \mathbb{E}_{q(Z|\theta)} \Big[ \log{\frac{P(X,Z|\theta)}{q(Z|\theta)}} \Big]
\end{aligned}
$$

As $$q(z\|\theta)$$ depends only on $$z$$ ($$\theta$$ is given when we evaluate $$q$$) therefore $$P(X\|\theta)$$
can come inside the  $$\mathbb{E}_{q(z\|\theta)}$$.

Therefore,

$$
\begin{align}
\log P(X|\theta) - \mathbb{E}_{q(Z|\theta)} \Big[ \log{\frac{P(X,Z|\theta)}{q(Z|\theta)}} \Big] &= \mathbb{E}_{q(Z|\theta)} \Big[ \log \frac{P(X|\theta) q(Z|\theta)}{P(X,Z|\theta)} \Big] \\
&= \mathbb{E}_{q(Z|\theta)} \Big[ \log \frac{q(Z|\theta)}{\frac {P(X,Z|\theta)}{P(X|\theta)} } \Big] \\
&= \mathbb{E}_{q(Z|\theta)} \Big[ \log \frac{q(Z|\theta)}{P(Z|X,\theta)} \Big] \label{elbo3} \\
\end{align}
$$

RHS of equation (\ref{elbo3}) denotes the Kullback-leibler divergence between the fictitious distribution of hidden variable $$Z$$ and
the true distribution of $$Z$$, generally denoted by $$KL \big[ q(Z|\theta) || P(Z|X,\theta)  \big]$$,

$$
\begin{align}
\log P(X|\theta) - \mathbb{E}_{q(Z|\theta)} \Big[ \log{\frac{P(X,Z|\theta)}{q(Z|\theta)}} \Big] &= KL
\big[ q(Z|\theta) || P(Z|X,\theta)  \big] \\
\log P(X|\theta) &= \mathbb{E}_{q(Z|\theta)} \Big[ \log{\frac{P(X,Z|\theta)}{q(Z|\theta)}} \Big] + KL
\big[ q(Z|\theta) || P(Z|X,\theta)  \big] \\
\end{align}
$$

ELBO,  $$\mathbb{E}_{q(Z|\theta)} \Big[ \log{\frac{P(X,Z|\theta)}{q(Z|\theta)}} \Big]$$ , can also be
rewritten as, $$\mathcal{L}(q,\theta)$$, and, subsequently,


$$
\log P(X|\theta) = \mathcal{L}(q,\theta) + KL\big[ q(Z|\theta) || P(Z|X,\theta)  \big] \label{elbo4}
$$

where,

$$
\mathcal{L}(q,\theta) = \mathbb{E}_{q(Z|\theta)} \Big[ \log{\frac{P(X,Z|\theta)}{q(Z|\theta)}} \Big] \label{elbo0}
$$



$$KL$$ divergence is a positive quantity.

Equation (\ref{elbo4}) gives us another definition of $$\mathcal{L}$$, or the **variational lower bound**, **E**vidence **L**ower **Bo**ound (**ELBO**),

$$
\begin{align}
\mathcal{L}(q,\theta) = \log P(X|\theta) - KL\big[ q(Z|\theta) || P(Z|X,\theta)  \big] \label{elbo5}
\end{align}
$$

 Equation (\ref{elbo5}) will be useful later.



An iterative algorithm known as EM, is applied. We stepwise optimize $$q$$ and $$\theta$$. We can use EM, these two steps can be executed properly

### Actual Algorithm
Let's assume we already have a $$\theta \leftarrow \theta^0$$

###  Step 1: E step
In this step we will minimize the KL divergence. $$KL(q(Z|\theta^0)||P(Z|X,\theta^0))$$ divergence become 0
if,
$$
q(Z|\theta^0) \leftarrow P(Z|X,\theta^0)
$$

It would be awesome if we can evaluate $$P(X,Z|\theta^0)$$. Remember it amounts to solving the following
 expression.

$$
\begin{align}
P(Z|X,\theta^0) &= \frac{P(X|Z,\theta^0)}{P(X|\theta^0)} \\
&= \frac{P(X|Z,\theta^0)} {\int_z P(X|Z,\theta^0) P(Z|\theta^0) dz} \label{marginal}
\end{align}
$$

The integration in the denominator of equation (\ref{marginal}) is the hardest part here,
for many reasons, such as  $$\int_z P(X|Z,\theta^0) P(Z|\theta^0) dz$$
might not give us an analytical closed form, and might not be integrable.


Say by some superpower (or if numerator of equation (\ref{marginal}) is analytically so simple that you can just integrate) you can solve the equation

$$
\begin{align}
q \leftarrow q^0 = P(Z|X,\theta^0)
\end{align}
$$


Finding this value is not where E-step ends, although that's the crux of it.
We also evaluate ELBO by plugging in $$q^0(Z|\theta) = P(Z|X,\theta^0)$$
in equation (\ref{elbo0})


$$
\mathcal{L}(q^0,\theta) = \mathbb{E}_{q^0(Z|\theta)} \Big[ \log{\frac{P(X,Z|\theta)}{q^0(Z|\theta)}} \Big] \label{estep}
$$

We move to the next step for obtaining a better estimate of $$\theta$$.

### Step 2: M step

Givem $$q^0$$ the next step is to maximize $$\mathcal{L}(q^0, \theta)$$. There can be many ways to solve it. You might want to put the function
$$q^0$$ in ELBO, to evaluate $$\mathcal{L}(q^0,\theta)$$. But that would just yield a function of $$\theta$$,

$$
\theta \leftarrow \theta^1 = \mathop{argmax}_\theta\mathcal{L}(q^0,\theta) \label{mstepopt}
$$

A very naive way to solve equation (\ref{mstepopt}) is to just differentiate it (if you can) and figure out $$\theta^1$$.

That's how EM algorithm proceeds,

$$
\begin{align}
\theta^0 \rightarrow \text{E} \rightarrow q^0 \rightarrow M \rightarrow  \theta^1 \rightarrow \text{E} \rightarrow q^1 \ldots \text{until
convergence}
\end{align}
$$

$$\theta^0$$ can be any value you deem reasonable (taking *any* value might elongate the convergence.)

> FAQ

### Why E step is called E step (or expectation)?

The name comes from evaluating the expectation in equation (\ref{estep}). We in essence evaluate the _expectation_ of the observed data _under_ the
fictitious distribution $$q$$.

### Why it's ELBO is called *variational* lower bound?

Because $$q$$ is called variational ditribution. In the case EM we got super lucky and evaluated $$q$$, that's not the case in most problems,
in which case we resort to a variational distribution $$q$$ and given that *variational* distribution ELBO is a *lower bound*. You can
evaluate $$\mathcal{L}(q^{i},\theta^i{})$$  and after each $$i$$-th $$M$$ step, and plot this value. If the implementation is successful we would
see an increasing curve that saturates with $$i$$ denoting that lower bound is getting improved.



### What if   $$\int_z P(X|Z,\theta^0) P(Z|\theta^0) dz$$  is a hard nut to crack (intractable)?

If we can't solve the integration $$\int_z P(X|Z,\theta^0) P(Z|\theta^0) dz$$  then we have to figure out
 other ways to lower the KL divergence. We of course can not make it 0 under $$\theta^0$$, but may be, we
 can get closer, and KL divergence would be a small value.

We still know that our best bet is to evaluate $$P(Z|X,\theta^0)$$ and therefore evaluating $$\int_z P(X|Z,\theta^0) P(Z|\theta^0) dz$$  or
$$\mathbb{E}_{P(Z|\theta^0)} [P(X|Z,\theta^0)]$$ , if we can't do that
exactly, then to the least we can use *numerical* algorithms in order to estimate this integration
approximately. There are many approximation algorithms that can give us numeric estimates of an actual
integration.


### 1. Try half-hearted EM nevertheless

A very naive technique of would be to sample a bunch of points from the ditribution
$$P(Z|\theta^0)$$ (which can be an intimidating task on
it's own). and evaluate $$P(X|Z,\theta^0)$$.

To be more concrete you sample (there are many many good algorithms for well-structured sampling such as
MCMC, gibbs) a vector (hidden variables) $$\mathbf{z}^i \sim P(Z|\theta^0)$$ (if you can) and evaluate $$P(X |\mathbf{z}^i,\theta^0)$$.
Say, do it  $$D$$ (a bunch of) times, then our best hope is to calculate

$$
\frac{1}{D} \sum_{i} P(X|\mathbf{z}^i,\theta)
$$

This approximation looks simple, **but** there could be caveats of such a solution, such as,

- The expression $$\frac{1}{D} \sum_{i} P(X|\mathbf{z}^i,\theta)$$  can be far from true $$\int_z P( |Z,\theta^0) P(Z|\theta^0) dz$$
when $$\mathbf{z}^i$$ is not well distributed, or are autocorrelated (
therefore does not capture the space of true hidden variable distribution).

-  We can't simply sample from $$P(Z|\theta^0)$$, because there is no analytical form to it. (This happens
a lot in a real world problem)

### 2. Mean field Inference

(To be written)



### 3. Variational autoencoder

Equation (\ref{elbo5}) states $$\mathcal{L}(q,\theta) = \log P(X|\theta) - KL\big[ q(Z|\theta) || P(Z|X,\theta)
\big]$$ , where we had taken help of a distirbution $$q$$. We realized that the $$E$$ step finds out a $$q$$
that minimizes the KL divergence part given an initial estimate of $$\theta$$, $$\theta^0$$. When we are
lucky we can set $$q(Z|\theta^0)$$ to the "true" estimate to $$P(Z|X,\theta^0)$$. In that case we are certain that
we can fully evaluate $$P(Z|X,\theta^0)$$, by getting a solution to  equation (\ref{marginal}) (doing integration etc.).

When we are _not_ so lucky (which is most of the cases), we parameterize $$q$$ and try to find out $$q$$
accordingly. Before getting into that, let's first realize a fact when we used in equation (\ref{mstepopt}), we
actually did the following,

$$
\begin{align}
\mathop{argmax}_q\mathcal{L}(q,\theta^0) &= \mathop{argmax}_q \Big[ \log P(X|\theta^0) - KL\big[ q(
Z|\theta^0) || P(Z|X,\theta^0)  \big] \Big] \\
&= \mathop{argmax}_q KL\big[ q(Z|\theta^0) || P(Z|X,\theta^0)  \big] \\
\end{align}
$$

Therefore $$\mathcal{L}$$ can be treated as a key component in this entire exercise.

Now, when $$P(Z|X,\theta^0)$$ is intractable, we cannot set $$q(Z|\theta^0)$$ to $$P(Z|X,\theta^0)$$, *however*
we define another parameterized function $$q_\phi(Z|X)$$ (Notice we removed $$\theta^0$$ here, we assume this
function $$q_\phi$$ is not dependent on the model parameters, but rather another set of parameters which
are all hidden in function $$\phi$$.).  Our hope $$q_\phi(Z|X)$$ will be close to $$P(Z|X,\theta^0)$$.

Now coming back to our discussion about not having a good $$q$$, one can think about parameterizing $$q	$$
itself as a function of some variables, and try to improve those parameters. In other words if we can get
a $$q_{\phi}$$ that is so flexible that it can be moulded in any way we want, that would be great. One such
a function is neural networks. We can assume the parameters in the network (all the weights and nonlinear
activation parameters) are encoded by $$\phi$$ then instead of using $$q$$ , we can call such a neural
network $$q_\phi$$.

> In fact you can think $$q_{\phi}(Z|X)$$
as an output from **_encoder_** part of the auto-encoder.

In the light of this new notation we can re-write equation (\ref{elbo5}), as

$$
\begin{align}
\mathcal{L}(\theta,\phi) =  \log P(X|\theta) - KL\big[ q_\phi(Z|X) || P(Z|X,\theta)  \big]
\end{align}
$$

We would simplify the above equation a but by assuming that we first like to find out the best $$\phi$$ for
one data point $$\mathbf{x}^i$$. We would also assume $$\mathbf{x}^i$$ is dependent on a subset of hidden
variables $$\mathbf{z}^i$$, with this assumptions, we can write likelihood for one data point
$$\mathbf{x}^i$$,

$$
\begin{align}
\mathcal{L}(\theta, \phi) &=  \log P(\mathbf{x}^i|\theta) - KL\big[ q_\phi(\mathbf{z}^i|\mathbf{x}^i) ||
P(\mathbf{z}^i|\mathbf{x}^i,\theta)  \big] \\
&=  \log P(\mathbf{x}^i|\theta) - \int_z q_\phi(\mathbf{z}^i|\mathbf{x}^i) \log \frac{q_
\phi(\mathbf{z}^i|\mathbf{x}^i)}{P(\mathbf{z}^i|\mathbf{x}^i,\theta)} d\mathbf{z}^i
\end{align}
$$

$$\log P(\mathbf{x}^i|\theta)$$
does not contain anything to do with $$q_\phi$$. We can rewrite
$$\log P(\mathbf{x}^i|\theta) = \int_z \log P(\mathbf{x}^i|\theta) q_\phi(\mathbf{z}^i|\mathbf{x}^i)
d\mathbf{z}^i$$, since $$q_\phi(\mathbf{z}^i|\mathbf{x}^i)$$ is a probability distribution and therefore,
$$\int_z q_\phi(\mathbf{z}^i|\mathbf{x}^i,\theta) d\mathbf{z}^i = 1$$. We used the similar trick while
absorbing  $$\log P(X|\theta)$$ inside expectation in equation 12.

$$
\begin{align}
\mathcal{L}(\theta, \phi) &= \int_z \Big[ q_\phi(\mathbf{z}^i|\mathbf{x}^i) \log P(\mathbf{x}^i|\theta)
-  q_\phi(\mathbf{z}^i|\mathbf{x}^i) \log \frac{q_
\phi(\mathbf{z}^i|\mathbf{x}^i)}{P(\mathbf{z}^i|\mathbf{x}^i,\theta)} \Big]  d\mathbf{z}^i \\
&= \int_z q_\phi(\mathbf{z}^i|\mathbf{x}^i) \log \frac{P(\mathbf{x}^i|\theta)
P(\mathbf{z}^i|\mathbf{x}^i,\theta)}{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}  d\mathbf{z}^i
\end{align}
$$

We can massage the expression $$P(\mathbf{x}^i|\theta) P(\mathbf{z}^i|\mathbf{x}^i,\theta)$$,
$$
\begin{align}
P(\mathbf{z}^i|\mathbf{x}^i,\theta) &= \frac {P(\mathbf{x}^i|\mathbf{z}^i,\theta) P(\mathbf{z}^i |
\theta)}{P(\mathbf{x}^i | \theta)} \\
P(\mathbf{x}^i|\theta) P(\mathbf{z}^i|\mathbf{x}^i,\theta) &= P(\mathbf{x}^i|\mathbf{z}^i,\theta) P(\mathbf{z}^i | \theta) \\
\end{align}
$$

The point of this mathematical juggling would be clear in a moment,

Our new  **variational lower bound** or **E**vidence **L**ower **Bo**ound (**ELBO**) is,

$$
\begin{align}
\mathcal{L}{(\theta,\phi)} &= \int_z q_\phi(\mathbf{z}^i|\mathbf{x}^i) \log
\frac{P(\mathbf{x}^i|\mathbf{z}^i,\theta) P(\mathbf{z}^i | \theta)}{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}
d\mathbf{z}^i \\
&= \int_z q_\phi(\mathbf{z}^i|\mathbf{x}^i)\log P(\mathbf{x}^i|\mathbf{z}^i,\theta)  d\mathbf{z}^i - \int_
z q_\phi(\mathbf{z}^i|\mathbf{x}^i) \log \frac{ q_\phi(\mathbf{z}^i|\mathbf{x}^i)}{P(\mathbf{z}^i |
\theta)} d\mathbf{z}^i \\
&= \mathbb{E}_{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}\log P(\mathbf{x}^i|\mathbf{z}^i,\theta) - KL[q_
\phi(\mathbf{z}^i|\mathbf{x}^i)||P(\mathbf{z}^i | \theta)] \label{vaeelbo}
\end{align}
$$

We are again in trouble, since the first part of equation (\ref{vaeelbo}) is also an expectation, but since
$$q_\phi(\mathbf{z}^i|\mathbf{x}^i)$$ is something that we can choose, we can take enough samples and
approximate the expression.

We need two different outcomes, firstly, we want to optimize ELBO, $$\mathcal{L}$$ , and get the best
values for $$\theta$$ and $$\phi$$. On the other hand, we need to evalueate $$\mathcal{L}$$.

###  - How do we optimize the ELBO?

- **E-like** step (finding optimal $$\phi$$ given $$\theta^0$$)

We follow something similar to EM, but a bit more complex, in the *E-like* step we find out a $$\phi$$ that
maximizes $$\mathcal{L}$$. (given an initial value $$\theta^0$$)

$$
\begin{align}
\phi^0 \leftarrow \mathop{argmax}_\phi \mathcal{L}(\phi,\theta^0)
\end{align}
$$


We differentiate $$\mathcal{L}(\phi,\theta^0)$$ w.r.t $$\phi$$,

$$
\nabla_\phi \Big[\mathbb{E}_{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}\log P(\mathbf{x}^i|\mathbf{z}^i,\theta^0)
- KL[q_\phi(\mathbf{z}^i|\mathbf{x}^i)||P(\mathbf{z}^i | \theta^0)]\Big]
$$

To understand how we optimize ELBO, first we have to understand how in reality such an expression
$$\mathcal{L}$$, is formed. Because we are not going to differentiate the exact form of equation (\ref{vaeelbo})
directly. This optimization scheme is very much tied to the actual generation process. To briefly
understand that, let's review the flow of inference.

$$
\mathbf{x}^i \rightarrow \phi(\mathbf{z}^i|\mathbf{x}^i) \rightarrow q_\phi(\mathbf{z}^i|\mathbf{x}^i)
\rightarrow P(\mathbf{x}^i|\mathbf{z}^i,\theta) \label{vaesteps}
$$

Given the data $$\mathbf{x}^i$$, a trainable parameterized function $$\phi$$  (such as _encoder_ network), is
used to generate a set of hidden variable (often called **latent** variables) $$\mathbf{z}^i$$, we use
another *easy* function $$q$$ (such as a normal distribution).

Given this super artificial distribution for $$q_\phi(\mathbf{z}^i|\mathbf{x}^i)$$,
we evaluate $$\mathbb{E}_{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}\log P(\mathbf{x}^i|\mathbf{z}^i,\theta)$$.
However, this expectation could be complex. As we resort to sampling

So the approximation process could be drawing a sample
$$\mathbf{z}_d^i \sim q_\phi (\mathbf{z}^i|\mathbf{x}^i)$$ and then evaluate
$$\log P(\mathbf{x}^i|\mathbf{z}_d^i,\theta)$$,
subsequently,


Summing them up and averaging would give us an approximation of

$$
\mathbb{E}_{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}\log P(\mathbf{x}^i|\mathbf{z}^i,\theta) \approx
\frac{1}{D} \sum_d \log P(\mathbf{x}^i|\mathbf{z}_d^i,\theta) \\
$$


The last part of \ref{vaesteps} is another neural network, that can be termed as a **_decoder_** with a set
of parameters $$\theta$$ (this part is very similar to a normal heirarchical bayesian model) . With this
sampling process we redefine the definition of ELBO,

$$
\mathcal{L}(\phi, \theta) \approx \frac{1}{D} \sum_d \log P(\mathbf{x}^i|\mathbf{z}_d^i,\theta) + KL[q_
\phi(\mathbf{z}^i|\mathbf{x}^i)||P(\mathbf{z}^i | \theta)]
$$


At last differentiating the above expression would lead to optimization.
If we want to differentiate the first part of 48, then we will face a
problem, since $$\nabla_\phi \Big[\frac{1}{D} \sum_d \log P(\mathbf{x}^i|\mathbf{z}_d^i,\theta)\Big]$$
is hard to evaluate.
Since the sampling is inside the summation notation depends on $$\phi $$ , specifically, $$\mathbf{z}_d^i \sim q_\phi (\mathbf{z}^i|\mathbf{x}^i)$$ itself depends on $$\phi$$.

-----

This is a classical problem and this does does not have anything to do with sampling in particular,

In short this problem is formulated as following,

$$
\begin{align}
\nabla_h\mathbb{E}_{y(h)}[f(y)] &= \nabla_h \int_y f(y)P(y(h))dy \label{logtrick} \\
&= \int_y \nabla_h P(y(h))f(y)dy
\end{align}
$$

Here probability of the distribution for wihich the expectation is taken depends on a variable $$h$$, and
we are differentiating w.r.t that variable.   One way to solve this problem is something called _log
derivative tick_ , which by now might be familiar: we multiply and divide the expresseion inside the
expression by $$P(y(h))$$.

$$
\begin{align}
\nabla_h\mathbb{E}_{y(h)}[f(y)] &= \int_y \frac{P(y(h))}{P(y(h))} \nabla_h P(y(h))f(y)dy \\
&= \int_y P(y(h)) \nabla_h \log P(y(h)) f(y) dy \\
&= \mathbb{E}_{y(h)} [\nabla_h \log P(y(h))f(y)]
\end{align}
$$

Effectively this is a good solution if we go back to equation (\ref{logtrick})

----

With the log-trick, we at last use this trick in sampling for evaluating ELBO

$$
\frac{1}{D} \sum_d \nabla_\phi [\log q_\phi(\mathbf{z}^i | \mathbf{x}^i)] \log P(\mathbf{x}^i|\mathbf{z}_
d^i,\theta)​
$$

Although this expression might look simple, it leads to a very bad sampling.

> Why?

Let's think about a real world scenario, where we start with some random $$\theta^0$$ and $$\log
P(\mathbf{x}^i|\mathbf{z}^i,\theta^0)$$ denotes the probability of generating an image from some random
initalization, this probability can be very low number (log likelihood of images $$\log10^{-10^6} = -10^6$$). So while sampling we choose
$$\mathbf{z}_d^i \sim q_\phi (\mathbf{z}^i|\mathbf{x}^i)$$ and then
take derivative of log of that  with respect to $$\phi$$, which could be negative or positive, multiply
with a highly negative number, becoming a very large or small number. So the sampling mechanisms (such as
MCMC) will take a long time to converge.



Another way of solving this problem is _change of variable_ or _reparameterization_. As opposed to _log
derivative trick_ we use change of variables in the calculation of expectation. This method is widely
popular in calculus,

$$
\begin{align}
\int_y f(y) dy &= \int_x f(g(x)) g'(x) dx \ \ \ \ (\text{given } y=g(x))
\end{align}
$$

One could easily think about this in terms expectation

$$
\begin{align}
\mathbb{E}_y[f] &= \int_y f(y) P(y) dy \\
&= \int_x f(g(x)) P(g(x)) g'(x) dx \ \ \ \ (\text{given } y=g(x))
\end{align}
$$

If $$P(g(x))g'(x)$$ can be represented as a probability distribution $$P'(x)$$ then

$$
\mathbb{E}_y[f] = \int_x f(g(x)) P'(x) dx = \mathbb{E}_x[f(g(x))]
$$

So basically we can change the expectation altogether.

In our particular case let's assume we have one dimensional world (just for the ease of calculation),


$$
\begin{align}
q_\phi(\mathbf{z}^i|\mathbf{x}^i) &= \mathcal{N}(\mu,\sigma) \ \ \ \text{(output of encoder network)} \\
\end{align}
$$

 is same as first sampling  $$\epsilon \sim \mathcal{N}(0,1)$$ and then multiplying by $$\sigma$$ and scaling
 by $$\mu$$,


$$
\begin{align}
q_\phi &= \mu + \sigma *\mathcal{N}(0,1) \ \ \ \text{(decompoistion with normal noise)} \\
\mathbf{z}^i &= \mu + \sigma \epsilon = g(\epsilon), g'(\epsilon) = \sigma \ \ \text{(differential w.r.t}, \epsilon) \\
q_\phi(\mathbf{z}^i | \mathbf{x}^i) &= \frac{1}{\sqrt{2\pi}\sigma} \exp{\Big[-\frac{1}{2} {\big(\frac{\mathbf{z}^i -
\mu}{\sigma}\big)}^2\Big]} \\
P(g(\epsilon))g'(\epsilon) &= \frac{1}{\sqrt{2\pi}\sigma} \exp{\Big[-\frac{1}{2} {\epsilon}^2\Big]\sigma} = \frac{1}{\sqrt{2\pi}}
\exp{\Big[-\frac{1}{2} {\epsilon}^2\Big]} = \mathcal{N}(\epsilon;0,1)
\end{align}
$$


So we change our sampling mechanism a bit, we assume $$\epsilon^i_d \sim N(0,1) $$ and we compute
$$\mathbf{z}_d^i = g(\epsilon_d^i)$$ , as the $$\mu, \sigma$$ are a result of $$\mathbf{x}^i$$ and $$\phi$$, so
it's a loded term and can also be stated as $$g(\epsilon^i_d,\mathbf{x}^i,\phi)$$

$$
\mathbb{E}_{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}\log P(\mathbf{x}^i|\mathbf{z}^i,\theta) = \mathbb{E}_
{p(\epsilon)} \log P(\mathbf{x}^i | g(\epsilon^i,\mathbf{x}^i,\phi), \theta)
$$


Therefore when we differentiate with respect to, $$\phi$$ (_decoder_)

$$
\begin{align}
\nabla_\phi \Big[\mathbb{E}_{q_\phi(\mathbf{z}^i|\mathbf{x}^i)}\log
P(\mathbf{x}^i|\mathbf{z}^i,\theta^0)\Big] &= \nabla_\phi \Big[\mathbb{E}_{p(\epsilon)} \log
P(\mathbf{x}^i | g(\epsilon^i,\mathbf{x}^i,\phi), \theta^0) \Big] \\
&= \mathbb{E}_{p(\epsilon)} \Big[ \nabla_\phi \log P(\mathbf{x}^i | g(\epsilon^i,\mathbf{x}^i,\phi),
\theta^0) \Big] \\
&\approx \frac{1}{D} \sum_d \nabla_\phi \log{P(\mathbf{x}^i | g(\epsilon^i_d,\mathbf{x}^i,\phi),
\theta^0)}
\end{align}
$$

In the real world we can offload these differentials to tensorflow ,



This can be mathematically intimidating. **But**, we can choose variational distributions that have nice forms then we have some hope. We
have absolutely no controle over entire $$P(\mathbf{x}^i|\mathbf{z}^i,\theta^0)$$ , since this denotes true data generation process (for
example a neural network), if we create simplistic assumptions for this distribution then the point of the exercise is futile. *However* we
can of course choose, a $$q_{\phi}$$ that is manageable and a prior distribution $$P(\mathbf{z}^i|\theta_0)$$.

> In case of autoencoder we can assume $$q$$ is a multivariate normal, which depnds on the output of a complex function $$\phi$$.
> We can assume $$\mu =\phi(\mathbf{z}^i|\mathbf{x}^i)$$ and $$\sigma = \phi(\mathbf{z}^i|\mathbf{x}^i)$$, (or one can think of any other
function on those)
> $$
> q_\phi(\mathbf{z}^i|\mathbf{x}^i) = \mathcal{N}(\mu_\phi,\sigma_\phi)
> $$
> That is we can use the the $$\mathbf{z}^i$$ obtained from _encoder_ network as both mean and variance for a normal distribution. If we
assume
> $$
> P(\mathbf{z}^i|\theta^0) = \mathcal{N}(0,I)
> $$
> We will see by choosing both of them as normal the KL divergence becomes so simple that we will have a analytical closed form. We will
discuss the differentiability of such function shortly.



- **M-like** w.r.t $$\theta$$ (finding optimal $$\theta$$ given $$\phi^0$$)

To optimize equation 39, we would first optimize the first part  w.r.t. $$\theta$$ ,
$$
\nabla_\theta [\mathbb{E}_{q_\phi(\mathbf{z}^i|\mathbf{x}^i,\theta)}\log P(\mathbf{x}^i|\mathbf{z}^i,\theta)] = \mathbb{E}_{q_
\phi(\mathbf{z}^i|\mathbf{x}^i,\theta)} [ \nabla_\theta \log P(\mathbf{x}^i|\mathbf{z}^i,\theta) ]
$$
This can also be evaluated by using sampling

$$
q_{\phi^0}(\mathbf{z}^i|\mathbf{x}^i,\theta) \approx \frac{1}{D} \sum_d \nabla_\theta \log{P(\mathbf{x}^i | g(\epsilon^i_
d,\mathbf{x}^i,\phi^0), \theta)}
$$

A full implemenration of the above described equation can be found [1].


###  References

[1]: https://krasserm.github.io/2019/12/17/latent-variable-models-part-2/

[2]: http://legacydirs.umiacs.umd.edu/~xyang35/files/understanding-variational-lower.pdf

[1]: https://krasserm.github.io/2019/12/17/latent-variable-models-part-2/

[2]: http://legacydirs.umiacs.umd.edu/~xyang35/files/understanding-variational-lower.pdf

[3]: https://gregorygundersen.com/blog/2018/04/29/reparameterization/

[4]: https://cedar.buffalo.edu/~srihari/CSE676/21.1-VAE-Theory.pdf

[5]: https://arxiv.org/pdf/1312.6114.pdf

[6]: https://ermongroup.github.io/cs228-notes/extras/vae/

[7]: http://www.cs.columbia.edu/~blei/papers/RanganathGerrishBlei2014.pdf

[8]: https://arxiv.org/pdf/1401.4082.pdf

[9]: https://arxiv.org/pdf/1606.05908.pdf

[10]: https://www.borealisai.com/en/blog/tutorial-5-variational-auto-encoders/ (one of the best explanation of vae)

[11]: https://arxiv.org/pdf/1906.02691.pdf

[12]: [Youtube list](https://www.youtube.com/playlist?list=PLJr1QMzD0i5vzR0rGRwFkEYqqhBbQD6D2)

[1] https://krasserm.github.io/2019/12/17/latent-variable-models-part-2/

[2] http://legacydirs.umiacs.umd.edu/~xyang35/files/understanding-variational-lower.pdf

[3] https://gregorygundersen.com/blog/2018/04/29/reparameterization/

[4] https://cedar.buffalo.edu/~srihari/CSE676/21.1-VAE-Theory.pdf

[5] https://arxiv.org/pdf/1312.6114.pdf

[6] https://ermongroup.github.io/cs228-notes/extras/vae/

[7] http://www.cs.columbia.edu/~blei/papers/RanganathGerrishBlei2014.pdf

[8] https://arxiv.org/pdf/1401.4082.pdf

[9] https://arxiv.org/pdf/1606.05908.pdf

[10] https://www.borealisai.com/en/blog/tutorial-5-variational-auto-encoders/ (one of the best explanation of vae)

[11] https://arxiv.org/pdf/1906.02691.pdf

[12] [Youtube list](https://www.youtube.com/playlist?list=PLJr1QMzD0i5vzR0rGRwFkEYqqhBbQD6D2)