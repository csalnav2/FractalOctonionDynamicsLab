# Fractal–octonion laboratory: mathematical process and interpretation

Source audit for revision 582 / v5.70-dynamic-fix12. This guide describes the implemented numerical pipeline, not an assertion that every visual feature is an independent physical measurement. Function names below refer to the distributed JavaScript source. The current screenshots are illustrations; quantitative claims should use an exported run with its configuration and sample times.

## 1. What the project actually does

The central experiment is: evolve a specified source system; construct eight features from its state and rate; normalize those features to a point on S⁷; observe that point through quaternionic Hopf and other explicitly defined maps; measure spectra, observation sensitivity, chosen geometric curvature, and finite-cloud connectivity; and compare bounded source interventions against a matched unforced baseline.

The eight features are not eight independent parameters of an attractor. The code integrates the source equations and then encodes each sample. It does not obtain the source trajectory by repeatedly multiplying octonions. An eight-component real vector can be represented in an octonion basis, but that representation alone does not define an octonion dynamical law or imply that nonassociativity is causing the displayed dynamics.

Use **coral red for actual/controlled**, **cyan blue for matched unforced baseline**, and **gold for requested target** in comparisons. Label these roles in words as well as color. An actual trajectory need not be perturbed: in off/readout/zero-action cases it should match its baseline. Colors used for the two separate geometric observation routes have a different meaning and must retain their route labels.

**Implementation:** `fractal_engine.js` (`PRESETS`, `Simulation`, `encode`); `feedback_control.js` (`Simulation`); `curvature_experiment.js` (`Simulation`).

## 2. Source equations, integration, and time

For the continuous presets the state is X=(x,y,z) and dX/dt=F(X;θ):

| Preset | Vector field | Default native Δt | RK4 substeps per native sample |
|---|---|---:|---:|
| Lorenz | (σ(y−x), x(ρ−z)−y, xy−βz), with σ=10, ρ=28, β=8/3 | 0.01 | 2 |
| Rössler | (−y−z, x+ay, b+z(x−c)), with a=b=0.2, c=5.7 | 0.02 | 2 |
| Thomas | (sin y−bx, sin z−by, sin x−bz), with b=0.208186 | 0.05 | 2 |

The other implemented presets are discrete maps:

- Hénon: (x,y,z)↦(1−1.4x²+y, 0.3x, 0).
- Logistic delay coordinates: (x,y,z)↦(3.99x(1−x), x, y).
- Mandelbrot orbit: (x,y)↦(x²−y²−0.12, 2xy+0.74); the third coordinate stores the squared magnitude of the new complex state. An escaping orbit is stopped after the accepted escape sample.

The map sampling interval is one iteration. “Mandelbrot orbit” is not a guarantee of an attractor, and “chaotic” is not a guaranteed classification of every preset, parameter setting, or finite run. “Maedung” is not a preset key in the audited engine; a specific additional attractor requires its equations and parameter convention.

The fixed native interval is distinct from the RK4 substep, topology selection interval, FFT window length, camera speed, and rendering frame rate. Native ODE time is model time. The feedback controls introduce an explicit conversion τ=s·t, where s=`secondsPerModelUnit`; their Hz and per-second growth labels use that declared conversion. It is not an experimentally calibrated physical clock unless the user supplies such a calibration.

The common 22-second sweep is **active display time**, not necessarily 22 source-time units and not a complete attractor orbit. A chaotic attractor generally has no finite “full trajectory” to close. Extending source duration improves coverage, but does not make the path periodic. A rendered trace should not join its newest point back to its oldest point merely to conceal a finite-window boundary.

**Implementation:** `fractal_engine.js` (`vectorField`, `mapStep`, `rk4`, `Simulation.advance`); `feedback_control.js` (`settings`); `run_recorder.js` (`CONVENTION`).

## 3. Exact eight-feature encoding and normalization

