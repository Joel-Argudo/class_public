Scalar-tensor CLASS 
==============================================

Author: Joel Argudo Panes

Advisor: Adrià Gómez-Valent

This code was developed as part of my master's thesis (TFM) at the University of Barcelona. 

Model
--------------------

CLASS code modification that introduces a non-minimal coupling between the scalar field and the Ricci scalar of the form $F(\varphi)R$, with $F(\varphi) = \frac{1+\alpha\varphi^2}{2\kappa^2}$. The scalar field has a canonical kinetic term and a quadratic potential $V = V_0 + \beta \varphi + 0.5 m^2 \varphi^2$.

Units
--------------------

Dimensionless variables:

$$\bar{V}_0 \equiv \frac{V_0}{H_0^2 \bar{M}_p^2}, \quad \bar{\varphi} \equiv \frac{\varphi}{\bar{M}_p}, \quad\bar{\kappa} \equiv \frac{\kappa}{\kappa_N}, \quad\bar{\alpha} \equiv \frac{\alpha}{\kappa_N^2}, \quad\bar{\beta} \equiv \frac{\beta}{H_0^2 \bar{M}_p}, \quad\bar{m} \equiv \frac{m}{H_0},$$

where $\bar{M}_p^2 = 1/(8\pi G_N) \equiv 1/\kappa_N^2$.

CLASS units:

$$\varphi_c = \frac{\bar{\varphi}}{\sqrt{3}}, \quad \kappa^2_c = \bar{\kappa}^2, \quad \alpha_c = 3\bar{\alpha}, \quad V_{0}^c = \frac{H_0^2}{3}\bar{V}_0, \quad \beta_c = \frac{H_0^2}{\sqrt{3}}\bar{\beta},  \quad m_c = H_0\bar{m}.$$

Code modifications
--------------------

- In `include/background.h`: Defined the modified gravity parameters $\alpha$ and $\kappa^2$ as `scf_alpha` and `scf_kappa2`.
- In `source/input.c`: Added the reading and default values (0.0, 1.0) of `scf_alpha` and `scf_kappa2`.
- In `source/input.c`: Turned the shooting parameters `xguess` and `dxdy`, used to tune $V_0$, into input parameters.
- In `source/input.c`: Modified the values of `x1` and `dxdy`, used in the shooting algorithm that tunes $V_0$.
- In `source/background.c`: Modified the equations that compute $H$ and $H'$ inside `background_functions()`.
- In `source/background.c`: Modified the scalar field background equation for $\bar{\varphi}''$ inside `background_derivs()`.
- In `source/background.c`: Modified the DE effective pressure and energy density.
- In `source/background.c`: Defined the scalar field potential and its derivatives.
- In `source/background.c`: Modified the computation of the time derivative of the DE effective pressure inside `solve_background()`.
- In `source/background.c`: Removed scalar field contribution to rho_m and rho_r.
- In `source/perturbations.c`: Modified the equations that compute $h'$, $\eta'$, $h''$ and $\tilde{\alpha}'$ (inside `perturbations_einstein()`), and $\delta\varphi''$ (inside `perturbations_derivs()`). $\tilde{\alpha}$ is defined as $\tilde{\alpha} \equiv (h'+\eta')/(2k^2)$.
- In `source/perturbations.c`: Eliminated the DE contributions to fluid perturbations ($\delta\rho$, $\delta p$, etc.).

Limitations
--------------------
So far, this code only supports a flat Universe, scalar modes and the synchronous gauge. Additionally, the shooting method may fail to converge in specific regions of parameter space depending on the values of `xguess` (the initial guess for $V_0$) and `dxdy`. Finally, we recommend setting $a_{ini} \sim 10^{-11}$ as the background integrator may crash for smaller values.
