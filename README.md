# ODE Analytical Approximations

An agent skill for deriving and validating approximate analytical solutions of ordinary differential equation models.

The workflow connects nondimensionalization, approximation selection, derivation, and validity assessment. It supports scalar equations, coupled systems, initial-value problems, and boundary-value problems.

## Methods

- Regular perturbation and coefficient equations
- Quasi-steady-state reduction with fast stability checks and slow-manifold corrections
- Matched asymptotics for initial and boundary layers
- Multiple scales and averaging for accumulating weak effects
- Poincare-Lindstedt frequency corrections for periodic motion
- Local linearization and dominant balance

The skill checks residuals, initial or boundary conditions, physical constraints, and the domain where the approximation remains useful. It distinguishes formal expansions, numerical evidence, and analytically bounded errors.

## Install

Install directly from GitHub:

```sh
npx skills add John-king-zhou/ode-analytic-approximations --skill ode-analytic-approximations -g -a codex -y
```

From a local checkout, use `npx skills add . --skill ode-analytic-approximations -g -a codex -y`.

Alternatively, copy `skills/ode-analytic-approximations` into your agent's personal skills directory. For Codex, the default location is `~/.codex/skills/ode-analytic-approximations`.

The skill itself requires no additional Python package. When available, the `math-reasoning` and `sympy` skills provide complementary derivation and symbolic-computation support. Numerical validation can use an existing suitable solver.

## Use

```text
Use $ode-analytic-approximations to derive an approximate analytical
solution for this ODE model and assess its validity:

[Equations and initial or boundary conditions]
[Parameter ranges and units]
[Quantity of interest and time interval]
```

If parameter values are unknown, the skill can derive conditional symbolic regimes. It identifies the assumptions required for its approximation.

## Contents

- [SKILL.md](skills/ode-analytic-approximations/SKILL.md): entrypoint and method-selection workflow
- [methods.md](skills/ode-analytic-approximations/references/methods.md): method construction and diagnostic checks
- [validation.md](skills/ode-analytic-approximations/references/validation.md): residuals, error propagation, and numerical comparison
- [openai.yaml](skills/ode-analytic-approximations/agents/openai.yaml): Codex interface metadata

## Validation

The skill passed structural validation and representative behavioral checks:

- A stable coupled fast-slow system with an initial layer and a first-order manifold correction
- A weakly nonlinear oscillator over a time interval proportional to the inverse small parameter
- A repelling fast subsystem where generic quasi-steady-state elimination fails

Checks included exact linear-system identities, symbolic residuals and coefficient equations, and numerical refinement. These examples assess the workflow; each new model still needs its own assumptions and validity analysis.

## Mathematical references

Supporting references link to primary academic material and official SymPy and SciPy documentation. In particular:

- [John K. Hunter, Asymptotic Analysis and Singular Perturbation Theory](https://math.ucdavis.edu/~hunter/notes/asy.pdf)
- [Kaper and Kaper, Asymptotic analysis of two reduction methods for systems of chemical reactions](https://arxiv.org/abs/math/0110159)

The repository contains original procedural guidance and links to those sources; it does not bundle the referenced textbooks or papers.