Let S=(S₀,S₁,S₂) be fixed scale factors, c the fixed center, and R the rate scale. Define

\[
u_j=\frac{X_j-c_j}{S_j},\qquad
r_j=\frac{F_j(X;\theta)}{S_jR}\quad\text{for an ODE}.
\]

For a map, the rate feature is (X_new−X_previous)/(S_jR), a per-iteration difference. The feature vector is

\[
v=(u_0,u_1,u_2,r_0,r_1,r_2,\|u\|,\|r\|),
\quad \rho=\|v\|,
\quad q=v/\rho\in S^7\subset\mathbb R^8.
\]

| Preset | S | R |
|---|---|---:|
| Lorenz | (20,30,50) | 10 |
| Rössler | (12,12,25) | 1 |
| Thomas | (4,4,4) | 0.3 |
| Hénon | (1.5,0.5,1) | 1 |
| Logistic delay | (1,1,1) | 1 |
| Mandelbrot orbit | (2,2,4) | 1 |

The audited centers are zero. A nonfinite feature or singular normalization is rejected rather than replaced with invented data. The raw norm ρ is exported alongside v, q, the source state, sample index, and source time. Normalization discards ρ from q; retaining it separately helps distinguish normalization from a lossless transform.

The last two features depend on the preceding six. For an ODE the rate features also depend on the three state coordinates and fixed parameters. Consequently this encoder does not create seven independent source degrees of freedom merely because q lies on S⁷.

In a controlled ODE run the encoder still evaluates the **original unforced vector field F at the actual controlled state**. The added actuator input is recorded separately. Thus r is not the full controlled derivative F+Su. This convention is deliberate and must accompany interpretation of rate-sensitive features.

**Implementation:** `fractal_engine.js` (`encode`); `feedback_control.js` (`makeEncoding`, `meta.feedback.encoding`); `curvature_experiment.js` (`encoding`, `record`).

## 4. Quaternionic Hopf observation and its restriction in this encoder

Split q into scalar-first quaternions a=(q₀,q₁,q₂,q₃) and b=(q₄,q₅,q₆,q₇). The implemented map is

\[
H(a,b)=\left(2a\bar b,\ |a|^2-|b|^2\right)=h\in\mathbb R^5.
\]

The quaternionic norm identity gives

\[
\|H(a,b)\|^2=4|a|^2|b|^2+(|a|^2-|b|^2)^2
=(|a|^2+|b|^2)^2.
\]

Therefore q∈S⁷ implies h∈S⁴. S⁴ is a four-dimensional sphere embedded in five-dimensional Euclidean space, not a five-dimensional sphere. The five coordinate signals h₀,…,h₄ obey Σh_k²=1.

There is an additional, important restriction from this particular feature ordering:

\[
\rho^2=2(\|u\|^2+\|r\|^2),\qquad
h_4=-\frac{r_1^2+r_2^2}{\|u\|^2+\|r\|^2}\le0.
\]

The ordinary encoded trajectory therefore explores a restricted portion of S⁴. A negative h₄ trace is not itself an error. A paired response Δh₄=h₄,actual−h₄,baseline can have either sign. A target or analytically constructed geodesic may leave the actual encoder image; it need not be physically reachable by the three-state source.

For any unit quaternion r, (a,b)↦(ar,br) preserves H. The canonical fiber is S³, of real dimension three. The Hopf map is many-to-one; observing h alone does not uniquely recover q. The three circles drawn in a fiber panel are great-circle generator sections of that S³ fiber, not the entire fiber and not measurements of an unobserved temporal trajectory.

**Implementation:** `fractal_engine.js` (`hopf4`); `hopf_fiber_core.js` (`rightAction`, `fiberGeodesic`, `build`).

## 5. Projections, smooth movement, and honest animation

