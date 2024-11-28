This file serves as a centralized repository for design notes, capturing insights, intermediate findings, and reminders throughout the project's design phase. It acts as a living document, evolving alongside the project's development.

> [!info]- Reverting simulation to earlier stages
> The design flow needs to be adjusted so that the polarizer outlet is aligned with the $z=0$ plane. This is because when defining an $E$-field probe in CST Studio Suite at a parametrized position, e.g., `(0,0,PolarizerLength)` if we started modelling from the polarizer input, the results from the probe don't update automatically with parameter change in template-based post-processing. By working backwards from the polarizer outlet to the feeding section, we can run verification simulations at each stage simply by defining a different excitation port and keeping the probe at `(0,0,0)`.
>
> However, this approach introduces other challenges, as having two ports at different locations within the cavity disrupts wave propagation. Consequently, the simulation step for each stage (labelled `x.1` for `Stage x`) becomes incompatible during later stages of the design process. Although backward verification by returning to earlier stages remains possible, this is done by keeping the previous simulation setups hidden in the history list. Reverting is achieved by un-hiding the relevant section and hiding all subsequent sections.
>
> > [!question] Maybe I can always just change the port to monitor-only?

# 1 Polarizer

This section details the modelling of the polarizer.

> [!question] Is it OK to evaluate the phase using only the $x$ component of the $E$-field probe?

- [x] *Select a frequency band*
	5 to 6 GHz, $f_{\mathrm{c}} = 5.5\ \mathrm{GHz} \sim \lambda_{\mathrm{c}} \approx 54.51\ \mathrm{mm}$
- [x] *Find a reference recommended frequency range among commercial polarizers*
    WR159
- [x] *Determine `polarizerSide` to match the lowest mode cutoff with the reference*
    50 mm
- [x] *Determine `chamferWidth`*
    24 mm (1.21 ratio, 67.43 deg lag on 100 mm length, 3.66 GHz cutoff)
- [x] *Determine `polarizerLength`*
    134 mm
- [x] *Put in all the numbers and verify by final polarizer simulation*

