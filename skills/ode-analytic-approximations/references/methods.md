# Selecting and constructing ODE approximations

Use only the sections relevant to the diagnosed regime. These are decision guides; derive coefficients from the actual model.

## Regular perturbation

For a smooth IVP `y' = F(t,y,epsilon)` on a stated interval, try

`y = y0 + epsilon*y1 + ...`.

With `F = F0(t,y) + epsilon*F1(t,y) + ...`, the first equations are

`y0' = F0(t,y0)`,

`y1' = D_y F0(t,y0)*y1 + F1(t,y0)`.

Expand the initial data too. Epsilon-independent initial data give `y0(t0)=y_init` and `y1(t0)=0`; parameter-dependent data contribute their own coefficients. For higher-order scalar equations, include derivative conditions at their proper order.

Check the ordering of successive terms on the requested interval. A correction proportional to `epsilon*t` can invalidate a fixed-time expansion on `t=O(1/epsilon)`. Near zeros of the leading solution, compare absolute or scaled errors rather than ratios to that solution. Reconsider the expansion if coefficients become singular or a branch changes.

Use exact arithmetic for symbolic coefficients. Preserve an order term until the truncation is explicit, and do not take a fixed-time series as evidence of uniformity on an epsilon-dependent interval.

## Quasi-steady-state reduction

For the dimensionless system

`epsilon*x' = f(x,z,epsilon)`, `z' = g(x,z,epsilon)`,

first solve `f(h0(z),z,0)=0` on the physically relevant branch. Check that the fast subsystem in `tau=t/epsilon`, with z frozen at leading order, attracts that branch. Along the intended trajectory, the fast Jacobian `D_x f` should have eigenvalues with real parts bounded away from zero on the negative side for this standard attracting reduction. A root or invertible Jacobian alone does not establish attraction.

Choose independent fast coordinates after fixing conserved totals or constraints; neutral directions created by redundant coordinates should not be mistaken for a failure of transverse attraction.

The leading slow dynamics are `z'=g(h0(z),z,0)`. Check that the initial fast state is in the attraction basin and whether an initial layer is needed. Preserve conserved totals in choosing coordinates and initial slow data. Off-manifold initial data should not be silently replaced by on-manifold data if the early transient matters.

For a higher-order slow manifold `x=h(z,epsilon)`, use its invariance equation

`epsilon*D_z h(z,epsilon)*g(h(z,epsilon),z,epsilon) = f(h(z,epsilon),z,epsilon)`.

Merely expanding the algebraic equation `f=0` omits the drift correction. If `h=h0+epsilon*h1+...`, then, with all quantities evaluated at `(h0(z),z,0)`,

`D_x f * h1 = D_z h0 * g0 - partial_epsilon f`.

If the manifold depends explicitly on slow time, include `partial_t h` alongside `D_z h*g` in the invariance equation. Fast time dependence in forcing needs its own scale analysis rather than a frozen-time assumption.

Check fast relaxation versus slow variation in the chosen units. Revisit the reduction at folds, eigenvalue crossings, branch changes, or loss of the basin. For unstable or oscillatory fast dynamics, a generic relaxation-to-equilibrium approximation is unjustified; select another analysis.