A three-coordinate projection of a five- or eight-coordinate object is a visualization, not an isometric reconstruction. Apparent crossings in a 3D view do not establish that fibers intersect or link in S⁷. The separate paired observation uses q=(a₀,a₁,a₂,a₃,b₀,b₁,b₂,c), blue=Hopf(a/|a|)∈S² and red=b/|b|∈S². It is different from the quaternionic S⁷→S⁴ map. A zero-sized required block makes that observation undefined; no point should be silently removed to make the chart look smooth.

For two unit vectors p and q with angle ω=arccos(p·q), spherical linear interpolation is

\[
\operatorname{SLERP}(p,q;\alpha)=
\frac{\sin((1-\alpha)\omega)}{\sin\omega}p+
\frac{\sin(\alpha\omega)}{\sin\omega}q.
\]

Numerical limiting rules are needed near coincident or antipodal endpoints. SLERP provides a sphere-respecting display cursor between sampled endpoints; it does not add measured samples or improve solver accuracy. Smooth camera/fiber motion, solid traces, wider bins, adaptive axes, centered layouts, and higher canvas resolution are presentation changes. FFTs, matrix values, connectivity, and exported measurements must continue to use actual stored source samples.

The analytic fiber/geodesic parameter s in q(s)=q cos s+v sin s is unit-S⁷ arclength, not source time. A horizontal geodesic maps to unit-S⁴ motion with speed two; a vertical fiber geodesic has constant Hopf basepoint. Smooth interpolation of these constructions should not be described as recovered hidden dynamics. A fiber speed setting can be halved without changing the underlying source integration.

All three visible Cartesian axes should meet at (0,0,0) in the displayed coordinate system. Adaptive plot limits should include all currently drawn actual/baseline/target data, including negative values, with padding. Data below y=0 are valid unless the quantity is mathematically nonnegative; hiding negative coordinates would be a measurement error.

**Implementation:** `hopf_fiber_core.js` (`makeCurve`, `visualGeometry`, `visualFiberGeometry`); `fractal_diagnostics.js` (`geometry`).

## 6. Meaning of the 8×8 matrices: JᵀJ, direct, and hidden

These matrices describe an **observation-map differential**, not the Jacobian of the source flow F. Let T=I−qqᵀ project onto the seven-dimensional tangent space of S⁷. For the canonical Hopf map, the code computes the tangent-restricted differential J=dH_q T, a 5×8 matrix, then

\[
G=J^\mathsf TJ,\qquad O=G/4,\qquad N=T-O.
\]

O is the rank-four visible/horizontal tangent projector and N the rank-three hidden/vertical tangent projector. They satisfy O²=O, N²=N, ON=0, O+N=T, and JN=0. The radial direction q is excluded from both. A perturbation Nv is locally invisible to this observation map, not necessarily invisible to every measurement or unreachable to a separately imposed source actuator.

For the paired S²×S² observation, the regular-chart visible projector still has rank four and hidden projector rank three, but G has unequal block weights 4/|a|² and 1/|b|². In that map G is not simply four times O and can become poorly conditioned near a vanishing block.

The animated 8×8 surface changes because q changes and therefore the local observation sensitivity changes. Height is a matrix entry; its x/y coordinates are component indices, not attractor coordinates. It is not itself a curvature surface. The accompanying 3D connectivity network is another diagnostic of the same measured samples; it does not imply that JᵀJ caused its edges.

**Implementation:** `fractal_diagnostics.js` (`s4Projectors`, `pairProjectors`, `projectorSummary`).

## 7. What curvature is being measured

The variable Ricci/Weyl experiment declares a four-dimensional graph immersion into Euclidean R⁵:

\[
x=1.6(q_0,q_1,q_2,q_3),\quad F_g(x)=(x,f(x)),\quad
f(x)=\tfrac12x^\mathsf TAx+w\sin(b^\mathsf Tx).
\]

The default A, w=0.18 and b=(1.1,0.75,−0.65,0.9) are explicitly stored in `curvature_diagnostics.js`. If p=∇f and D²f is its Hessian,

