# Packaging validation

Both supplied notebooks were byte-identical. All 306 embedded source files matched the supplied SHA-256 manifest before documentation changes. Notebook outputs and execution counts were cleared for distribution.

Python source syntax: PASS

## test_fft_localization_595.js

Exit code: 0

```text
PASS occupied frequency union protects every weak channel and displayed baseline independently
PASS current-window crop retains genuine broadband power, reports discarded mass, and includes DC honestly
PASS energy-fit rejects mismatched, nonuniform or invalid spectral arrays
PASS default FFT is actual h with valid baseline while response metrics and coordinate traces remain paired
PASS display-only frequency, linear/log and heatmap contrast choices leave exact PSD and source data unchanged
PASS paired and follow signals are explicit; their PSDs and history are distinct without changing response metrics
PASS independent actual-spectrum baseline checkbox works in paired response and missing paired data never silently falls back
PASS narrow canvases retain readable units while detailed crop policy stays in wrapping DOM text
PASS zero spectral power remains dark at every heatmap contrast without inventing a PSD floor
9 localized FFT display checks passed.

```

## test_dashboard_fft_595.js

Exit code: 0

```text
PASS lorenz post-curvature PSD uses native h samples, direct-DFT normalization and source-time Nyquist
PASS henon post-curvature PSD uses native h samples, direct-DFT normalization and source-time Nyquist
PASS paired spectrum uses each native actual-minus-baseline coordinate, not sparse response endpoints
PASS advancing replay observations preserve the bounded FFT update cadence
PASS replay rewind and source reset never expose a future or previous-run spectrum
{"passed":5,"scope":"Actual dashboard/worker plus independent direct DFT; no browser layout, live audio or provider claim"}

```

## test_dashboard_curvature_595.js

Exit code: 0

```text
PASS fix18g starts with real bounded curvature coupling and visible native FFT controls
PASS top-level off and restore buttons change the actual worker configuration
PASS existing preset button explicitly starts a measured, matched curvature experiment at observation one
PASS deliberate zero gain is preserved and never converted into a fabricated effect
PASS SPSA and curvature remain exclusive; Reset model does not misrepresent other feedback
PASS unsupported source retains geometry inspection and rejects coupling without resetting
PASS paired actual samples and connectivity records survive export and causal replay
PASS failed publication blocks controller mutations and preserves the applied measured packet
8 fix18g active-curvature integration checks passed. Native browser layout and provider calls were not tested.

```

## test_curvature_default_594.js

Exit code: 0

```text
silon": 0.011034371591949565,
      "witnessBeta0Actual": 1,
      "witnessBeta0Baseline": 4,
      "distanceConvention": "Euclidean chord in all five S4 coordinates / 2; fixed round-sphere geometry",
      "rk4HalfStepEndpointChange": 6.022977110099402e-7
    },
    {
      "system": "rossler",
      "samples": 512,
      "sourceTime": 10.24,
      "nativeDt": 0.02,
      "solverDt": 0.01,
      "forceNorm": 0.005,
      "deltaState": 0.14267594149325336,
      "deltaH": 0.020551593094029444,
      "deltaJTJ": 0.11625736672779866,
      "deltaMeanRicci": -0.00794387102959243,
      "deltaSectionGaussian": 0.0020584171993000833,
      "window": 128,
      "epsilon": 0.05,
      "beta0Actual": 1,
      "beta0Baseline": 1,
      "epsilonCriticalActual": 0.013843321283399487,
      "epsilonCriticalBaseline": 0.01297183057210868,
      "deltaCriticalEpsilon": 0.0008714907112908066,
      "posthocWitnessEpsilon": 0.013407575927754083,
      "witnessBeta0Actual": 4,
      "witnessBeta0Baseline": 1,
      "distanceConvention": "Euclidean chord in all five S4 coordinates / 2; fixed round-sphere geometry",
      "rk4HalfStepEndpointChange": 2.548949448558322e-10
    }
  ],
  "scope": "Deterministic numerical tests of explicit graph-curvature feedback into Lorenz/Rossler. Fixed S4/S7 round metrics are not deformed; connectivity uses changed samples with the same chord distance. No universal direction of clustering, physical-force interpretation or hidden-fiber recovery is asserted."
}

```

## test_dynamic_fft_594.js

Exit code: 0

```text
PASS completed native windows advance five-channel history while identical and pending frames stay inert
PASS history survives sub-ulp measured-dt roundoff, remains bounded, and stores genuine distinct source endpoints
PASS rewind, source context, signal mode and FFT size changes start fresh history; gaps never become spectra
PASS real gapped histories stay unavailable and future samples cannot enter spectral history
PASS mounted five-row history advances from exact FFTs and pause/drawing adds no columns
PASS mounted rewind and FFT window changes clear old spectrum history without fabricating warmup
6 dynamic FFT history checks passed.

```

These are focused automated checks, not validation of scientific novelty. Live Colab, microphone, tunneling, external AI services, full browser layout and the complete historical test suite were not exercised. Original source models and optional service dependencies were not modified.

## Separate-file delivery

Source restoration passed: 306 files; source bytes matched the prior verified extraction; repeated restoration is safe; the current README is preserved; existing different files are rejected. Numerical source was not modified.