Primary source for slow-manifold reduction and its asymptotic corrections: [Kaper and Kaper, Asymptotic analysis of two reduction methods for systems of chemical reactions](https://arxiv.org/abs/math/0110159). Use theorem statements only after checking their precise hypotheses.

## Matched asymptotics

When the limiting equation loses conditions, construct outer and inner problems rather than discarding the lost data.

1. Derive the outer balance away from the rapid transition.
2. Obtain the layer thickness `delta(epsilon)` by balancing terms. For a temporal initial layer, use `tau=(t-t0)/delta`; for a spatial boundary layer, stretch distance from the relevant boundary. Do not assume every layer has thickness epsilon.
3. Transform derivatives explicitly, derive the inner problem, and apply the conditions it carries.
4. Express inner and outer asymptotics in a common overlap regime and match coefficients at compatible orders. Endpoint equality alone is insufficient.
5. Construct a composite, when appropriate: `y_comp = y_outer + y_inner - y_overlap`, with all expressions in common variables.
6. Check both original conditions and residuals in the layer and outer regions. At higher orders, subtract the full common expansion at the retained order.

Identify layer location from the differential balance and admissible decaying modes; it need not occur at both boundaries. For a BVP, a stable/unstable direction may select which boundary supplies the outer condition.

For the dimensionless IVP `epsilon*x'=-x`, `x(0)=1`, the outer state is zero and the initial-layer equation with `tau=t/epsilon` has solution `exp(-tau)`. The zero outer state misses an O(1) initial value although it describes the post-transient state. The reconstructed inner expression is exact in this simple case. Check whether a proposed approximation is intended for the layer, the outer regime, or both.

## Multiple scales and averaging

If a regular expansion produces resonant forcing or secular terms, introduce scales justified by the accumulation, such as `T0=t`, `T1=epsilon*t`. Apply the chain rule:

`d/dt = D0 + epsilon*D1 + ...`,

`d^2/dt^2 = D0^2 + 2*epsilon*D0*D1 + ...`.

Allow leading amplitudes and phases to depend on slow time. Derive modulation equations by eliminating resonant forcing or applying an appropriate solvability condition. Map the original initial data into amplitude and phase; check initial derivative conditions at the retained order.

For averaging, transform to suitable amplitude/phase or rotating coordinates and average the slow vector field over the fast phase with slow variables fixed. Near resonance, preserve resonant terms and scale detuning with the perturbation. State the interval supported by the analysis; do not infer arbitrary long-time accuracy from a bounded leading expression.

Check for phase drift as well as amplitude error. A small frequency error can become an O(1) trajectory error after enough cycles. Near a separatrix, vanishing fast frequency, bifurcation, or resonance excluded by the derivation, the assumed scales may fail.

## Poincare-Lindstedt

For a persisting periodic orbit with a weak frequency shift, use `tau=omega(epsilon)*t`, expanding both the periodic profile and omega. Select frequency corrections by removing resonant terms. Fix amplitude and phase conventions to avoid nonunique coefficients, and transform velocity conditions using the time rescaling.

Establish that a periodic orbit is the right target. A freely damped oscillator generally needs amplitude evolution instead of a constant-amplitude periodic ansatz. Forced oscillators require a resonance/detuning analysis appropriate to the forcing.

## Local linearization and dominant balance

If no global small parameter is justified, consider a local approximation near a specified state or equilibrium. For `y=y_star+eta*u` about an equilibrium,

`u'=J*u + O(eta*||u||^2)` under suitable smoothness, with `J=D_y F(y_star)`.

State the small-deviation domain and horizon. Track growth in unstable directions; a locally small initial displacement can leave the linear regime. A zero or nearly zero eigenvalue may require retaining nonlinear terms, a center-manifold analysis, or a new parameter scaling.

For dominant balance, estimate the magnitudes of terms in the proposed regime, retain a consistent balance, and use the candidate solution to check the neglected terms. Change scales near cancellations or transitions. If parameters have no justified hierarchy, report a conditional regime or local series instead of treating an arbitrary truncation as an asymptotic approximation.

## Further sources

- [John K. Hunter, Asymptotic Analysis and Singular Perturbation Theory](https://math.ucdavis.edu/~hunter/notes/asy.pdf): nondimensionalization, order notation, matching, and oscillatory methods. Consult the relevant section for fuller theory rather than treating this guide as a theorem statement.
- [SymPy ODE documentation](https://docs.sympy.org/latest/modules/solvers/ode.html): symbolic solution interfaces and exact solution checks.
- [SymPy series documentation](https://docs.sympy.org/latest/modules/series/series.html): explicit expansion variables and order terms. A computer-generated series has the domain assumptions of the requested expansion.