\[
g=I+pp^\mathsf T,\quad B=\frac{D^2f}{\sqrt{1+\|p\|^2}},\quad
S=g^{-1/2}Bg^{-1/2}.
\]

The Gauss equation in an orthonormal frame gives R_ijkl=S_ik S_jl−S_il S_jk. Hence scalar curvature R=(tr S)²−tr(S²), and Ric=(tr S)S−S². The dashboard also computes trace-free Ricci and the Weyl tensor. Its “signed mean Ricci” objective is R/4. Gaussian curvature belongs to a separately frozen two-dimensional section with its own induced metric.

The metric is positive definite because g has eigenvalues 1,1,1,1+||p||². Its curvature changes as the measured point moves through this chosen metric or when metric settings change. This is **not curvature inferred from the attractor**, nor the intrinsic curvature of the round S⁴ or round S⁷. A unit round S⁴ would instead have constant sectional curvature one, Ric=3g, scalar curvature 12, and zero Weyl tensor.

Geometry-only settings change this inspection metric without forcing the source. The separate coupled mode constructs a bounded source intervention, supported for Lorenz and Rössler. With B_source=∂q/∂u_source, a selected visible/hidden gradient request r is mapped to source input B_sourceᵀr and capped. The input is held for eight native intervals. “Increase/decrease curvature” is a local control request, not a guarantee of monotonic measured curvature under the original dynamics. Hidden requests can acquire a visible component through the reachable three-state source.

**Implementation:** `curvature_diagnostics.js` (`MODEL`, `graphField`, `graphTensor`, `decompose`, `sectionMetric`); `curvature_experiment.js` (`model`, `gradient`, `sourceJacobian`, `force`, `Simulation.advance`).

## 8. FFT, power density, and the Shannon–Nyquist condition

The opening spectral calculation accepts N=128,256,512,1024,2048. The post-curvature S⁴ power-density panel accepts N=256,512,1024,2048. The supplied spectral screenshots use **2048 native samples**. These N values are sample-window lengths, not RK4 substeps or octonion multiplication counts. Topology window sizes are a separate setting, normally 128 and bounded at 256.

For one uniformly sampled coordinate x_j, sample interval Δt, and periodic Hann window,

\[
\bar x=N^{-1}\sum_jx_j,\quad w_j=\tfrac12-\tfrac12\cos(2\pi j/N),\quad
X_k=\sum_{j=0}^{N-1}w_j(x_j-\bar x)e^{-2\pi i jk/N}.
\]

The one-sided power spectral density is

\[
P_k=c_k\,\frac{\Delta t\,|X_k|^2}{\sum_jw_j^2},\qquad
c_k=1\text{ at DC/Nyquist, otherwise }2.
\]

\[
f_k=\frac{k}{N\Delta t},\quad f_s=\frac1{\Delta t},\quad
f_N=\frac1{2\Delta t},\quad\Delta f=\frac1{N\Delta t}.
\]

The native sample span is (N−1)Δt; the DFT period is NΔt. For default Lorenz Δt=0.01 and N=2048, these are 20.47 model units and 20.48 model units, with fs=100, Nyquist=50 and Δf≈0.048828 cycles/model unit. Frequency should not be called physical Hz without an appropriate time calibration.

The normalization obeys

\[
\sum_kP_k\,\Delta f=
\frac{\sum_jw_j^2(x_j-\bar x)^2}{\sum_jw_j^2}.
\]

This is Hann-weighted centered energy, not automatically the ordinary unweighted variance. Removing the unweighted mean before applying the Hann window need not make the weighted DC bin exactly zero. A tiny nonzero DC result is therefore not automatically a software artifact. Warm-up periods, missing samples, and undefined values must remain explicitly unavailable rather than plotted as fabricated zeros.

