# DLIT Dual-DAWI Boundary Frequency Calculator

A self-contained browser calculator for estimating the two main RF frequencies used to define the low- and high-mass boundaries of a dual-digital-asymmetric-waveform-isolation (dual-DAWI) sequence in a digital linear ion trap (DLIT).

The calculator runs entirely in the browser. It has no external dependencies and does not upload input values or experimental data.

## Run locally

Open [`index.html`](index.html) in a modern web browser. No build step or local server is required.

For a public web version, enable GitHub Pages for the repository and publish the default branch from the repository root. The resulting URL will normally be:

```text
(https://elite-lulu.github.io/DLIT-Calculator/)
```

## Inputs and outputs

Inputs:

- Duty cycle, `d`
- Single-ended RF voltage amplitude, `V_rf`
- Characteristic electrode radius, `r_0`
- Geometric field-correction coefficient, `A_2`
- Lower mass boundary, `M_min`
- Upper mass boundary, `M_max`

Outputs:

- `f_LM`, the DAWI-1 main RF frequency associated with `M_min`
- `f_HM`, the DAWI-2 main RF frequency associated with `M_max`
- `f_HM - f_LM`
- The numerically solved `q_left` and `q_right` boundaries

## Calculation method

At the selected duty cycle, the calculator solves the Floquet trace equations numerically for the stability boundaries:

```text
Tr(M_x) =  2  at beta_x = 0  -> q_left
Tr(M_y) = -2  at beta_y = 1  -> q_right
```

The mass-to-frequency conversion is implemented as:

```text
C     = e A_2 V_rf / (u r_0^2 pi^2)
M_max = C / (q_left  f_HM^2)
M_min = C / (q_right f_LM^2)
```

The code converts `r_0` from millimeters to meters and converts the system constant from Hz^2 Da to MHz^2 Da before calculating the displayed frequencies. The boundary-setting relations are described by eqs 3 and 4.

The shipped defaults are `r_0 = 5.00 mm` and `A_2 = 0.810`, matching the rounded parameter values used in the manuscript. The other example defaults are `d = 0.606075`, `V_rf = 100 V`, `M_min = 193.5 Da`, and `M_max = 196 Da`. Change these values when modeling a different instrument or target.

The numerical root finder uses a scan followed by bisection. It is intended for the operating range represented by the current DLIT implementation. It should not be interpreted as a universal stability solver for every digital-ion-trap geometry or waveform convention.

## Scientific scope and limitations

- The calculator estimates nominal DAWI boundary frequencies from the supplied model parameters.
- It does not simulate ion loss, space-charge effects, switching transients, field imperfections, or the experimental AC ejection response.
- The output is not a substitute for instrument-specific calibration or experimental verification.
- The default values are a starting configuration and should be replaced with the parameters of the instrument being studied.

## Relation to the manuscript

This calculator implements the boundary-frequency expressions and the Floquet-based q-boundary calculation described in:

> Lu, J.; Li, D.; Zhang, Y.; He, X.; Li, L.; Li, P. *A Rapid Two-Stage High-Resolution Ion Isolation Strategy for Portable Digital Linear Ion Trap Mass Spectrometry*. Manuscript associated with this repository. Add the final journal citation and DOI after publication.

The calculator is intended as a reproducibility aid for the dual-DAWI boundary-setting procedure. It is not a replacement for the Supporting Information derivation.

## Repository structure

```text
.
|-- index.html
|-- README.md
`-- LICENSE        # add before public release
```
## Citation and feedback

When using the calculator in a publication, cite the associated manuscript and this repository. For bug reports or improvements, open a GitHub issue with the input parameters, browser, expected result, and observed result.