> [!note] Reference waveguide
> The side length of the polarizer is chosen to achieve a cutoff frequency that is similar to or lower than that of the reference commercial waveguide listed below. This ensures that the desired operating frequency band of 5-6 GHz aligns well with the recommended frequency range of the reference waveguide, which extends up to 7 GHz. Since the polarizer is excited by two modes with different cutoff frequencies, the higher cutoff frequency is used for comparison, providing an adequate margin between it and the lowest frequency of the operating band.
> - **Identification:** WR159
> - **Inner dimensions:** 40.39 x 20.19 mm
> - **Recommended frequency:** 4.90 to 7.05 GHz
> - **Cutoff frequency lowest mode:** 3.71 GHz
>
> > [!quote] Operating band - rule of thumb
> > [Waveguide Mathematics | Microwaves101](https://www.microwaves101.com/encyclopedias/waveguide-mathematics)
> > The accepted limits of operation for rectangular waveguides are (approximately) between 125% and 189% of the lower cutoff frequency. Thus for WR-90, the cutoff is 6.557 GHz, and the accepted band of operation is 8.2 to 12.4 GHz. Remember, at the lower cutoff, the guide simply stops working.

## 1.1 Cross-section tuning

The following section puts a feeding port to the input of the polarizer for tracking the polarizer's internal figures of merit, such as

- mode amplitude ratio (should be ~1),
- mode phase lag per unit length (should be as high as possible).

Both of these metrics are proportional to the `ChamferWidth` parameter since introducing a larger obstacle into the waveguide cavity leads to overall greater differences between the propagating modes. This creates the necessity of a trade-off during this design step. Striving to achieve mode amplitude equality also means yielding a very low mode phase lag per unit length. This requires the resulting polarizer to be impractically long.

> [!warning] Lowest mode cutoff
> Increasing the `chamferWidth` will also raise the cutoff frequency of the lowest propagating mode. This adjustment may push the design outside the recommended frequency range for the reference commercial waveguide. Therefore, when we need to increase the `chamferWidth`, it's crucial to verify the lowest mode cutoff frequency. To ensure that the cutoff frequency is within acceptable limits, consider returning to the initial design stage and increasing the `polarizerSide`.

Assuming that the mode phase lag per unit length is roughly length-invariant, we can set up a post-processing calculation to determine `polarizerLength`.

> [!note] Mode phase lag per unit length
> The assumption that the mode phase lag per unit length remains constant is likely an oversimplification which helps with rough calculations. It seems more likely that the propagation constants of individual modes change as the wave travels down the polarizer, causing the second derivative of mode phase lag with respect to length to be non-zero, i.e. mode phase lag per unit length not to be length-invariant.
>
> > [!success] It seems to be pretty much a negligible error!

# 2 Waveguide feed

The following is a validation stage where the polarizer is fed by an ordinary TE10 or TE01 wave guided by a square waveguide. This waveguide is just a short section flared open at the input end which facilitates an easy possibility to provide a linear polarization at the polarizer's input and verify the tuning performed in the previous step.

## 2.1 Waveguide feed simulation

The feeding waveguide is excited by an ordinary waveguide port with two modes at the input. Thanks to the square, i.e., symmetrical, cross-section of the waveguide, the two excited modes (TE10 and TE01) propagate with identical wave vectors $\boldsymbol{k}$ and cutoff frequencies. These two modes differ only in their field orientation, one propagating with a vertical electric field and the other horizontal. Each of them should produce LHCP and RHCP polarizations at the polarizer's output, respectively, or vice versa.

> [!info]- Theoretical explanation
> The previously tracked metrics now serve as a secondary evaluation method. By defining an $E$-field monitor at the centre frequency, we can observe how the time variations of the electric field on a cutting plane perpendicular to the $k$-axis evolve as the value of $k$ increases, i.e., as we move further into the polarizer. Initially, at the input, the field exhibits pulsating behaviour corresponding to a single sine wave, but at the output, it transitions to a pattern where the pulses cease, and the electric field vectors trace circular paths. This pattern evolution corresponds to the core concept that the polarizer "obstructs" the incident wave propagation via the triangular prisms, breaking its linear polarization into two modes, each with different propagation properties, namely the phase constant. At the polarizer output, the two modes are of close-to-equal magnitudes and are mutually delayed in phase by $\pi/4$.

# 3 Open-ended radiation

Since the polarizer alone does not efficiently radiate outward from its structure, it is necessary to incorporate an additional square outlet to guide the circularly polarized wave. This modification allows for the evaluation of far-field properties of the emitted wave, such as the radiation pattern and axial ratio.

> [!note] Outlet length
> A parameter sweep of `outletLength` from 30 mm to 100 mm, in 10 mm increments, demonstrated that variations in far-field properties are negligible. As a result, further optimization of this parameter is unnecessary. However, including at least the 30 mm square waveguide section is recommended, as the difference between having no outlet and a 30 mm outlet is the only significant impact observed.

# 4 Dual feed

This stage is very complex as it concerns designing a dual feeding structure for realistic excitation of the propagation modes. Each mode introduced into the polarizer via the waveguide feed added during the previous stages must be excited by a coaxial probe protruding into the waveguide cavity through a side wall. The common approach to achieving a coax-to-waveguide transition is the **right-angle transition**.

The design principally follows the work of [Karki et al.](https://ieeexplore.ieee.org/document/9933815), which tackles the same issue by displacing the two probes a certain distance along the propagation direction and using a grating of wires parallel to the second probe. This serves as its back-short while having very little influence on the wave propagation from Probe 1 and helping with crosstalk minimization.

- S. K. Karki, J. Ala-Laurinaho and V. Viikari, "Dual-Polarized Probe for Planar Near-Field Measurement," in *IEEE Antennas and Wireless Propagation Letters*, vol. 22, no. 3, pp. 576-580, March 2023, doi: 10.1109/LAWP.2022.3218731.

As is the common practice, the specific values stated in the paper are most likely scrambled to protect the design's uniqueness. It is apparent when double-checking the values with devised theoretical guidelines laid out of the design process description. The table below serves as a reference to the values mentioned by the authors and their alleged logical origin (where applicable).

| Parameter          | Value in paper     | Guideline                  | Theoretical value | Comments |
| ------------------ | ------------------ | -------------------------- | ----------------- | -------- |
| Lower cutoff       | 10.3 GHz           |                            |                   |          |
| Higher cutoff      | 14.6 GHz           |                            |                   |          |
| Center frequency   | 12.45 GHz          |                            |                   |          |
| Center wavelength  | 24.08 mm           |                            |                   |          |
| Guide wavelength   | 42.89 mm           |                            |                   |          |
| Probe {1,2} length | 4.5 mm             | $3\lambda_0/16$            | 4.51 mm           | OK       |
| Probe 1 distance   | 8.6 mm             | $< \lambda_{\mathrm{g}}/4$ | 10.72 mm          | OK       |
| Probe 2 distance   | 8.8 mm             | $< \lambda_{\mathrm{g}}/4$ | 10.72 mm          | OK       |
| Grating distance   | 27 mm              | $3\lambda_{\mathrm{g}}/4$  | 18.06 mm          | NOK      |
| Grating gap        | 2.6 mm             |                            |                   |          |
| Wire dimensions    | 1 $\times$ 1.25 mm |                            |                   |          |

> [!warning] I am assuming that when the authors mention $\lambda$, they mean $\lambda_{\mathrm{g}}$.

## 4.1 Single feed

> [!question] Air or PTFE coax in the waveguide drilling hole?
> So far, I have modelled the coax transition so that the PTFE reaches into the drilling hole, meaning that, in order to preserve $50\ \Omega$ impedance, the drilling hole diameter is basically the same as the diameter of the SMA coax outer conductor.

The first step is a textbook practise on designing a simple right-angle transition using the general guidelines taken from the website Microwaves101 and noted below.

> [!quote]- Right-angle transition (rectangular waveguide)
> [Waveguide to coax transitions | Microwaves101](https://www.microwaves101.com/encyclopedias/waveguide-to-coax-transitions)
> These are also known as $E$-plane transitions or orthogonal transitions. The waveguide is interfaced with a coaxial cable by using a simple antenna probe reaching a certain height into the waveguide to excite the preferred TE01 waveguide mode. A "back-short" is positioned some distance away from the probe. It reflects EM energy that was propagating the wrong way back toward the probe where it combines in-phase with the incident wave. Thus the probe sets up a time-varying electric field, which is constrained to propagate down the guide.
>
> > [!note] Coax to antenna probe transition
> > While the outer conductor and insulation of the feeding coax are terminated at the waveguide's outer wall, the inner conductor extends through a drilled hole, forming a section of air-filled coaxial line with a length equal to the thickness of the waveguide's wall. To minimize impedance discontinuities and secure the signal integrity as it propagates to the antenna probe, it is important to select the radius of the drilled hole, which acts as the outer conductor for this section so that the air-filled coax has an impedance close to $50\ \Omega$.
> >
> > This also means that, when stripping the feeding coax of its outer conductor and dielectric, it is necessary to account for the air-filled coax length by removing a total length of `waveguideThickness` + `probe1Length`.
>
> The probe's **distance from the back-short** is usually somewhat smaller than a quarter of a guide wavelength at center frequency. In free space (outside the waveguide), the distance to the backshort would be more than a quarter wavelength but all analyses of waveguides should be done from inside the guide, not looking at it from outside. Note that the wavefront in the guide appears to move faster than the speed of light, which is why the guide wavelength is more than in free space (remember, $v = f \cdot \lambda$).
>
> > [!quote] Guide wavelength $\lambda_{\mathrm{g}}$
> > [Waveguide Mathematics | Microwaves101](https://www.microwaves101.com/encyclopedias/waveguide-mathematics)
> > Guide wavelength is defined as the distance between two equal-phase planes along the waveguide. The guide wavelength is a function of the operating wavelength $\lambda_0$ (or frequency $f_0$) and the lower cutoff wavelength $\lambda_{\mathrm{cutoff}}$ and is always longer than the wavelength would be in free space. Here's the equation for guide wavelength:
> >
> > $$\lambda_{\mathrm{g}} = \dfrac{\lambda_0}{\sqrt{1-\left(\dfrac{\lambda_0}{\lambda_{\mathrm{cutoff}}}\right)^2}},$$
> >
> > where $\lambda_{\mathrm{cutoff}} = 2A$ generally, $A$ is `polarizerSide`.
> >
> > For $f_0 = 5.5\ \mathrm{GHz}$ ($\sim \lambda_0 \approx 54.51\ \mathrm{mm}$) and $A = 50\ \mathrm{mm}$,
> >
> > $$\lambda_{\mathrm{g}} = \dfrac{54.51}{\sqrt{1-\left(\dfrac{54.51}{2\cdot 50}\right)^2}} \approx 65.02\ \mathrm{mm}.$$
>
> The coax is typically $50\ \Omega$ impedance, whereas the waveguide might be $200\ \Omega$. Actually, the impedance of a waveguide is a complicated subject, there are three ways to calculate it, and it is a strong function of frequency. Therefore, some tuning must be applied. The tuning can be in the form of screws, or position of the probe, or the distance to the back short.
>
> What about the **length of the probe**, compared to the wavelength? The lower TE01 cutoff of a guide occurs when the broad dimension is half-wavelength in free space. At the centre of the band, the broad dimension is 3/4 $\lambda$, and the narrow dimension is (typically) 3/8 $\lambda$. The probe is typically 1/2 the narrow dimension in length, or 3/16 $\lambda$ at centre frequency. However, this is also a parameter that can be varied to optimize a design, along with the diameter of the probe and whether it retains a dielectric jacket or is bare.
>
> > [!warning] Probe radius tuning
> > The inner conductor of the air-filled coax is assumed to maintain the same diameter as the inner conductor of the preceding coaxial cable. To avoid impedance discontinuities, if adjustments to the probe's radius are needed during manufacturing (e.g., using a lathe), it is crucial to preserve the radius of this section. This can be achieved by marking the distance corresponding to the thickness of the waveguide after the insulation has been removed.

### Boundary consideration

During simulation, special attention must be paid to the boundary settings, specifically at the open end of the feeding structure. Considering that the structure will be directly connected to a waveguide polarizer, we can anticipate a small reflection to be introduced at the outlet when put together. This reflection, however, is not a subject of feed tuning and will be dealt with later during the final optimization. Therefore, the open boundary should not be modelled by the addition of computational space ("open (add space)" boundary in CST). This would act as an open end which introduces great reflection and therefore deteriorates the simulation results. The open boundary should be modelled using a non-reflective wall, i.e., the "open" boundary in CST.

### Effects of individual parameters

The following notes down intermediate simulation results from different sweeps and conclusions on how the individual parameters influence the probe's reflection.

1. Probe length
	- `probe1Distance`: 16 mm
	- `probe1Length`: {8, 10, 12, 14, 16} mm
	- `waveguideLength`: 100 mm
	- **S-parameters:** Certain values cause resonances to emerge, causing dips
2. Probe distance
	- `probe1Distance`: {10, 12, 14, 16, 18, 20} mm
	- `probe1Length`: 10 mm
	- `waveguideLength`: 100 mm
	- **S-parameters:** Non-uniform creation of a resonance above 6 GHz

### Optimization

The table below shows the theoretical values of the single feed parameters expressed as fractions of wavelengths along with the values obtained by optimizing with the goal of minimizing reflection in the design frequency band. The initial values for optimization were set to the theoretical ones.

| Parameter        | Guideline                  | Value                  | Optimized value      |
| ---------------- | -------------------------- | ---------------------- | -------------------- |
| `probe1Distance` | $< \lambda_{\mathrm{g}}/4$ | $< 16.25\ \mathrm{mm}$ | $14.53\ \mathrm{mm}$ |
| `probe1Length`   | $\approx 3\lambda_0/16$    | $10.22\ \mathrm{mm}$   | $11.67\ \mathrm{mm}$ |

> [!info] Comments on deviations from the guidelines
> The deviation of `probe1Length` from its guideline value is relatively significant. However, this specific guideline was taken with a grain of salt from the beginning since it was vaguely derived from the standard geometry of rectangular waveguides.

### Results

- Achieved reflection: $\forall f \in (5\ \mathrm{GHz}, 6\ \mathrm{GHz}): |S_{11}| < -15\ \mathrm{dB}$.

## 4.2 Grating

The next step, which can still be considered isolated from the second probe design, is the addition of grating. This section aims to draw a prototype wire grating to optimise its distance from Probe 1 with respect to $|S_{11}|$, i.e., the reflection when feeding from Port 1. Such an approach should yield a reduction of variables for the final, combined optimization as this parameter should only affect Probe 1 as the grating ideally acts simply as a shorting wall for Probe 2. Radii and gaps in the grating are subject to the combined optimization as its parameters will be set to produce a tradeoff between minimizing Probe 2 reflection and mutual crosstalk between probes.

### Reflection vs. crosstalk minimization tradeoff

As Karki et al. describe in their paper, the progression of $|S_{11}|$ with increasing separation distance between Port 1 and the grating is as follows: Reflection $|S_{11}|$

- is high when the grating polarizer is places close to Port 1, $s < \lambda_{\mathrm{g}}/4$;
- decreases with increasing $s$ and reaches a minimum at $s \approx \lambda_{\mathrm{g}}/4$;
- increases with increasing $s$ and reaches a maximum at $s \approx \lambda_{\mathrm{g}}/2$;
- has the next minimum when $s \approx 3\lambda_{\mathrm{g}}/4$.

Although it might seem compelling to conclude that $s \approx \lambda_{\mathrm{g}}/4$ is the best candidate with respect to the low reflection and reasonable size (i.e., fabrication cost) there is another effect of varying $s$ on the feed's performance which is also the core idea of the feeding probes' displacement: minimizing crosstalk $|S_{21}|$. The mutual probe coupling decreases inversely proportional to their distance, ergo to the distance between Port 1 and the grating as well. However, the effect of diminishing $|S_{21}|$ with increasing distance begins to sature around distance somewhat larger than $s = \lambda_{\mathrm{g}}/2$, resulting in the second reflection minimum at $3\lambda_{\mathrm{g}}/4$ being a reasonable tradeoff between minimizing both $|S_{11}|$ and $|S_{21}|$.

| Parameter         | Guideline                 | Theoretical value    | Optimal value        |
| ----------------- | ------------------------- | -------------------- | -------------------- |
| `gratingDistance` | $3\lambda_{\mathrm{g}}/4$ | $48.76\ \mathrm{mm}$ | $51.52\ \mathrm{mm}$ |

### Results

In terms of reflection coefficient, the addition of grating seems to dampen the dips in the design band and introduce new resonant peaks which must be pushed outside the band.

- Achieved reflection: $\forall f \in (5\ \mathrm{GHz}, 6\ \mathrm{GHz}): |S_{11}| < -12\ \mathrm{dB}$

## 4.3 Second feed

In theory, the introduction of Probe 2 into the cavity doesn't affect Probe 1 radiation. At the same time, Probe 2 is not expected to by affected by `gratingDistance` and its geometry should be very similar. Although these are hypotheticals, these logically sound expectations yield a very good initial value estimates for the final optimization.

However, tuning ofthe grating mesh parameters `gratingGap` and `wireDiameter` affects the entire S-matrix (both reflections and crosstalk). There parameters should be chosen by an optimization process performing a trade-off between low $|S_{11}|$ and $|S_{21}|$.

| Parameter         | Optimal value           |
| ----------------- | ----------------------- |
| `probe1Distance`  | $14.06801\ \mathrm{mm}$ |
| `probe1Length`    | $12.19318\ \mathrm{mm}$ |
| `probe2Distance`  | $14.30893\ \mathrm{mm}$ |
| `probe2Length`    | $11.96697\ \mathrm{mm}$ |
| `gratingDistance` | $51.5183\ \mathrm{mm}$  |
| `gratingGap`      | $1.90274\ \mathrm{mm}$  |
| `wireDiameter`    | $1.17699\ \mathrm{mm}$  |

### Results

- Probe 1 reflection: $\forall f \in (5\ \mathrm{GHz}, 6\ \mathrm{GHz}): |S_{11}| < -10.5\ \mathrm{dB}$
- Probe 2 reflection: $\forall f \in (5\ \mathrm{GHz}, 6\ \mathrm{GHz}): |S_{22}| < -14.6\ \mathrm{dB}$
- Probe coupling: $\forall f \in (5\ \mathrm{GHz}, 6\ \mathrm{GHz}): |S_{21}| < -70\ \mathrm{dB}$

# 5 Horn antenna

**Target gain:** 15 dBi
The design of a horn antenna is a simple and well-documented step of the design. The most recent and relevant review of theory behind the design of an optimal conical horn antenna was done in 2013 by [Aboserwal et al.](https://ieeexplore.ieee.org/document/6492234) In this paper, authors give an extensive look into the problematic of aperture phase distribution and accurate modelling given desired optimal gain.

- N. A. Aboserwal, C. A. Balanis and C. R. Birtcher, "Conical Horn: Gain and Amplitude Patterns," in *IEEE Transactions on Antennas and Propagation*, vol. 61, no. 7, pp. 3427-3433, July 2013, doi: 10.1109/TAP.2013.2256453.

However, for practical purposes, the tool Antenna Magus was utilized. This design software gives very quick and convenient parametric results for all standard antennas, most likely following design guidelines such as those given in scientific publications, and provides the designer with the ability to "tweak" certain parameters to suite the design requirements. Nevertheless, this tweaking ability of Antenna Magus is restricted by the data set used for the training of neural networks which perform these adjustments. Since this design uses a non-standard geometry (transitioning from a square waveguide to a conical antenna) with an abnormally wide waveguide, it is necessary to take the parametric result from Antenna Magus as a mere reference which is then to be adopted in the actual geometry. *Waveguide-fed conical horn antenna* design parameters from Antenna Magus are:

- Waveguide diameter `Dg = 41.64 mm` (maximum for tweaking)
- Waveguide length `Lg = 54.51 mm`
- Flare diameter `Df = 134.8 mm`
- Flare length `Lf = 68.65`

These parameters were then used to manually model the conical antenna with a square waveguide feed with the exception of setting `Dg = 1.33 * waveguideSide = 66.5 mm`.

### Effects of individual parameters

For this design, the following parameter sweeps have been simulated to determine the impact their alteration effects.

1. Flare diameter:
	- `flareLength`: 70 mm
	- `flareDiameter`: {115, 120, 125, 130, 135, 140, 145} mm
	- **Gain:** The sweet spot seems to be at the value of 130 mm
		- This corresponds to the flare angle of $\alpha \approx 27.67\ \mathrm{deg}$
2. Flare length
	- `flareLength`: {60, 65, 70, 75, 80, 85, 90, 95, 100, 105, 110} mm
	- `flareDiameter`: 135 mm
	- **Gain:** Continuously proportional increase without saturation.

### Results

The target gain can be achieved by various parameter combinations while for each value of flare length, the gain-to-flare-diameter dependence is a concave function with a well-defined local maximum - for illustration, refer to Figure 3 in Aboserwal et al. This maximum might correspond to the preservation of specific angle but that was not proven by the paper.

The decision is then a trade-off between comfortable achievement of the target gain and minimizing the antenna size. Two favourable combinations (both corresponding to $\alpha \approx 27.67\degree$) were found:

1. `feedLength = 60`, `feedDiameter = 120` yields $G \approx 15\ \mathrm{dBi}$,
2. `feedLength = 70`, `feedDiameter = 130` yields $G = 15.8\ \mathrm{dBi}$,

while the second (more generous) one was chosen for the final design to leave some gain margin.

# 6 Entire structure

First simulation attempts after simply putting everything together:

- $|S_{11}| < -\xi\ \mathrm{dB}$
- $|S_{22}| < -\xi\ \mathrm{dB}$
- $|S_{21}| < -\xi\ \mathrm{dB}$
- $G = \xi\ \mathrm{dBi}$