Exact Shannon–Nyquist reconstruction requires a bandlimited underlying signal and a sample rate exceeding twice its highest frequency. A finite FFT cannot prove that the source is bandlimited or alias-free. Increasing N improves frequency spacing but does not raise Nyquist; decreasing the native Δt does. Downsampling calls for an anti-alias filter before decimation. The optional multiscale observer keeps its causal filtered signal spectra separate from raw geometric measurements.

The opening amplitude FFT is a different normalization, A_k=c_k|X_k|/Σw. Do not compare its vertical values directly with PSD. The post-curvature PSD uses contiguous native IDs in one segment with uniform timestamps, without zero padding, resampling, or SLERP samples. In paired response mode it analyzes **Δh=h_actual−h_baseline**; generally PSD(Δh)≠PSD(h_actual)−PSD(h_baseline).

In revision 582, selecting **Live measurements** also enables the checked-by-default **“Overlay matched unperturbed baseline · traces and FFT”** control when a validated feedback or curvature baseline exists. The actual h is coral red and its matched unforced h is cyan blue; both use solid lines. Each coordinate channel shares one actual-plus-baseline vertical scale and a common frequency range. Separate FFT engines center and Hann-window the two recorded signals with identical N, Δt, sample IDs, source times and PSD normalization. The baseline is locked to the actual spectrum's completed window even while the next computation is pending. Exact probes report both measured powers; the common 220 ms display animation interpolates power only. A missing matched baseline remains a trace gap or unavailable spectrum, never a zero signal. In logarithmic mode exact zero is a gap rather than an invented positive floor. The paired response view still shows Δh and PSD(Δh); the automatic view can select that paired response when one is available, so select Live explicitly to inspect the overlaid actual/baseline spectra.

“Spectral fingerprint” is useful informal shorthand for a descriptive spectral signature of this encoded observable and window. It is not a proven unique identifier, a physical spectroscopic measurement, or an inverse map to the attractor. Power discards Fourier phase, and distinct trajectories can share a power spectrum. The plotted h channels use their own vertical scales; compare numerical power values and matched windows, not visual peak heights across rows. A changing frequency-focus range can make the same spectral content appear differently spaced.

The PSD computation is limited to at most four updates per display second; smooth power-space drawing between results is presentation only. Each spectrum engine owns 49,200 bytes of reusable FFT/window/output buffers at N=1024 or 98,352 bytes at N=2048. With both actual and optional baseline engines allocated, their combined internal buffers are 98,400 or 196,704 bytes respectively. These figures exclude source history, presentation buffers, detached snapshots, canvas and browser overhead; they are not total heap measurements. Enlarging the plot principally increases rendering memory, not the native scientific sample count.

**Implementation:** `fractal_engine.js` (`spectrum`, `amplitudeSpectrum`, `psdSpectrum`); `s4_spectrum.js` (`SIZES`, `create`, `update`); `multiscale_diagnostics.js` (`fir`, `spectrum`, `analyze`).

## 9. Perturbations, target programs, and SPSA

The matched baseline starts with the same source initial state, source parameters, integration rule, and time grid, but receives zero source input. For the controlled branch,

\[
\dot X=F(X;\theta)+\operatorname{diag}(S)u.
\]

An observed actual-minus-baseline separation is a finite intervention response in this simulation. Large separation alone is expected for chaotic systems and is not proof of stabilization, improved information recovery, or a changed attractor class.

The program supports sine, step, box and chirp segments; segment amplitude, frequency, duration, phase and route are explicit. A sine uses sin(2πfτ+φ). A linear chirp uses phase φ+2π[f₀τ+(f₁−f₀)τ²/(2T)]. Thus phase shifts are supported by the target program, although phase is not reported by a PSD plot. The command guard requires at least twenty native samples per requested sine/chirp cycle. Source samples, rather than wall-clock render frames, determine the command phase.

