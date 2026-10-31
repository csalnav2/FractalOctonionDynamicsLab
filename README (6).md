# AI-Based Fractal Octonion Labs

**AIbasedFractalOctonionlabs**  
**Created by Christopher Salnave**

### Exploring nonlinear dynamics through higher-dimensional geometry

AI-Based Fractal Octonion Labs is an interactive computational research dashboard for examining nonlinear trajectories through eight-component representations, quaternionic Hopf geometry, spectral analysis, curvature diagnostics, and evolving point-cloud connectivity. Optional AI assistance supports explanation, experiment design, and interpretation of recorded results.

The central research question is whether changing the representation of a dynamical system makes useful structure easier to measure: transitions between dynamical regimes, changes in observation sensitivity, shifts in spectral content, and differences in the connectivity of sampled trajectories.

The framework follows a defined mathematical sequence: source equations generate states; state and rate features produce a normalized point on the seven-sphere; a quaternionic Hopf map produces a point on the four-sphere; numerical diagnostics compare the resulting representations. Separate experiments can feed a bounded geometry-dependent intervention back into the original source equations.

This repository is a **public research preview** containing explanations and visual outputs. The application implementation and private notebooks are not distributed here.

## Contents

1. [Research purpose](#1-research-purpose)
2. [Visual previews](#2-visual-previews)
3. [The mathematical pipeline](#3-the-mathematical-pipeline)
4. [Source dynamical systems](#4-source-dynamical-systems)
5. [Integration, sampling, and time](#5-integration-sampling-and-time)
6. [Eight-feature encoding and octonion representation](#6-eight-feature-encoding-and-octonion-representation)
7. [The quaternionic Hopf map](#7-the-quaternionic-hopf-map)
8. [What the fiber animation represents](#8-what-the-fiber-animation-represents)
9. [Base-point oscillations on the four-sphere](#9-base-point-oscillations-on-the-four-sphere)
10. [Observation sensitivity and the matrix plots](#10-observation-sensitivity-and-the-matrix-plots)
11. [Fourier analysis and power spectra](#11-fourier-analysis-and-power-spectra)
12. [Connectivity, Rips, Čech, and the graph Laplacian](#12-connectivity-rips-čech-and-the-graph-laplacian)
13. [The declared curvature model](#13-the-declared-curvature-model)
14. [Curvature-dependent interventions and controls](#14-curvature-dependent-interventions-and-controls)
15. [How the diagnostics work together](#15-how-the-diagnostics-work-together)
16. [Validation and evidence](#16-validation-and-evidence)
17. [A reproducible research protocol](#17-a-reproducible-research-protocol)
18. [Research roadmap](#18-research-roadmap)
19. [AI assistance and project scope](#19-ai-assistance-and-project-scope)
20. [References and preview organization](#20-references-and-preview-organization)

## 1. Research purpose

A dynamical system can display different patterns depending on what is observed. A trajectory in its original state space, a normalized feature cloud, and a nonlinear observation of that cloud are related representations with different sensitivities and information losses.

The laboratory studies these relationships through several questions:

- Which source-state changes produce large or small movements in the geometric observations?
- Which perturbations are visible to the Hopf map, and which directions lie along a fiber?
- How do the spectra of geometric observables change with source parameters or a declared intervention?
- How does the connection threshold of a finite point cloud change across representations and time windows?
- Can a geometric diagnostic improve a specific inference or forecasting task beyond a source-space baseline?

“New information” is a research objective about useful interpretation or inference. A deterministic encoding does not add independent measurements to its input. Its potential value is that it can expose relationships in a form that is easier to analyze, compare, or use in a defined task.

A meaningful result would therefore include a measurable effect, a comparison condition, numerical checks, and evidence that the result persists beyond a selected animation.

## 2. Visual previews

The following clips are generated from the numerical engine and geometric constructions described below. They are rendered visualizations, rather than recordings of the complete dashboard interface. The accompanying source-dynamics and geometric clips use **unforced Lorenz and Rössler baselines**.

| Preview | What is displayed | GIF | MP4 |
|---|---|---|---|
| Lorenz dynamics | Three-dimensional state trajectory with synchronized coordinate traces | [View GIF](media/Lorenz_Dynamics.gif) | [Play MP4](media/Lorenz_Dynamics.mp4) |
| Rössler dynamics | Spiral-and-excursion geometry with synchronized coordinate traces | [View GIF](media/Rossler_Dynamics.gif) | [Play MP4](media/Rossler_Dynamics.mp4) |
| Hopf fibers | Three generator-circle sections of the fiber through each evolving source point | [View GIF](media/Hopf_Fibers.gif) | [Play MP4](media/Hopf_Fibers.mp4) |
| Four-sphere base points | Three-coordinate projections and all five base-coordinate signals for both sources | [View GIF](media/S4_Basepoint_Oscillations.gif) | [Play MP4](media/S4_Basepoint_Oscillations.mp4) |

![Generator-circle sections of the quaternionic Hopf fibers](media/Hopf_Fibers.gif)

![Five-coordinate oscillations of the Hopf base points](media/S4_Basepoint_Oscillations.gif)

Each clip lasts 16 seconds. The MP4s use 1280 × 960 resolution at 30 frames per second; the GIFs use 896 × 672 resolution at 12 frames per second and loop automatically. Playback is accelerated, and the displayed time window restarts at each loop. A loop does not imply that the underlying chaotic trajectory is periodic.

The fiber-angle markers are geometric constructions. They are distinct from the recorded evolution of a source state in model time.

## 3. The mathematical pipeline

| Stage | Mathematical object | Role |
|---|---|---|
| Source dynamics | $X(t)\in\mathbb R^3$ | Generates the original trajectory |
| Feature construction | $v(X)\in\mathbb R^8$ | Combines scaled position, rate, and magnitude information |
| Sphere normalization | $q=v/\|v\|\in S^7$ | Retains feature direction and separates its overall magnitude |
| Hopf observation | $h=H(q)\in S^4\subset\mathbb R^5$ | Identifies points related by a quaternionic fiber action |
| Differential diagnostics | $dH$, $J^{\mathsf T}J$, tangent projectors | Measures locally visible and fiber-tangent directions |
| Spectral diagnostics | Coordinate traces, FFTs, power densities | Measures temporal structure of chosen observables |
| Cloud diagnostics | Distances, graphs, $\beta_0$, $\varepsilon_*$ | Measures finite-sample connectivity |
| Curvature experiment | A declared graph metric and bounded source intervention | Tests a specific geometry-to-dynamics feedback mechanism |

The representation has several layers. A feature cloud in eight ambient coordinates, a point on a seven-dimensional sphere, and a point on a four-dimensional sphere are different objects. Their dimension labels should not be used interchangeably.

## 4. Source dynamical systems

### 4.1 Lorenz system

For $X=(x,y,z)$, the Lorenz vector field is

$$
\dot x=\sigma(y-x),\qquad
\dot y=x(\rho_L-z)-y,\qquad
\dot z=xy-\beta z.
$$

The default parameters are $\sigma=10$, $\rho_L=28$, and $\beta=8/3$, with initial state $(1,1,1)$. The source model is based on the classic Lorenz system [1].

Its equilibria are obtained by setting the three derivatives to zero:

$$
C_0=(0,0,0),\qquad
C_{\pm}=\left(\pm\sqrt{\beta(\rho_L-1)},\,
\pm\sqrt{\beta(\rho_L-1)},\,\rho_L-1\right).
$$

At the default parameters, $C_{\pm}=(\pm\sqrt{72},\pm\sqrt{72},27)$. These are equilibrium locations, not the moving trajectory itself. The familiar two lobes are regions of one double-scroll trajectory; their appearance alone does not demonstrate two independent attractors.

The source-flow Jacobian is

$$
DF_L(X)=
\begin{pmatrix}
-\sigma&\sigma&0\\
\rho_L-z&-1&-x\\
y&x&-\beta
\end{pmatrix}.
$$

This matrix describes infinitesimal source-state evolution. It is different from the eight-by-eight observation matrices introduced later.

### 4.2 Rössler system

The Rössler vector field is

$$
\dot x=-y-z,\qquad
\dot y=x+ay,\qquad
\dot z=b+z(x-c).
$$

The implemented default is $a=b=0.2$, $c=5.7$, with initial state $(1,1,1)$. These are the laboratory's selected parameters for the system introduced by Rössler [2].

The source Jacobian is

$$
DF_R(X)=
\begin{pmatrix}
0&-1&-1\\
1&a&0\\
z&0&x-c
\end{pmatrix}.
$$

The trajectory combines approximately rotational motion with nonlinear excursions in the third coordinate. The coordinate traces help identify when a visually narrow or broad section of the three-dimensional path corresponds to a change in the actual recorded values.

### 4.3 Additional source presets

The broader dashboard includes Thomas flow, the Hénon map, logistic delay coordinates, and a Mandelbrot-orbit preset. Continuous flows and discrete maps have different time conventions. An escaping Mandelbrot orbit is a finite orbit segment, not automatically an attractor. Classification depends on the equations, parameter values, initial state, and duration being examined.

## 5. Integration, sampling, and time

Continuous sources use fixed-step fourth-order Runge–Kutta integration. If $\delta$ is one integration substep,

$$
\begin{aligned}
k_1&=F(X_n),\\
k_2&=F(X_n+\delta k_1/2),\\
k_3&=F(X_n+\delta k_2/2),\\
k_4&=F(X_n+\delta k_3),\\
X_{n+1}&=X_n+\frac{\delta}{6}(k_1+2k_2+2k_3+k_4).
\end{aligned}
$$

Two substeps are taken for each native recorded sample in the default flow presets.

| Source | Native sample interval $\Delta t$ | RK4 substep $\delta$ |
|---|---:|---:|
| Lorenz | 0.01 | 0.005 |
| Rössler | 0.02 | 0.01 |
| Thomas | 0.05 | 0.025 |

Three clocks must remain separate: numerical model time, the interval between recorded samples, and animation playback time. Camera rotation and frame rate change presentation, not the native trajectory or spectral sampling rate.

The preset coordinates and time are dimensionless model quantities unless an explicit calibration is supplied. They are not measured positions in meters or laboratory time in seconds. There is no spatial-grid CFL condition for these autonomous ODE integrations; accuracy and numerical behavior must instead be checked by time-step refinement and relevant diagnostics.

For a smooth bounded solution, RK4 has fourth-order global accuracy over a fixed finite interval. Chaotic sensitivity means that long-time pointwise agreement between two resolutions can be lost even when both approximate the dynamics well. Short-time convergence and longer-time statistical comparisons answer different questions.

## 6. Eight-feature encoding and octonion representation

### 6.1 Scaled state and rate

Let $s_j$ be fixed coordinate scales, $c_j$ fixed centers, and $R_0$ a rate scale. For an unforced flow,

$$
u_j=\frac{X_j-c_j}{s_j},\qquad
r_j=\frac{F_j(X)}{s_jR_0}.
$$

Here $u_j$ denotes the scaled position coordinate; $r_j$ is the scaled source-rate coordinate. For discrete maps, a per-iteration difference replaces the continuous vector field in the rate feature.

| Source | Coordinate scales $(s_0,s_1,s_2)$ | Rate scale $R_0$ |
|---|---|---:|
| Lorenz | $(20,30,50)$ | 10 |
| Rössler | $(12,12,25)$ | 1 |
| Thomas | $(4,4,4)$ | 0.3 |

The default centers are zero. Scaling is part of the experiment: changing it changes the geometry of the encoded cloud.

### 6.2 Feature vector and sphere normalization

The implemented feature vector is

$$
v=(u_0,u_1,u_2,r_0,r_1,r_2,\|u\|,\|r\|).
$$

Define its magnitude $\rho_v=\|v\|$. For a nonzero finite feature vector,

$$
q=\frac{v}{\rho_v},\qquad \|q\|=1,
\qquad q\in S^7\subset\mathbb R^8.
$$

The magnitude satisfies

$$
\rho_v^2=2\bigl(\|u\|^2+\|r\|^2\bigr).
$$

Normalization keeps the direction of the feature vector. The raw magnitude is a separate observable and should be retained when comparing runs. A zero feature vector has no defined direction on the sphere; the numerical workflow rejects singular or nonfinite encodings.

### 6.3 Why the octonion language is relevant

An eight-component real vector can be expressed in an octonion basis:

$$
q=q_0+q_1e_1+\cdots+q_7e_7.
$$

This provides an algebraic representation of the data and connects the project to normed division-algebra geometry [3]. The present source trajectory is generated by the stated source equations. Writing its features in an octonion basis does not, by itself, introduce an octonion multiplication law into the source evolution.

The eight features are also not eight independent inputs. The final two are magnitudes of the preceding groups; for a fixed autonomous ODE, the rate is already determined by the state. The image is therefore a structured subset of the ambient sphere. Increasing the number of representation coordinates does not increase the number of independent source-state variables.

## 7. The quaternionic Hopf map

### 7.1 Splitting the eight coordinates

Write the unit vector as a pair of scalar-first quaternions,

$$
a=(q_0,q_1,q_2,q_3),\qquad
b=(q_4,q_5,q_6,q_7),\qquad |a|^2+|b|^2=1.
$$

The map used by the laboratory is

$$
H(a,b)=\left(2a\overline b,\ |a|^2-|b|^2\right)=h.
$$

The quaternion component contributes four real coordinates and the final scalar contributes a fifth. Thus $h$ is represented in $\mathbb R^5$.

### 7.2 Why the output lies on the four-sphere

Using quaternion norm multiplicativity,

$$
\begin{aligned}
\|H(a,b)\|^2
&=4|a\overline b|^2+(|a|^2-|b|^2)^2\\
&=4|a|^2|b|^2+(|a|^2-|b|^2)^2\\
&=(|a|^2+|b|^2)^2=1.
\end{aligned}
$$

Therefore

$$
H:S^7\longrightarrow S^4\subset\mathbb R^5.
$$

The four-sphere has intrinsic dimension four and requires five Cartesian coordinates in this embedding. A single time sample gives one base point $h(t)$ with five coordinates.

### 7.3 The quotient and its fiber

For a unit quaternion $r$, consider the simultaneous right action

$$
(a,b)\mapsto(ar,br).
$$

Quaternion multiplication is associative, so

$$
(ar)\overline{(br)}=ar\overline r\,\overline b=a\overline b.
$$

The quaternion norms are unchanged as well. Hence $H(ar,br)=H(a,b)$. The right-action orbit is an $S^3$ fiber, yielding the quaternionic Hopf fibration

$$
S^3\hookrightarrow S^7\xrightarrow{\ H\ }S^4.
$$

The base identifies all points on the same fiber. This is a quotient construction; it does not set a quaternion coordinate equal to one or recover the original point uniquely from the base [3]. The distinct octonionic Hopf fibration has total space $S^{15}$ and base $S^8$.

### 7.4 An encoder-specific restriction

For the particular feature ordering in this laboratory,

$$
h_4=-\frac{r_1^2+r_2^2}{\|u\|^2+\|r\|^2}\le0.
$$

The encoded source trajectory therefore occupies a restricted part of the four-sphere. A nonpositive $h_4$ in the ordinary encoded trajectory is expected. This restriction comes from the encoder and should not be interpreted as a universal property of Hopf geometry. A difference signal $\Delta h_4$ can have either sign.

## 8. What the fiber animation represents

For each quaternionic imaginary basis direction $e_k$, a unit quaternion circle is

$$
r_\theta=\cos\theta+e_k\sin\theta,\qquad k=1,2,3.
$$

Applying this action to a recorded anchor gives

$$
q_\theta=(a r_\theta,b r_\theta),\qquad H(q_\theta)=H(q).
$$

The animation draws three such great-circle sections through the evolving anchor. All three belong to its associated three-dimensional fiber. The full $S^3$ fiber cannot be represented by three circles alone.

The presentation combines two different changes: the anchor moves because the source evolves, while the colored markers move around geometric sections as their angle parameter changes. At a fixed anchor, motion along these sections leaves the base point constant. As the anchor changes with source time, the associated base point generally changes.

The displayed curves are fixed linear projections from eight ambient coordinates to three, followed by a camera view. A projected crossing does not imply an intersection in the original sphere. The section parameter is geometric angle or unit-sphere arclength; it is not an inferred hidden clock.

## 9. Base-point oscillations on the four-sphere

The encoded trajectory produces

$$
h(t)=\bigl(h_0(t),h_1(t),h_2(t),h_3(t),h_4(t)\bigr),
\qquad \sum_{k=0}^{4}h_k(t)^2=1.
$$

The preview shows a three-coordinate projection alongside all five coordinate traces. The projection makes the motion visible, while the traces preserve access to each measured coordinate value. A narrow projected path can coexist with substantial variation in coordinates omitted from that projection.

Useful quantities include coordinate ranges, correlations, turning points, spectral power, and differences between conditions. Where the source path and encoder are differentiable, differentiating the sphere constraint gives

$$
h^{\mathsf T}\dot h=0.
$$

The instantaneous velocity is tangent to the four-sphere. By the chain rule, the unforced path obeys

$$
\dot h=DH(q)\,D_Xq\,F(X).
$$

This separates the roles of the source dynamics, the feature encoder, and the Hopf observation. A changing base coordinate reflects their composition, rather than a separate five-variable source system.

## 10. Observation sensitivity and the matrix plots

### 10.1 Tangent and observation operators

At a unit point $q$, the ambient tangent projector is

$$
T_q=I_8-qq^{\mathsf T}.
$$

For a perturbation $(\delta a,\delta b)$, the differential of the Hopf map is

$$
DH_{(a,b)}(\delta a,\delta b)=
\left(
2\delta a\,\overline b+2a\,\overline{\delta b},\;
2\langle a,\delta a\rangle-2\langle b,\delta b\rangle
\right).
$$

Set $J=DH_qT_q$, a five-by-eight matrix. The diagnostic Gram matrix is

$$
G=J^{\mathsf T}J.
$$

For the canonical map between unit round spheres, its visible and fiber-tangent projectors are

$$
P_{\mathrm{vis}}=\frac14G,\qquad
P_{\mathrm{fib}}=T_q-P_{\mathrm{vis}}.
$$

They obey

$$
\begin{gathered}
P_{\mathrm{vis}}^2=P_{\mathrm{vis}},\quad
P_{\mathrm{fib}}^2=P_{\mathrm{fib}},\quad
P_{\mathrm{vis}}P_{\mathrm{fib}}=0,\\
\operatorname{rank}(P_{\mathrm{vis}})=4,\quad
\operatorname{rank}(P_{\mathrm{fib}})=3,\quad
JP_{\mathrm{fib}}=0.
\end{gathered}
$$

The factor of four follows from the squared dilation of this unit-sphere map on its horizontal tangent space. These identities use the stated metric convention; they should not be transferred unchanged to a different observation map.

### 10.2 Interpreting the eight-by-eight surfaces

A matrix-surface height is a matrix entry. The two horizontal indices label components in the ambient eight-coordinate basis. As the anchor moves, the local observation sensitivity changes and so does the surface.

“Hidden” refers to a direction invisible to this particular observation differential. It does not mean an additional physical dimension has been measured or that the original source can freely move in all three fiber directions.

Where defined, $D_Xq$ has at most rank three for this three-state autonomous source. The composite source-to-base differential is a five-by-three matrix. The larger tangent spaces of the ambient spheres describe geometric possibilities beyond the subset reachable by the chosen encoder.

## 11. Fourier analysis and power spectra

For one coordinate signal $x_j$ sampled at uniform interval $\Delta t$, the spectral workflow removes its mean and applies a periodic Hann window:

$$
\overline x=\frac1N\sum_{j=0}^{N-1}x_j,\qquad
w_j=\frac12-\frac12\cos\left(\frac{2\pi j}{N}\right).
$$

The discrete transform is

$$
X_k=\sum_{j=0}^{N-1}w_j(x_j-\overline x)
\exp\left(-\frac{2\pi i jk}{N}\right).
$$

The one-sided power spectral density is

$$
P_k=c_k\,\frac{\Delta t\,|X_k|^2}{\sum_jw_j^2},
$$

where $c_k=1$ at DC and the Nyquist bin and $c_k=2$ at interior positive-frequency bins. This density convention and its distinction from squared-magnitude spectrum scaling are documented in the SciPy reference [4].

The sampling quantities are

$$
f_s=\frac1{\Delta t},\quad
f_{\mathrm{Nyquist}}=\frac1{2\Delta t},\quad
f_k=\frac{k}{N\Delta t},\quad
\Delta f=\frac1{N\Delta t}.
$$

| Example | $\Delta t$ | $N$ | DFT period | Nyquist | Frequency spacing |
|---|---:|---:|---:|---:|---:|
| Lorenz main-window default | 0.01 | 512 | 5.12 | 50 | 0.1953125 |
| Lorenz direct/paired-window default | 0.01 | 1024 | 10.24 | 50 | 0.09765625 |
| Rössler example at the same window length | 0.02 | 512 | 10.24 | 25 | 0.09765625 |

Periods are in model-time units; frequencies are cycles per model-time unit. Increasing $N$ improves bin spacing but does not increase the Nyquist frequency.

The normalization has a useful verification identity:

$$
\sum_kP_k\,\Delta f=
\frac{\sum_jw_j^2(x_j-\overline x)^2}{\sum_jw_j^2}.
$$

This is the window-weighted centered energy. It need not equal the ordinary unweighted variance. A power spectrum also discards Fourier phase: different time series can share a spectrum.

### Actual signals, paired responses, and display localization

The current dashboard distinguishes actual coordinates $h$, matched unforced coordinates $h_{\mathrm{base}}$, and the paired response $\Delta h=h-h_{\mathrm{base}}$. The spectrum of the difference is generally different from the difference of the spectra.

The fix18g display selects actual coordinate spectra independently of the paired-response cards. Its focused frequency view retains at least 99.5% of each current nonzero displayed channel's calculated power, including matched baseline channels when shown. This is a display framing rule, not a filter or evidence of reduced dynamical bandwidth. The 40 dB heatmap contrast is a separate visibility setting.

The calculations use contiguous native samples. Display interpolation, camera motion, missing-value placeholders, and GIF frames are not additional spectral observations. A finite FFT does not by itself establish alias-free sampling or a unique dynamical fingerprint.

## 12. Connectivity, Rips, Čech, and the graph Laplacian

### 12.1 Distances must be declared

The recorded dashboard cloud diagnostics use the following normalized distances:

| Cloud | Distance |
|---|---|
| Encoded sphere points | $d_q(q_i,q_j)=\|q_i-q_j\|/2$ |
| Hopf base points | $d_h(h_i,h_j)=\|h_i-h_j\|/2$ |
| Hidden tangent projectors | $d_P(P_i,P_j)=\|P_i-P_j\|_F/\sqrt6$ |

These are ambient chord or projector distances. They are distinct from distances measured in a three-dimensional screen projection and from geodesic distances in the separate curvature model.

The earlier vertical Instagram teaser used an explicitly labeled **unnormalized** $S^7$ chord threshold of 0.15 for its illustrative graph. That corresponds to 0.075 under the normalized $d_q$ convention above; the threshold numbers should not be compared without the scale conversion.

### 12.2 Connected components and the critical threshold

Connect nodes when $d(i,j)\le\varepsilon$. The number of connected components is $\beta_0(\varepsilon)$. If a complete finite distance graph has $n$ vertices and minimum-spanning-tree edge lengths $d_1\le\cdots\le d_{n-1}$,

$$
\beta_0(\varepsilon)=n-\#\{d_j\le\varepsilon\},\qquad
\varepsilon_*=\max_j d_j.
$$

The value $\varepsilon_*$ is the smallest inclusive threshold connecting the cloud. For one point or an all-coincident cloud, it is zero. An empty cloud is not assigned a meaningful threshold.

Two visibly different clouds can both have $\beta_0=1$ at a generous threshold. Comparing the full threshold curve or $\varepsilon_*$ can reveal differences that a single component count hides.

### 12.3 What Rips and Čech add

A Vietoris–Rips complex extends the threshold graph by filling cliques. A Čech complex is built from common intersections of metric balls. Under the ambient Euclidean ball convention used here, balls of radius $\varepsilon/2$ have the same edge-intersection rule as the threshold graph at $\varepsilon$. Their connected-component counts therefore agree.

That statement concerns $H_0$ connectivity. It does not establish agreement of all higher homology groups. A connected graph can retain cycles, and a graph cycle can be filled by a higher-dimensional simplex. In particular, the graph cycle rank $E-n+\beta_0$ is not automatically the first Betti number of a filled simplicial complex.

### 12.4 The zero-order graph Hodge operator

For an oriented vertex-edge incidence matrix $B_1$,

$$
L_0=B_1B_1^{\mathsf T},\qquad
(L_0f)_i=\sum_{j\sim i}(f_i-f_j).
$$

Its nullity equals the number of connected components for this unweighted finite graph. It describes how a vertex signal varies across the selected edges. It is a graph operator, rather than a measurement of the intrinsic curvature of the sphere.

A rolling-window graph can reconnect or disconnect as samples enter and leave. Time to first connection at fixed $\varepsilon$ is a different measurement from the threshold $\varepsilon_*(t)$.

## 13. The declared curvature model

The variable-curvature experiment constructs a four-dimensional graph in Euclidean five-space:

$$
\Phi(\xi)=(\xi,f(\xi)),\qquad
f(\xi)=\tfrac12\xi^{\mathsf T}A\xi+w\sin(b^{\mathsf T}\xi),
\qquad \xi=1.6(q_0,q_1,q_2,q_3).
$$

Here $A$, $w$, and $b$ define a chosen model. Its induced metric is

$$
g=I_4+pp^{\mathsf T},\qquad p=\nabla f.
$$

The eigenvalues are $1,1,1,1+\|p\|^2$, so this metric is positive definite. The second fundamental form and its orthonormal shape representation are

$$
\mathrm{II}=\frac{D^2f}{\sqrt{1+\|p\|^2}},\qquad
C=g^{-1/2}\mathrm{II}\,g^{-1/2}.
$$

In an orthonormal frame the Gauss equation gives

$$
\mathcal R_{ijkl}=C_{ik}C_{jl}-C_{il}C_{jk},\qquad
\mathrm{Ric}=(\operatorname{tr}C)C-C^2,\qquad
R=(\operatorname{tr}C)^2-\operatorname{tr}(C^2).
$$

In four dimensions, the trace-free Ricci part is $\mathrm{Ric}_0=\mathrm{Ric}-(R/4)g$. The Weyl tensor is the remaining trace-free curvature tensor after removing the Ricci and scalar contributions. These objects are derived from one consistent metric; their entries are not independent knobs.

This distinction matters when interpreting isotropic and anisotropic changes. Adding a scalar multiple of the identity to $A$ changes the graph model, but it does not independently prescribe an isotropic Ricci tensor. The metric, inverse metric, shape operator, and curvature must all be recomputed.

The graph model is separate from the unit round spheres used by the Hopf construction. Unit round $S^4$ has $\mathrm{Ric}=3g$, scalar curvature 12, and zero Weyl tensor. Variable Weyl curvature in the graph model does not mean that the intrinsic round-sphere metric has changed. The declared metric is Riemannian and is not a measured physical spacetime metric.

## 14. Curvature-dependent interventions and controls

### 14.1 The causal feedback path

With coupling disabled, changing the graph metric changes curvature readouts without moving the source or its encoded cloud. Coupling introduces an explicit route back to the source dynamics.

For a selected scalar curvature objective $V(q)$, a schematic form of the implemented source action is

$$
u_{\mathrm{request}}=
B_{\mathrm{src}}^{\mathsf T}\bigl(-\eta P_{\mathrm{route}}\nabla_{S^7}V\bigr),
\qquad
B_{\mathrm{src}}=\frac{\partial q}{\partial u_{\mathrm{source}}},
$$

followed by a norm cap. Here $u_{\mathrm{source}}$ denotes scaled source-position coordinates. It is distinct from the resulting actuator input $u_{\mathrm{request}}$. The actual source evolves as

$$
\dot X=F(X)+\operatorname{diag}(s_0,s_1,s_2)u.
$$

Thus the dependency is: declared metric, curvature gradient, bounded input, changed source trajectory, changed features, changed Hopf observations, changed cloud distances.

The default coupled Ricci objective is $V=\tanh(R/4)$. For the supplied fix18g dashboard, the inherited startup configuration requests a decrease with gain 0.1 and normalized input norm cap 0.005, using both tangent routes and holding each input for eight native intervals. This is separate from the unforced media previews.

A requested local decrease need not produce a monotonic observed curvature trace. Native dynamics, the three-state source's reachable directions, input saturation, and sample timing can all influence the result. In controlled runs the rate feature convention still evaluates the original source field at the controlled state; the added actuator is recorded separately.

### 14.2 Targets, gain tuning, and stability diagnostics

The broader control workflow supports specified target programs and bounded source actions. A local damped least-squares request takes the form

$$
u_{\mathrm{request}}=(J_s^{\mathsf T}J_s+\lambda I)^{-1}J_s^{\mathsf T}e,
$$

where $e$ is the selected output error and $J_s$ its source-coordinate sensitivity. The resulting action is bounded rather than imposed directly on the displayed tensor entries.

An SPSA workflow can adjust four bounded route gains using paired perturbation evaluations and finite-horizon costs. This is an optimization experiment on the available controls, rather than a proof of a globally optimal policy or an inverse reconstruction of the fiber.

Conditional growth compares nearby trajectories under the same realized forcing history. A full closed-loop growth calculation would also account for how the controller and its memory change under perturbation. These quantities answer different stability questions. Large actual-minus-baseline separation, by itself, does not establish stabilization or a change in attractor dimension.

## 15. How the diagnostics work together

| Observed change | Useful interpretation | Additional comparison |
|---|---|---|
| Source path changes | A change in the generated trajectory | Match initial state, parameters, input and integration grid |
| Base path changes | A change visible to the Hopf observation | Compare the source and encoded sphere paths |
| Matrix entries change | A change in local observation sensitivity | Check projector identities and the source-reachable tangent subset |
| Spectral power shifts | A change in the selected observable over the stated window | Match sampling, window length, normalization and baseline |
| Connection threshold changes | A change in finite-cloud geometry under a declared distance | Match point count, sample IDs, stride and threshold convention |
| Curvature changes in readout-only mode | A change in where or how the graph metric is evaluated | Check whether any source intervention actually occurred |
| Conditional growth changes | A change in finite-time divergence under the realized forcing | Compare durations, perturbation scales and a closed-loop diagnostic when needed |

The strongest interpretation uses several diagnostics with matched provenance. A visually dramatic curve is a starting point for an experiment, while agreement across controlled measurements provides stronger evidence.

## 16. Validation and evidence

### 16.1 Checks completed for the shared work

The two original uploaded launch notebooks were byte-identical. Their embedded archive contained 306 editable source files, each of which matched its declared SHA-256 checksum before documentation changes.

Five focused regression scripts passed during the initial packaging work: localized FFT display, dashboard FFT, dashboard curvature integration, active-curvature defaults, and dynamic FFT history. The extracted Python sources also passed syntax parsing. These results apply to the checked revision and scope; they are not a claim that every historical test or external service was exercised.

For the four later media previews, the numerical engine and its fiber core were used directly. Checks across the exported trajectories and sampled fiber sections gave the following maximum absolute residuals:

| Check | Maximum residual |
|---|---:|
| $\lvert\lVert q\rVert-1\rvert$ | $2.22\times10^{-16}$ |
| $\lvert\lVert H(q)\rVert-1\rvert$ | $6.66\times10^{-16}$ |
| $\max_k\lvert H(q_\theta)_k-H(q)_k\rvert$ | $5.55\times10^{-16}$ |
| $\lvert\lVert q_\theta\rVert-1\rvert$ | $4.44\times10^{-16}$ |

These are finite numerical checks at floating-point precision, not exact bounds for all possible inputs. Media duration, resolution, frame counts, GIF looping, and representative beginning/middle/end frames were also checked.

### 16.2 Evidence still needed for broader research claims

A claim that the geometric representation improves prediction or detection needs an independently defined task and a matched baseline. A claim about attractor dimension needs a dedicated dimension estimate with scaling and finite-data checks. A claim about physical matter or spacetime needs a physical model and experimental evidence beyond these mathematical trajectories.

Public previews communicate the numerical construction and its behavior. Full independent reproduction of the complete private dashboard would additionally require access to its implementation and exact experiment configurations. Live microphone, tunneling, provider services, and full browser behavior were not revalidated while expanding this README.

## 17. A reproducible research protocol

1. **Fix the scientific question.** Choose a primary outcome, such as a change in $\varepsilon_*$, a band-power statistic, or performance on a held-out inference task.
2. **Specify the source.** Record equations, parameters, initial conditions, model-time convention, integration substep and native sample interval.
3. **Specify the representation.** Record centers, scales, rate definition, feature order, magnitude handling, quaternion ordering and observation map.
4. **Choose a settling interval.** Exclude or report initial transients according to a declared rule.
5. **Create the matched baseline.** Use identical initialization and sampling with zero intervention. Verify that a zero-action controlled run agrees with it.
6. **Declare analysis windows.** Match point counts, source sample IDs, strides, FFT sizes, frequency ranges and distance normalizations.
7. **Apply one specified intervention.** Record its objective, onset, bounds, input history, requested targets and realized outputs.
8. **Check invariants and numerical resolution.** Monitor sphere norms, fiber invariance and projector residuals; repeat relevant short-time and statistical comparisons at smaller integration steps.
9. **Compare across conditions.** Use multiple initial states, durations and parameter choices. For learned or tuned methods, reserve held-out trajectories.
10. **Preserve provenance.** Retain native samples and metadata rather than measuring values from compressed animations.

For a finite cloud, also record the selected threshold and its scale. For time-to-connectivity, define the time origin, evaluation interval, complete-window rule, and whether connectivity must persist for a specified duration.

For spectra, distinguish actual, baseline and response signals explicitly. For control, distinguish a requested waveform from an achieved output and a finite-horizon improvement from longer-term behavior.

## 18. Research roadmap

| Research direction | Concrete next experiment | Evidence to seek |
|---|---|---|
| Encoding sensitivity | Vary scale choices and remove one feature group at a time | Which signatures persist under reasonable representation changes |
| Cross-representation analysis | Compare source space, normalized features and Hopf base with matched windows | Whether a geometric diagnostic adds value to a declared task |
| Spectral structure | Compare coordinate spectra, phase-sensitive measures and surrogate signals | Which effects depend on phase or temporal ordering |
| Connectivity dynamics | Sweep threshold, window duration and stride | Stable geometric differences across finite-sample choices |
| Curvature feedback | Compare geometry-only, coupled and zero-action cases | Whether changes follow the explicit intervention path |
| Reachability | Compare desired tangent actions with the source-encoding Jacobian | Which geometric requests the three-state source can realize |
| Control evaluation | Repeat across gain settings, input budgets and longer horizons | Tradeoffs between output error, divergence and actuator effort |
| Mathematical extension | Define and benchmark a separate octonion-valued evolution law | Effects attributable to the proposed algebraic dynamics |

The last item is an extension beyond the current source-equation-to-encoding pipeline. Its algebra, constraints, integrator, and benchmark would need to be specified before attributing an observed effect to octonion multiplication or nonassociativity.

## 19. AI assistance and project scope

The numerical trajectory and geometric identities are defined by the mathematical model. Optional AI assistance can help explain controls, discuss observed patterns, formulate comparisons, and summarize available experiment data. The wider application includes optional chat, voice and research-oriented integrations.

AI-generated interpretations remain hypotheses until checked against the recorded numerical quantities. A fluent explanation is not an additional measurement, and provider availability or external API performance is separate from the deterministic numerical calculation.

The project currently offers a structured environment for mathematical exploration, comparative diagnostics and controlled computational experiments. Its aim is to turn interesting visual patterns into explicit questions that can be tested.

## 20. References and preview organization

### Scientific references

1. **Lorenz, E. N. (1963).** *Deterministic Nonperiodic Flow.* Journal of the Atmospheric Sciences, 20, 130–141. [Original paper PDF](https://people.ucsc.edu/~rmont/classes/chao/2013/Orig_Papers/Lorentz_EN.pdf).
2. **Rössler, O. E. (1976).** *An equation for continuous chaos.* Physics Letters A, 57(5), 397–398. [DOI](https://doi.org/10.1016/0375-9601(76)90101-8) · [Original paper PDF](https://www.chaotic-circuits.com/wp-content/uploads/2015/01/AN-EQUATION-FOR-CONTINUOUS-CHAOS_ROSSLER.pdf).
3. **Baez, J. C. (2002).** *The Octonions.* Bulletin of the American Mathematical Society, 39, 145–205. [Author paper](https://arxiv.org/abs/math/0105155) · [Author's discussion of projective lines and Hopf bundles](https://math.ucr.edu/home/baez/octonions/node9.html).
4. **SciPy documentation.** *Periodogram: density and spectrum scaling, windowing, and one-sided spectra.* [Official reference](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.periodogram.html).

These references provide background for the named source systems, division-algebra geometry, and spectral conventions. The specific feature encoder, declared graph metric, controls, and validation numbers above describe this project and its recorded checks; the references do not independently validate the complete application.

### Public preview organization

Place this file at the root of the GitHub preview repository as `README.md`. Place the eight previously supplied animation files in a folder named `media` so the gallery links and embedded GIFs resolve. This README contains no source restoration, installation, API-key setup, or private notebook links.

Feedback on mathematical definitions, diagnostic interpretation, experiment design, and numerical validation is welcome.

**Creator: Christopher Salnave**  
**Project: AI-Based Fractal Octonion Labs**
