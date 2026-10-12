---
name: ode-analytic-approximations
description: Derive and validate approximate analytical solutions of ordinary differential equation models using scaling, perturbation methods, timescale separation, and local reduction. Use for asymptotic ODE analysis, quasi-steady-state approximations, initial or boundary layers, weakly nonlinear oscillations, and assessing approximation validity.
---

# ODE Analytical Approximations

Turn an ODE model into a justified analytical approximation with an explicit parameter regime, time interval, and error assessment. Support scalar equations, coupled systems, initial-value problems, and boundary-value problems. Work inline unless the user requests an artifact.

## Establish the approximation target

Extract the equations, initial or boundary conditions, parameter ranges, units, and quantity of interest. Determine whether the user needs trajectories, steady states, oscillation frequency, transient duration, or another observable; accuracy of one observable need not imply accuracy of the whole trajectory.

Ask only for missing information that changes the analysis. When numerical parameter values are absent, derive conditional symbolic regimes. Label assumptions; do not manufacture parameter values or infer smallness from a dimensional coefficient alone.

Check for an accessible exact solution, quadrature, conservation law, or variable transformation before constructing an expansion. Keep this check proportional to the problem; an unresolved symbolic solver call is not evidence that no analytical solution exists.

## Scale and diagnose

Choose state and time scales from the model and the requested regime. Record the transformation back to physical variables and the dimensionless parameter combinations. Preserve constraints such as nonnegative concentrations, conserved totals, and admissible equilibria.

Compare terms on the proposed solution scales. For several small parameters, specify their ordering or separate distinguished regimes. Allow fractional powers, logarithms, and exponential scales when balance requires them.

Determine whether the proposed limit loses differential order, initial/boundary data, stability, or uniformity in time. Check whether weak terms accumulate over the requested interval. A small coefficient on a highest derivative, a fast transient, or secular growth requires attention to additional scales.

## Choose and derive a method

Read the relevant section of [references/methods.md](references/methods.md) when choosing or applying a method:

| Diagnostic | Method and reference |
|---|---|
| Smooth parameter dependence and an ordered correction on the target interval | [Regular perturbation](references/methods.md#regular-perturbation) |
| Stable fast relaxation toward an isolated constraint manifold | [Quasi-steady-state reduction](references/methods.md#quasi-steady-state-reduction) |
| Missing initial/boundary conditions or a localized rapid transition | [Matched asymptotics](references/methods.md#matched-asymptotics) |
| Weak oscillatory effects accumulate or corrections develop secular terms | [Multiple scales and averaging](references/methods.md#multiple-scales-and-averaging) |
| A periodic orbit needs a frequency correction | [Poincare-Lindstedt](references/methods.md#poincare-lindstedt) |
| Small deviation from an equilibrium; no justified global small parameter | [Local linearization and dominant balance](references/methods.md#local-linearization-and-dominant-balance) |

Explain the chosen balance, expansion, and leading equations. Derive the order needed for the user's accuracy target, showing steps that justify the approximation. Apply conditions consistently at each order; explicitly identify conditions supplied through a layer or matching. Reconstruct eliminated variables when they are requested.

If a reduction leaves an unsolved ODE, identify it as a reduced model. Offer a quadrature, special-function expression, implicit solution, or a further justified approximation when helpful. Distinguish these from an explicit closed-form trajectory.

If the assumed regime fails, give the failure condition and an appropriate alternative regime or local result. Avoid extending a formula past a fold, turning point, separatrix, blow-up, or loss of fast stability without a new analysis.

## Validate and communicate

Read [references/validation.md](references/validation.md) for residuals, error propagation, and numerical comparison when making an accuracy or validity claim.

Substitute the approximation into the original equations, check initial/boundary conditions and physical constraints, and test relevant limiting cases. Evaluate residuals in the correct scaled equations and in fast layers when present. Residual order alone does not establish solution-error order.

State the parameter regime, time/state domain, order retained, and breakdown conditions. Distinguish a formal expansion, a proven error estimate, and a numerical comparison. A numerical fit or an assumed truncation order is not a uniform error bound. For long-time or singular claims, include the stability or conditioning argument needed to turn residuals into trajectory error, or label the claim formal.

When numerical comparison would resolve a meaningful uncertainty and execution is available, compare the original model with the approximation on the intended interval and relevant parameter values. Resolve fast layers and refine solver tolerances enough to distinguish numerical error from approximation error. Report what was actually checked. If computation is unavailable, preserve the derivation and state the unperformed check.

Present the useful result first, followed by assumptions, the decisive derivation, and validity evidence. Include physical-unit reconstruction when the input is dimensional. Scale detail to the user's request; no fixed report format or exhaustive method survey is required.

## Supporting skills and sources

When available, use `sympy` for exact coefficient algebra, series, and residual calculations, and `math-reasoning` for formal derivations or proof checks. This skill remains usable without either. Resolve companion references from their actual installed skill directories rather than assuming Claude-specific paths.

Use local Python dependencies when available; symbolic assistance is optional. Consult current official documentation if unfamiliar library behavior affects the result. Never claim a computation was run unless it was executed.

The method references contain primary academic and official software sources for deeper analysis. Cite sources for a specific theorem or nontrivial borrowed result when used; verify its hypotheses for the model at hand.