Available routes are S⁴, visible JᵀJ entries, hidden N entries, both matrix routes, a smooth connectivity surrogate, and combined routes. A requested S⁴ target is a great-circle displacement of the baseline h, with angle θ=(π/4)·clamped signal. Matrix targets are requests for selected entries; actual matrices are always recomputed from the resulting source state. Such requests can be mutually incompatible, violate the structural restrictions of a projector/Gram matrix, or lie outside the source encoder image.

The controller estimates the selected outputs' sensitivity to the three scaled source coordinates by centered differences, then applies a damped least-squares pullback:

\[
u_{request}=(J_s^\mathsf TJ_s+\lambda I)^{-1}J_s^\mathsf Te.
\]

It clips input magnitude to `maxForce × amplitude` and limits the step-to-step input change to `maxForce × Δt`. This is an explicit actuator coupling; it is not inverse Hopf reconstruction. The connectivity objective is an average sigmoid of recent q distances, not the exact integer β₀. Its purpose is a smoother optimization signal, not a claim that its optimum guarantees connectivity.

SPSA tunes four nonnegative route gains, bounded in [0,2]: S⁴, visible matrix, hidden matrix, and connectivity. It does not tune eight attractor parameters or directly overwrite measured tensor entries. Default settings are seed 553, update every 128 active samples, rollout horizon 16 samples, initial gains (1,1,1,1), base learning rate 0.08 and perturbation 0.12.

At optimizer iteration k, active gains receive seeded Rademacher signs Δ_k,j∈{−1,+1}. Inactive routes receive zero. The code uses

\[
c_k=0.12/(k+1)^{0.101},\qquad a_k=0.08/(k+1)^{0.602},
\]

with user-configurable numerator constants. Probes θ⁺ and θ⁻ are clipped to [0,2]. The gradient component is (L(θ⁺)−L(θ⁻))/(θ⁺_j−θ⁻_j), using the actual clipped displacement. The candidate is clip(θ−a_k ĝ). It is accepted only when valid and its evaluated cost improves the unchanged-gain cost by more than 10⁻¹⁰.

The loss is

\[
L=\pm\tanh(\gamma_H)+0.1\overline{e^2}
+0.02\overline{\|u\|^2/U_{max}^2}
+0.01\overline{\|u_n-u_{n-1}\|^2/(\mathrm{maxForce}\,\Delta t)^2}.
\]

The plus sign requests lower predicted growth; the minus sign requests higher growth. Tiny denominator guards avoid division by zero. γ_H is a **predicted finite-horizon finite-perturbation growth**, computed by two branches initially separated by 10⁻⁵ in normalized source coordinates. Each branch independently recomputes its feedback and propagates its input memory; there is no intermediate separation renormalization. Both branches share the starting baseline, program time and initial input memory.

The implementation evaluates four cloned common-start candidate scenarios per update: plus probe, minus probe, unchanged gains, and proposed gains. These do not replace the live trajectory. A short-horizon cost improvement is not a theorem of convergence, long-term stability, or global chaos control. The legacy adaptive mode uses a separate bounded coordinate-search heuristic; it should not be described as SPSA.

**Implementation:** `feedback_control.js` (`programSettings`, `waveform`, `programChannels`, `connectivityScore`, `programForce`, `spsaProposal`, `evaluateGains`, `optimize`, `policy`).

## 10. What “conditional” means in the screenshot

The online displayed conditional growth is different from the SPSA look-ahead objective. The actual source receives its realized input u_n. A nearby shadow source receives **the identical u_n**, rather than recomputing a controller response to the shadow's changed state.

Let D=diag(S), δ=10⁻⁷, and d_n=D⁻¹(X_shadow,n−X_actual,n). After each interval the code measures g_n=log(||d_n||/δ) and renormalizes the shadow to normalized distance δ. It excludes the first 100 active intervals as a tangent-alignment transient, then reports

\[
\lambda_{conditional}=
\frac{\sum_{n\in\mathrm{retained}}g_n}
{M\,\Delta t\,\mathrm{secondsPerModelUnit}}.
\]

