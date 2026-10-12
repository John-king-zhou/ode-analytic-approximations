# Validity and error assessment

Choose evidence proportional to the claim. An exploratory leading approximation may need a residual and regime check; a quantitative or uniform accuracy claim needs stronger support.

## Symbolic checks

- Substitute the reconstructed approximation into the original equations, including derivative transformations and eliminated variables. For `M*y'=F`, retain the original mass matrix as well as any normalized formulation.
- Check initial and boundary data at the retained order. Quantify any mismatch and state whether an outer-only formula intentionally excludes the layer.
- Check physical dimensions after returning to original variables, conserved quantities, signs, and feasible branches.
- Test unperturbed and relevant limiting cases. Keep parameter-dependent initial data in these checks.
- Identify the first nonvanishing residual coefficient and the scaling of that coefficient throughout the claimed domain. Include derivatives, fast coordinates, and possible secular factors. A residual of `epsilon^2*t` has a different order when `t=O(1/epsilon)`.

For an approximate solution, compute and expand the residual explicitly. An exact-solution checker reporting nonzero residual can be expected; quantify it instead of declaring the approximation wrong. Do not silently replace a failed symbolic simplification with a numerical assertion of identity.

## Residual versus solution error

For the normalized IVP `y'=F(t,y,epsilon)`, define

`r(t)=y_a'(t)-F(t,y_a(t),epsilon)`.

If F is Lipschitz in y with constant L on a region containing both trajectories, a standard finite-interval estimate is

`||y(t)-y_a(t)|| <= exp(L*(t-t0))*||y(t0)-y_a(t0)||`

`                       + integral[t0,t] exp(L*(t-s))*||r(s)|| ds`.

This follows by integrating the error equation and applying Gronwall's inequality. State the region and whether L is independent of epsilon. If the integral or stability factor grows over the requested horizon, do not advertise the residual order as a uniform error order. Contraction, a fundamental matrix, or an appropriate energy estimate can give a sharper bound when justified.

For `epsilon*x'=f`, a raw residual `R=epsilon*x_a'-f(x_a,...)` becomes `R/epsilon` in the normalized evolution equation. The fast Jacobian also scales with `1/epsilon`. A generic Gronwall estimate may therefore be useless even for a stable problem; attraction, layers, and a bound appropriate to the singular system are essential. Conversely, small raw residuals can coexist with large error for an unstable fast branch.

For BVPs, an IVP estimate does not establish boundary-value accuracy. Use the conditioning of the boundary-value operator, its Green function, or a relevant stability theorem. Near a singular operator or bifurcation, small residual and boundary mismatch may be amplified.

Distinguish these statements:

- **Formal:** coefficient equations are satisfied through the retained order under the stated scaling.
- **Numerically supported:** comparisons at stated parameter values and intervals support the claimed behavior.
- **Bounded analytically:** an explicit stability or remainder argument bounds solution error under stated assumptions.

A formal asymptotic series need not converge. Define the norm and domain when writing an `O(epsilon^p)` error claim; keep its constant independent of epsilon on that domain, or explain any dependence that weakens the claim.

## Numerical comparison when useful

Integrate the original model with the original data. Compare both trajectories and the requested observable; do not validate only a simplified model against itself.

Use a stiff integrator where timescale separation warrants it, check success and termination, and resolve the fast transient in both computation and comparison samples. With SciPy, `Radau` or `BDF` suit stiff IVPs; `DOP853` is an option for accurate nonstiff IVPs. `t_eval` specifies output samples, not internal step sizes; use `max_step` when internal resolution is needed. For BVPs use a suitable BVP solver and check boundary residuals and convergence.

Choose tolerances according to state scales and the anticipated approximation error. Tighten tolerances or refine a BVP mesh to check that the reference is stable at the accuracy being reported. Solver tolerance controls local error estimates and is not itself a certificate of global accuracy.

Measure a sup error or another norm appropriate to the target. Use absolute or physically scaled error near zeros; naive relative errors can diverge at zero crossings. Sample initial/boundary layers, long-time behavior if claimed, and regions near suspected breakdown.

For an asymptotic error claim, compare several decreasing epsilon values with all other dimensionless parameters fixed according to the proposed ordering. Specify whether the interval is fixed or grows with epsilon. If `E(epsilon)` is an error measure, estimate an observed order from

`p_obs = log(E(epsilon1)/E(epsilon2)) / log(epsilon1/epsilon2)`.

Check for numerical error floors, phase effects, and pre-asymptotic behavior before interpreting this slope. Finite tests support behavior over sampled cases; they do not prove a uniform bound.

## Useful result content

Include the formula or reduced equations; reconstruction to physical variables; parameter ordering; assumptions and branch; retained order; admissible time/state domain; residual and condition checks; evidence supporting accuracy; and breakdown conditions. Include only material needed to assess the requested result.

If the user requests a standalone LaTeX document, use the built-in LaTeX editor workflow. Otherwise math stays inline; this skill does not require document generation.

Official computation sources: [SciPy solve_ivp](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_ivp.html), [SciPy solve_bvp](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_bvp.html), and [SymPy ODE checks](https://docs.sympy.org/latest/modules/solvers/ode.html).
