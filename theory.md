# The Hydrogen Wavefunction — Theory & ESA Decomposition

**Equation under analysis (atomic units: ℏ = m_e = e = a₀ = 1):**

$$\psi_{nlm}(r,\theta,\phi,t) \;=\; R_{nl}(r) \cdot Y_l^m(\theta,\phi) \cdot e^{-iE_n t}$$

with

$$R_{nl}(r) \;=\; \sqrt{\Big(\tfrac{2}{n}\Big)^{3}\,\tfrac{(n-l-1)!}{2n\,(n+l)!}}\;\Big(\tfrac{2r}{n}\Big)^{l}\,L^{\,2l+1}_{n-l-1}\!\Big(\tfrac{2r}{n}\Big)\,e^{-r/n}$$

$$Y_l^m(\theta,\phi) \;=\; \sqrt{\tfrac{(2l+1)}{4\pi}\,\tfrac{(l-m)!}{(l+m)!}}\;P_l^m(\cos\theta)\,e^{im\phi}$$

$$E_n \;=\; -\tfrac{1}{2n^2}$$

The state space is indexed by three integers: principal `n ∈ {1, 2, 3, ...}`, azimuthal `l ∈ {0, 1, ..., n-1}`, magnetic `m ∈ {-l, ..., +l}`. `L^α_k` is the generalized Laguerre polynomial; `P_l^m` is the associated Legendre polynomial.