The unforced baseline has its own shadow and the same procedure with zero input. The displayed statistic is a cumulative finite-time estimate over those M post-transient intervals, not a sliding-window average or an asymptotic Lyapunov exponent. “Conditional” means conditional on the realized forcing history. It omits changes in the control law and controller memory caused by perturbing the state, so it is not the full closed-loop Lyapunov exponent.

In the supplied screenshot both growth curves appear positive. The controlled curve is lower than the unforced curve, which is consistent with reduced finite-time conditional divergence in that interval. It does **not** establish stabilization. Even a negative finite-window value would require checks across durations, initial conditions, parameter settings, and perturbation magnitudes before a broader stability claim.

**Implementation:** `feedback_control.js` (`LegacySimulation.advance`, `Simulation.advance`, `analyze`).

## 11. Connectivity, β₀=1, and recorded epsilon values

For a finite cloud of n points, connect pairs whose normalized distance is ≤ε. β₀(ε) is the number of connected components. If the complete-graph minimum spanning tree has edge lengths d₁≤…≤dₙ₋₁, then

\[
\beta_0(\varepsilon)=n-\#\{d_j\le\varepsilon\},\qquad
\varepsilon_* = d_{n-1}.
\]

Thus ε* is the first edge threshold at which the cloud has one component. A singleton/all-coincident cloud has ε*=0. Empty or singular whole windows remain undefined.

| Recorded cloud | Points | Normalized distance |
|---|---|---|
| `q` | Unit q∈S⁷⊂R⁸ | ||q−q′||/2 |
| `pair` | Concatenated blue/red unit S² observations in R⁶ | ||[blue,red]−[blue′,red′]||/(2√2) |
| `s4` | Unit h∈S⁴⊂R⁵ | ||h−h′||/2 |
| `hidden` | Rank-three hidden projector N, flattened in a common R⁶⁴ basis | ||N−N′||F/√6 |

The hidden cloud records local tangent subspaces, not recovered hidden states. Its definition depends on whether the selected observation map is canonical S⁴ or paired S²×S². All four clouds use the same selected source sample IDs for a valid comparison. Distances and ε are unitless under these conventions. Their values are not interchangeable with unnormalized physical source-space distance.

An ambient Čech ball radius r*=ε*/2 gives the same connected-component threshold because two balls intersect when their center separation is ≤2r. This statement concerns H₀; the software does not thereby compute all higher Čech or Rips homology. β₀=1 means **one connected component**, not one point, one simplex, a contractible complex, disappearance of all holes, or physical trajectory collapse.

The 3D topological network uses full five-coordinate S⁴ distances for edges even when only three coordinates are drawn. Its exact zero-order graph Hodge operator is L₀=B₁B₁ᵀ, with (L₀f)_i=Σ_{j~i}(f_i−f_j). The graph cycle rank E−n+β₀ is not simplicial β₁. Network changes reflect moving samples, ε, window and stride; they are not an intrinsic curvature measurement.

The recorder already retains ε* for **all four clouds at every committed worker endpoint**, separately from its lower-cadence matrix/diagnostic archive. It does not invent values for solver steps at which that diagnostic was not computed. The dedicated collapse CSV includes:

- `run_id`, `cycle`, `display_seconds`, `sample_index`, `source_time`, `segment`;
- `cloud`, `epsilon_beta0_one`, `cech_radius_beta0_one`, `beta0_at_threshold`, `beta0_at_selected`, `selected_epsilon`;
- `defined`, `reason`, `map_mode`;
- `window_n`, `window_required`, start/end IDs and times, `sample_stride`, `effective_sample_interval`;
- `distance_convention`, `distance_scale`, `sample_ids_json`.

The full run JSON includes native samples, feedback/source inputs, paired baseline data, configuration events and these threshold frames. The main CSV also includes rows typed `beta0_collapse_threshold`. Capacity is explicitly bounded (default 12 sweeps, 40,000 samples, 32 MiB serialized-entry accounting); a capacity stop retains existing recordings and reports the stop instead of silently discarding them.

