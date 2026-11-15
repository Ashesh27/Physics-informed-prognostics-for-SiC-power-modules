# Physics-informed prognostics for SiC power modules — a worked prototype

**Author:** Ashesh Kumar Gupta  ·  prepared as a companion to my application for *CPEC DC5: power-module health monitoring and lifetime estimation for SiC modules*.

The DC5 project frames prognostics as a **world-modelling problem**: a latent degradation state described by a *monotone jump process (subordinator)* that runs in an **operational time** set by Arrhenius / Coffin–Manson stress, delivering **RUL distributions with calibrated tails** and answering **counterfactual mission-profile questions**. This notebook builds a small, fully runnable version of that pipeline end to end:

| § | What it does | Position requirement it exercises |
|---|---|---|
| 1 | PV-type mission profile → Foster thermal model → daily $T_{j,\max}$, $\Delta T_j$ → **operational-time clock** (Norris–Landzberg form: Arrhenius × Coffin–Manson) | device physics enters as the *clock*, not as a loss penalty |
| 2 | Gamma process + compound-Poisson jumps (a **subordinator**) time-changed by that clock, with exactness tests | Lévy / jump processes |
| 3 | **Sequential Monte Carlo** (particle filter with Liu–West parameter learning) on noisy, temperature-contaminated health indicators | Bayesian inference, SMC |
| 4 | Fan chart of RUL distributions, then **simulation-based calibration** of RUL intervals against a naive regression baseline | *calibrated tails* |
| 5 | **Counterfactual queries**: de-rate the load by 10 %; find the load that keeps the device alive to the next inspection with probability 0.99 | conditional / counterfactual queries |
| 6 | **Experimental design**: which accelerated-ageing stress levels make the Coffin–Manson exponent identifiable? | "data produced around the requirements of the model" |

> **Scope and honesty.** Everything here is *synthetic*. Parameter values (thermal network, activation energy, exponent, failure criterion, jump rates) are illustrative and **not** calibrated to any real SiC module; the model is *well specified* by construction, so the calibration results in §4 show that the inference machinery is correct, not that the physics is right. Real accelerated-ageing data are exactly what the DC5 project would add.
>
> **This notebook was developed through iterative implementation, numerical testing and independent study of the underlying mathematical methods. AI-assisted coding tools were used during development for coding assistance and debugging. The modelling assumptions, implementation choices, numerical experiments and interpretation of results are presented here as a research prototype and are the author's responsibility.**