The configuration is the **complex-valued** field ψ on R³ × R. The standard atomic-orbital visualizer (Falstad's qmatom, the most cited 3D viewer) shows only `|ψ|²` — a real, positive scalar density. **That is a lossy projection.** The phase `arg(ψ)` is where time evolution lives, where superposition interference comes from, and where angular momentum is encoded. Discarding it is the quantum-mechanical analogue of showing the Rössler attractor without the trajectory: you see the form but not the rule that generates it.

This document is the apophatic prediction sheet. Each component below is decomposed in isolation; the predicted behavior under stripping is stated before the artifact lets you test it. Toggling each kill-switch in the artifact's ESA panel is a falsification test of these predictions.

---

## 1. The four constitutive components, analyzed in isolation

### 1.1 R_nl(r) — the radial function

A real-valued function of r alone. Built from three nested pieces:

- **Exponential envelope** `e^(-r/n)`: imposes square-integrability. Decay length is `n` (in units of `a₀`). Without it the function does not normalize on R³; the eigenstate cannot exist as a bound state.
- **Power prefactor** `(2r/n)^l`: forces R → 0 as r → 0 for l > 0. This is why p, d, f orbitals have a nucleus-centered node. For l = 0 (s-states), R(0) is finite and the electron has nonzero probability at the origin.
- **Generalized Laguerre polynomial** `L^{2l+1}_{n-l-1}(2r/n)`: polynomial of degree `n - l - 1` in `2r/n`. Provides `n - l - 1` real positive roots → that many **radial nodes** (zero crossings of R) at finite r.

Total radial nodes: `n - l - 1`. Total angular nodes (from Y, see §1.2 and §1.3): `l`. Total nodes: `n - 1`. This is the orbital nodal count theorem.

**Strip prediction A (kill R, set R ≡ 1):** the radial weighting disappears. Probability density becomes uniform in r out to whatever sampling boundary the artifact uses. The angular structure of Y_l^m remains visible — you see a "shell of constant thickness extending to infinity" rather than a localized orbital. The ⟨r⟩ readout diverges (or pegs to the sampling limit). The norm ∫|ψ|² ceases to be 1 (the artifact will display the new value; verifying this is the diagnostic).

**Strip prediction B (kill Laguerre, set L^{2l+1}_{n-l-1} → 1):** radial nodes vanish. R(r) = N · (2r/n)^l · e^(-r/n) becomes a single-bump function with peak at r = n·l (no zeros except at r = 0 for l ≥ 1). The orbital looks like the n = l + 1 ground state of that l-channel. *Implementation note: this kill is one I considered as a finer-grained toggle but did not surface in the v1 panel — present in code, can be exposed if Roy wants it.*

### 1.2 P_l^m(cos θ) — the polar (associated Legendre) factor

Real-valued function of θ alone. Provides `l - |m|` **polar nodal cones** (zero circles of θ at which the function vanishes). At m = ±l, the Legendre is the maximum-m solid harmonic: a single ring concentrated at the equator (no cone nodes). At m = 0, the Legendre is the Legendre polynomial proper: `l` cone nodes, the configuration concentrates along the polar axis.

The Legendre factor enforces the orbital's "angular shape" — the lobed structure that distinguishes p_z from d_{z²} from f_{x³-3xy²}. Its zeros are the nodal cones visible in the standard Falstad picture (where they appear as dark gaps in the |ψ|² cloud).

**Strip prediction C (kill Y, set Y ≡ 1/√(4π)):** all angular structure vanishes. The orbital becomes spherically symmetric — a glow shell at the characteristic radius of R_nl. The 1s, 2s, 3s shapes are preserved exactly (they were already spherical); 2p, 3d, etc. collapse to spheres. This is the literal sense in which "the angular term kills the orbital's character" — the orbital identity beyond shell number is gone. The energy ⟨E⟩ readout is unchanged (energy depends only on n in hydrogen). Norm remains 1.

This is the strongest visual ESA move in the artifact: it makes vivid that the angular structure is *not* a property of the energy level — it is an additional structural commitment that the s-states happen to lack.

### 1.3 e^(imφ) — the azimuthal phase factor

The single most important component for the present project. Complex-valued, unit-modulus, depends only on φ. Carries `m` units of angular momentum (the operator `-iℏ∂_φ` returns `mℏ` on this eigenstate).

Its modulus is exactly 1 everywhere, so `|e^(imφ)|² = 1` and it does not show up in the probability density. Falstad's |ψ|² visualization makes it invisible. **But it is the entire azimuthal structure of the wavefunction.** Killing it forces the state into a real-valued cosine standing wave — losing the rotation symmetry and gaining a chosen lobe orientation.

**Strip prediction D (kill e^(imφ), replace with cos(mφ)):** the orbital ceases to rotate about the z-axis. For m ≠ 0, the cloud spontaneously develops `2m` standing-wave lobes around the z-axis where before there were none in `|ψ|²` (because |e^(imφ)|²=1 hid them). Phase argument arg(ψ) on a constant-r constant-θ ring becomes step-wise (0 or π) instead of continuously winding. The angular momentum readout collapses from `mℏ` to 0. The state is no longer an eigenstate of L_z, but is still an eigenstate of L².

This is the cleanest demonstration in the artifact of *what phase information encodes*. The probability density before and after the strip can be visually similar (for some orientations), but the meaning is entirely different — one state carries angular momentum, one does not.

### 1.4 e^(−iE_n t) — the temporal phase factor

Complex-valued, unit-modulus, depends only on t. Rotates the global phase at rate `ω_n = E_n = −1/(2n²)`. For a single eigenstate it is unobservable: `|e^(−iE_n t)|² = 1` and the probability density is time-independent. **Eigenstates are stationary.**

For a superposition `ψ = c_A ψ_A + c_B ψ_B`, the *relative* phase `e^(−i(E_A−E_B)t)` becomes physically observable: probability density oscillates at the angular frequency `|E_A − E_B|`. The beat period is `T_{AB} = 2π/|E_A − E_B|`. For 1s + 2s: `|E_1 − E_2| = |−1/2 + 1/8| = 3/8`, so `T = 16π/3 ≈ 16.76` atomic time units.

**Strip prediction E (kill time phase, freeze e^(−iE_n t) ≡ 1):** for a single eigenstate, no visible change. The artifact will look identical, and that *is the diagnostic* — confirms that single eigenstates are stationary. For a superposition, the beating stops. The probability density freezes in whatever pattern the relative phase was at the moment of strip. This is the cleanest demonstration that *nothing physically happens in a single eigenstate*, and that all observable atomic dynamics is interference between eigenstates.

### 1.5 The full-modulus collapse — the "Falstad floor"

**Strip prediction F (project ψ → |ψ|²):** the apophatic counterpoint to the full artifact. Drop the phase entirely. The cloud's color collapses to a single hue (or whatever fallback the artifact uses to indicate "phase information has been discarded"). All the previous strip-tests that relied on phase observation (D, E) become impossible to verify. For a single eigenstate, the visual is identical to a standard atomic-orbital viewer. For a superposition, the beating *is preserved* (since |ψ|² for a superposition is time-dependent through cross terms |c_A c_B*| · 2Re(ψ_A ψ_B* e^(−iω_AB t))) — but the *cause* of the beating is no longer visible. You see the dance but not the dancers.

This is the equivalent of the Uzumaki's "literal 3D Cartesian" mode: a counterfactual rendering that shows what every conventional visualization shows you, with the full-information rendering available as the apophatic comparison. The difference is the load-bearing observation: the phase IS the quantum state.

---

## 2. Composite predictions: superposition and beating

A superposition `ψ = α ψ_{n₁l₁m₁} + β ψ_{n₂l₂m₂}` with `|α|² + |β|² = 1` is the simplest configuration whose **probability density is time-dependent**. The artifact constructs this composite by:

1. Computing each component eigenstate separately (R_nl, Y_lm, e^(−iE_n t))
2. Multiplying each by its complex amplitude (α for state A, β·e^(iδ) for state B — δ is the controllable relative phase offset)
3. Summing in the complex plane
4. Reading |sum|² and arg(sum) at each sample point

Key observable: for fixed δ = 0 at t = 0, the density at t = 0 is `|αψ_A + βψ_B|² = |α|²|ψ_A|² + |β|²|ψ_B|² + 2αβ·Re(ψ_A ψ_B*)`. The cross term `2αβ·Re(...)` is the **interference term** — it is the part of the density that is *not* a weighted sum of the parts and is the only reason superposition density is more interesting than weighted blending.

**Strip prediction G (kill superposition coupling, draw α-state and β-state in parallel without coherent sum):** the cross term disappears. Density becomes the classical mixture `|α|²|ψ_A|² + |β|²|ψ_B|²`. The beating stops. Visually, two static orbitals overlap with no interference fringe. This is the difference between a quantum superposition and a classical statistical mixture — the artifact makes the difference visible.

**Predicted beat periods (atomic units):**

| superposition | ω = \|E_A − E_B\| | T = 2π/ω |
|---|---:|---:|
| 1s + 2s | 0.375 | 16.76 |
| 1s + 2p | 0.375 | 16.76 |
| 2s + 2p (same n!) | 0 | ∞ (no beat — degenerate) |
| 1s + 3s | 0.444 | 14.14 |
| 2s + 3p | 0.0694 | 90.46 |

The 2s + 2p row is the **Stark-mixing case** — same n, so no temporal beat from energy difference, but the spatial structure is a genuinely different orbital (one of the sp hybrids) that is itself a stationary configuration. This is a particularly clean diagnostic: if you set up 2s + 2p and see no beating, that confirms the degeneracy in the hydrogen energy spectrum at fixed n.

---

## 2.5 Reveal — the one cataphatic toggle (ESS, not ESA)

Every toggle in §1–§2 is an **apophatic** operation: delete a constitutive factor, read what goes missing. Reveal is the exception. It strips nothing. It is a **cataphatic / ESS construction** — a visualization layer built *on top of* the unaltered residue to make a component that is already present, but perceptually weak, legible. It is the in-artifact instance of the ESA → ESS reversal flagged in §6.

**The problem it answers.** Strip prediction E establishes that a single eigenstate is stationary in `|ψ|²`: the temporal factor e^(−iE_n t) has unit modulus, so it never touches the density, and the cloud sits still. That stillness is correct and is itself the diagnostic. But it leaves the temporal factor — the only time-dependent constitutive component of an eigenstate — with no visible expression at all. The phase clock is running; nothing on screen says so. arg(ψ) is mapped to hue, so the whole cloud's color rotates uniformly at rate E_n, but a rigid, uniform hue rotation across an unstructured cloud is below the threshold of casual perception. The residue is faithfully rendered and effectively invisible.

**The construction.** Reveal renders the temporal phase as its own propagating object. Surfaces of constant arg(ψ) sweep through the fixed cloud as the e^(−iE_n t) clock advances; a point flares with its true phase hue as a front reaches it (saturation spikes sharply, then decays fast), while its brightness lingers and decays slower — a colored crest trailing a luminous wake. The asymmetry (saturation fades faster than brightness) gives the front an implied direction of travel.

**Identity criterion (why this is faithful, not decorative).** The construction is admissible only if it adds nothing to what is. The criterion it satisfies is **time-average output-equivalence**: integrated over one phase cycle, each point's emitted brightness equals its `|ψ|`. Reveal does not add net amplitude; it redistributes *when* each point announces its phase. Turn Reveal off and the time-averaged picture is unchanged — `|ψ|²` is stationary again, exactly as Strip prediction E requires. The layer re-times emphasis; it does not fabricate density.

**What it touches — nothing constitutive.** Reveal moves no sample points (substrate preserved, per §3) and alters no value of ψ (configuration preserved). It reads arg(ψ) and `|ψ|` and re-sequences emission. It sits strictly in the rendering pipeline — the same channel §3 identifies as the second substrate-level commitment — and strengthens that channel's fidelity to a residue already there.

**The three invariants (the observer-independence / "closed box" property).** The sweep is computed purely from world-space phase, θ_sweep(t) = −E_n·t; the camera never enters the computation. Three readouts therefore become directly visible and are fixed in space regardless of how the view is rotated or interacted with:

- **rate = |E_n|** — the heartbeat frequency. More tightly bound states beat faster (1s fastest). A `pace` multiplier scales this perceptually without altering the relative ordering; pace = 1 is the literal E_n.
- **arm count = |m|** — the constant-arg surfaces of an e^(imφ) state are |m| half-planes hinged on z, so |m| fronts circle the axis at once. The magnetic quantum number is read off directly.
- **handedness = sign(m)** — the circulation direction. Orbiting the camera flips the *view*; the circulation in *space* is invariant. This is the toggle's "Schrödinger's-cat" character: the observation frame is free, the underlying fact is not.

For m = 0 there is no azimuthal winding, so the fronts are radial shells pulsing outward rather than a rotating pinwheel — the visual statement that m = 0 carries no L_z. The contrast between the m = 0 pulse and the m ≠ 0 pinwheel is Strip prediction D's content made kinetic.

**Default state.** Off. The stationarity of `|ψ|²` in §1.4/E is the prior lesson and must be encounterable first; Reveal is the deliberate second move that shows what the stillness was concealing.

---

## 3. Substrate: what is and is not "the field"

ESA-aligned distinction (per §3 of ESM v1.0): substrate vs configuration.

- **Configuration** = ψ_nlm(r,θ,φ,t) at a particular `(n, l, m)`. This is what gets stripped. The "thing under analysis."
- **Substrate** = the complex-valued field over R³ × R that ψ inhabits. The locus the configuration lives on. It does not get stripped by ESA — it is preserved across all toggles.

The artifact respects this by maintaining a **fixed point cloud** of sample positions throughout the session. The positions are the substrate's discretization. They never change. What changes per frame is the *configuration* — what amplitude and phase live at each substrate point. Stripping the radial term doesn't move points; it changes what the points display. This is the technical instantiation of "the substrate is where, not what."

A second substrate-level commitment: the **rendering pipeline** preserves both amplitude and phase as independent channels into the output (opacity from |ψ|, hue from arg(ψ)). The Falstad-mode toggle is precisely the act of severing this — collapsing arg(ψ) to a constant so the output channel that carried it can be observed as missing. The substrate doesn't change; the rendering's faithfulness to the substrate does.

---

## 4. Source / literature check (ESA Step A-5)

The hydrogen wavefunction is the foundational solved problem of quantum mechanics. Standard references:

- Griffiths & Schroeter, *Introduction to Quantum Mechanics* (3rd ed., 2018), §4.2 — analytic derivation
- Sakurai & Napolitano, *Modern Quantum Mechanics* (2nd ed., 2017), §3.7 — operator/algebraic treatment
- NIST DLMF, §18.3 (Laguerre), §14.7 (Legendre), §14.30 (Spherical Harmonics) — special-function definitions matching this artifact's conventions

The visualization tradition that prompted this build:

- A. Falstad, "Hydrogen Atom Applet" (`falstad.com/qmatom/`) — the canonical 3D `|ψ|²` viewer; the apophatic baseline this artifact restores phase to
- Desmos calculator `nlupeegdgg` — 2D radial-function plotter, dimensional reduction; useful but loses 3D structure
- mysimulator.uk wave-packet sim — 1D TDSE animation, complementary to this artifact (handles wave packets where this one handles eigenstates)

No prior 3D viewer encountered in the search exposes the four constitutive components (R, Y_polar, e^(imφ), e^(−iEt)) as independently killable. This artifact is the apophatic-complete view: every multiplicative factor of the wavefunction is independently strippable, and the consequences of each strip are visible as both amplitude (cloud density) and phase (cloud color) changes.

---

## 5. Files

- `index.html` — single-file interactive (Three.js r0.160 via importmap, no build step). Open by double-click.
- `theory.md` — this file. Strip predictions in advance.
- `README.md` — quick-start and controls.

## 6. ESA inflection (Step A-6)

After stripping completes and the residue is read, the recursion test surfaces these candidate next moves:

- **Lateral** — Build the wave-packet artifact as the complementary substrate (TDSE in a 3D potential, packet bouncing/tunneling). The eigenstate artifact and the wave-packet artifact are the two halves of the apophatic vs cataphatic decomposition of "the quantum system": eigenstates ARE the stationary modes (necessary structure under H-eigenequation); wave packets are constructed superpositions that EVOLVE (sufficient structure under TDSE). Both should exist.
- **Deeper** — Add spin (Pauli structure on top of ψ_nlm), making the configuration a two-component spinor. Adds fine-structure splitting as a phase-rate diagnostic.
- **Reversal (ESA → ESS)** — Treat the residue from this ESA pass (the constitutive factorization R · Y · e^iEt) as the seed for an ESS construction: build a non-hydrogen substrate (e.g., harmonic oscillator) and ask whether the same factorization pattern emerges from a different separable potential. The construction tests whether R·Y·e^iEt is necessary (a property of the H atom) or sufficient (a property of separable spherical potentials generally).