If a temporal “time to one component” is desired, define it separately at a **fixed ε, cloud, sample window/stride, and post-intervention origin**, and require a complete window to avoid a trivial one-point warm-up. A rolling cloud can subsequently disconnect as points enter and leave. Report a first-passage event only over actual evaluated times, with the evaluation interval and any persistence criterion. The existing ε* filtration record alone is not such a time-to-collapse measurement.

**Implementation:** `fractal_diagnostics.js` (`completeGraphMSTState`, `componentsAt`, `h0WithStride`); `epsilon_connectivity.js` (`RollingTrace`, `SampledTrace`); `hodge_network.js` (`buildGraph`, `signalAction`, `analyze`); `run_recorder.js` (`collapseFrame`, `capture`, `exportCollapseCSV`).

## 12. A reproducible experiment and honest project recap

1. Choose the source equations, parameters, initial state and fixed native step. Record the scale factors and time convention. Let a predeclared settling interval elapse; the control default starts at model t=20.
2. Keep a matched unforced baseline with identical initial conditions and time grid. Start with off/readout/zero-amplitude checks, which should not generate a source intervention.
3. Select the observation map, native FFT length, topology window and analysis stride separately. Match FFT windows and source endpoints when comparing conditions. Longer topology strides usually cover longer durations at the same node count.
4. Declare the perturbation route, waveform, amplitude, frequency, phase, duration, force bound, and optimization objective before judging the result. Distinguish target requests from realized action and achieved output.
5. Inspect source paths, h traces, PSDs, direct/hidden observation matrices, actual versus baseline differences, and ε* together. Keep render interpolation and constructed fiber curves separate from measured samples.
6. Export the native run and all-four-cloud ε records. Report incomplete windows, guard stops, singular charts, saturation and archive capacity honestly; unavailable data are not zero measurements.
7. Repeat with multiple initial conditions, longer horizons, a smaller solver step, and declared control settings before making scientific claims about robustness. Changes in a screenshot alone are not a validated scientific result.

The implemented work combines source dynamics, explicit eight-feature encoding, quaternionic Hopf geometry, matched perturbation comparisons, bounded feedback and SPSA, declared-metric curvature, native-window S⁴ spectra with sampling metadata, finite-cloud H₀ thresholds, shared-time visualization, and bounded reproducible run exports. The visual revisions aim for smoother solid traces, consistent shared endpoints, clearer axes, centered/larger plots, and contrasting condition colors while preserving the numerical data.

A defensible public description is: **“An interactive numerical laboratory for exploring how controlled changes to nonlinear source systems appear in an eight-feature S⁷ representation, quaternionic Hopf S⁴ observables, their spectra, local observation sensitivity, and finite-window connectivity.”**

Avoid claiming eight independently discovered physical parameters, unique spectroscopic identification, inverse recovery of hidden information, a complete attractor in a 22-second loop, proven chaos stabilization from a lower positive conditional exponent, or physical/topological collapse merely because β₀ becomes one. Real-time voice/chat/search are application capabilities and require separate authenticated service and browser-permission checks; they are not validated by the mathematical identities in this guide.

## 13. Scope of the Wolfram checks

`WOLFRAM_582.md` contains the exact Wolfram Language evaluator input and returned output from the Wolfram MCP checks for this revision. The generic Hopf squared-norm polynomial residual and the encoder-specific h₄ formula residual both returned exactly zero. A numerical Hann-windowed example returned zero difference between the power spectra of x and −x, illustrating nonuniqueness. A scalar derivative example returned {a, a−k} for fixed-input versus recomputed-feedback linearizations.

These checks support the stated algebra and the distinction between conditional and full-feedback growth. They do not certify every dashboard calculation, validate a physical model, demonstrate stabilization in a user run, or test ngrok, chat, search, microphone permissions or real-time voice.
