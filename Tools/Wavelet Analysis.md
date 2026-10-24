## Torrence and Compo (1998)
[A Practical Guide to Wavelet Analysis](https://paos.colorado.edu/research/wavelets/bams_79_01_0061.pdf)

### Wavelet Analysis
Assume that one has a time series, $x_n$ , with equal time spacing $δt$ and $n = 0 … N − 1$. Also assume that one has a wave let function, $ψ_0 (η)$
Morlet wavelet
$$ψ_0 (η) = \pi^{-1/4}\mathrm{e}^{i\omega_0\eta}\mathrm{e}^{-\eta^2/2}$$
$$\hat\psi_0(\omega)=\pi^{-1/4}H(\omega)\mathrm{e}^{-\frac{(\omega-\omega_0)^2}{2}}$$
![[Morlet_wavelet.png]]
continuous wavelet transform
$$
W_n(s) = \sum_{n'=0}^{N-1} X_{n'} \psi^{*}\left[ \frac{(n'-n)\delta t}{s} \right]
$$
we can do faster calculations in Fourier space
discrete Fourier transform (DFT)
$$
\hat{X}_k = \frac{1}{N}\sum_{n=0}^{N-1} X_n \mathrm{e}^{-2\pi \mathrm{i} k n / N}
$$
then
$$
W_n(s) = \sum_{k=0}^{N-1} \hat{X}_k \hat{\psi}^{*}(s\omega_k) \mathrm{e}^{\mathrm{i}\omega_k n\delta t}
$$
$$
\omega_k =
\begin{cases}
\dfrac{2\pi k}{N\delta t} & :\ k \le \dfrac{N}{2}\\[6pt]
-\dfrac{2\pi k}{N\delta t} & :\ k > \dfrac{N}{2}
\end{cases}
$$
the wavelet function at each scale s is normalized to have unit energy
$$
\hat{\psi}(s\omega_k) = \left( \frac{2\pi s}{\delta t} \right)^{1/2} \hat{\psi}_0(s\omega_k)
$$
$$
\int_{-\infty}^{+\infty} \left| \hat{\psi_0}(\omega') \right|^2 d\omega' = 1
$$
$$
\sum_{k=0}^{N-1} \left| \hat{\psi}(s\omega_k) \right|^2 = N
$$
Thus, the wavelet transform is weighted only by the amplitude of the Fourier coefficients $\hat{X}_K$ and not by the wavelet function

### Wavelet functions
==Wavelets can be categorized by four properties: orthogonality, complex/real type, width, and shape. Orthogonal wavelets yield compact signal representations but suffer from spectrum changes under time shifts, while nonorthogonal continuous wavelets (used here) have redundant, smoothly varying coefficients and are preferred for time series. Complex wavelets capture amplitude and phase of oscillations; real wavelets detect peaks or jumps. Wavelet width controls the time–frequency uncertainty tradeoff. Wavelet shape should match signal features, though for wavelet power spectra the choice often only weakly affects qualitative results.==

### Choice of scales
![[Table1_wavelet_properties.png]]
For nonorthogonal wavelet analysis, one can use an arbitrary set of scales to build up a more complete picture. It is convenient to write the scales as fractional pow ers of two: $$ \begin{align*} s_j &= s_0\, 2^{j\delta_j}, \quad j=0,1,\dots,J \\ J &= \delta_j^{-1}\log_2\big(N\delta t / s_0\big), \end{align*} $$
$s_0$ denotes the smallest resolvable scale while J sets the largest scale; $s_0$ is selected such that its equivalent Fourier period is roughly $2\delta t$. The parameter $\delta_j$ controls the density of scale sampling and depends on the wavelet’s spectral width. For the Morlet wavelet, 0.5 is the maximum $\delta_j$ maintaining adequate scale sampling, and smaller $\delta_j$ yields finer scale resolution. Other wavelets can adopt larger $\delta_j$.

### Cone of influence
Finite-length time series cause edge errors in wavelet power spectra because FFT assumes periodic data. Zero-padding (used here) extends the series length to the next power of two to reduce edge effects and accelerate FFT, yet it creates artificial discontinuities and suppresses power near endpoints at large scales. The cone of influence (COI) marks the region where edge contamination matters, defined by the e-folding time where edge-induced power falls by \(e^{-2}\). Features inside the COI may be artifacts rather than real signals. The COI width depends on wavelet shape: narrower wavelets have smaller COI. COI also helps distinguish isolated noise spikes from genuine periodic oscillations. Periodic data require no zero-padding and have no COI.

### Significance levels
#### Fourier red noise spectrum
A simple model for red noise is the univariate lag-1 autoregressive [AR(1), or Markov] process:
$$
X_n = \alpha X_{n-1} + Z_n
$$
where α is the assumed lag-1 autocorrelation, $X_0 = 0$, and zn is taken from Gaussian white noise. Following Gilman et al. (1963), the discrete Fourier power spectrum of (15), after normalizing, is
$$
P_k = \frac{1-\alpha^2}{1+\alpha^2-2\alpha\cos(2\pi k/N)}
$$
where k = 0 … N/2 is the frequency index. Note that α = 0 gives a white-noise spectrum

#### Significance levels
using a background noise null hypothesis. The null hypothesis assumes the time series follows a background spectrum (typically AR(1) red noise). A wavelet power peak is statistically significant at the 5% level (95% confidence) when its value exceeds the corresponding background threshold.

If the original time series is Gaussian, its Fourier coefficients have normally distributed real and imaginary parts. The squared magnitude of Fourier power $|\hat x_k|^2$ follows a $\chi^2_2$(chi-square with 2 degrees of freedom, DOF) distribution. The 95% confidence threshold is obtained by multiplying the background spectrum by the 95th percentile of $\chi^2_2$.

Wavelet coefficients act as band-pass filtered Fourier components. For **complex wavelets (e.g., Morlet)**, the local wavelet power $|W_n(s)|^2$ obeys the $\chi^2_2$ distribution. For **real-valued wavelets (e.g., Mexican hat)**, power follows $\chi^2_1$ (1 DOF). This statistical distribution is independent of the choice of mother wavelet except for the DOF difference between real and complex wavelets.

At each time n and scale s, compute the background power $P_k$ from the Fourier frequency matched to scale s, then construct the 95% significance contour. Smoothing the wavelet power in either time or scale domain increases effective DOFs and improves statistical confidence, and the related DOF and significance levels for smoothing are discussed later.

#### Smoothing in time and scale
![[Table2_wavelet_reconstruction_factors.png]]
##### Time averaging (global wavelet spectrum)
The time-averaged wavelet power over a time window from $n_1$ to $n_2$ is defined as
$$
\overline{W}^2(s)=\frac{1}{n_a}\sum_{n=n_1}^{n_2}|W_n(s)|^2 \tag{21}
$$
where $n_a=n_2-n_1+1$ is the count of averaged time points.

Averaging over the entire time series yields the **global wavelet spectrum**:
$$
\overline{W}^2(s)=\frac{1}{N}\sum_{n=0}^{N-1}|W_n(s)|^2 \tag{22}
$$
The global wavelet spectrum is an unbiased and consistent estimator of the true power spectrum and may be used as a background spectrum for testing local wavelet peaks. Fourier spectra converge toward the global wavelet spectrum after running-average smoothing, with less smoothing required for larger scales.

The confidence bound for wavelet power is
$$
\frac{|W_n(s)|^2}{2\sigma^2}P_k^{-1} \le \chi^2_\nu(p/2) \quad \text{and} \quad \frac{|W_n(s)|^2}{2\sigma^2}P_k^{-1} \ge \chi^2_{2-\nu}(1-p/2) \tag{20}
$$
where $p=0.05$ for the 95% confidence level. For real wavelets, the $\chi^2_1$ distribution replaces $\chi^2_2$, and the factor of 2 is removed.

Individual local wavelet power points are correlated in time and scale; temporal correlation extends for larger scales because the wavelet window broadens. The effective DOF is calibrated via Monte Carlo simulation:
$$
\nu=2\left(1+\frac{n_a\delta t}{\gamma s}\right) \tag{23}
$$
$\gamma$ is the empirical decorrelation factor listed in Table 2. For real-valued wavelets such as the Mexican hat, the prefactor $2$ is removed. If averaged points lie within the cone of influence (COI), $n_a$ is reduced to account for amplitude suppression and information loss. The time-averaged power follows $P_k \chi^2_\nu/\nu$, and the fitted chi-square model has relative error below 7% compared with Monte Carlo results.

A feature can be locally significant on the time–scale map even if it lacks significance in the global wavelet spectrum.

##### Scale averaging
Scale-averaged wavelet power, the weighted sum over a band of scales $s_1$ to $s_2$, is written:
$$
\overline{W}_n^2=\frac{\delta j \delta t}{C_\delta}\sum_{j=j_1}^{j_2}\frac{|W_n(s_j)|^2}{s_j} \tag{24}
$$
$C_\delta$ is the reconstruction factor from Table 2.

The averaged scale metric $S_{\text{avg}}$ is:
$$
S_{\text{avg}}=\frac{\sum_{j=j_1}^{j_2}1/s_j}{\sum_{j=j_1}^{j_2}1/s_j^2} \tag{25}
$$

The statistical relation for scale-averaged power is:
$$
\frac{C_\delta S_{\text{avg}}}{\delta j \delta t \sigma^2}\overline{W}_n^2 \Rightarrow P_{\text{avg}} \frac{\chi^2_\nu}{\nu} \tag{26}
$$
where the averaged background spectrum over the scale band is
$$
P_{\text{avg}}=\frac{\sum P_k(s_j)/s_j^2}{\sum_{j=j_1}^{j_2}1/s_j^2} \tag{27}
$$

The effective DOF for scale averaging:
$$
\nu=2\left(1+\frac{n_a S_{\text{avg}}}{\delta_{j0} S_{\text{mid}}}\right) \tag{28}
$$
$n_a=j_2-j_1+1$ is the number of averaged scales, $\delta_{j0}$ is the empirical scale decorrelation factor in Table 2, $S_{\text{mid}}=s_0 2^{0.5(j_1+j_2)\delta_j}$. Again the prefactor $2$ is dropped for real wavelets. This formula is valid only for confidence levels ≤95%; at higher confidence the chi-square approximation breaks down. The model matches Monte Carlo results with relative error less than 1.5%.

Scale-averaged power describes time-varying variance within a selected frequency band and can detect amplitude modulation of oscillations. The Niño3 SST and SOI example uses 2–8 yr band scale averaging. Since their AR(1) red-noise background spectra differ, their 95% confidence thresholds differ.

#### Cross-wavelet spectrum, wavelet coherence and phase
Given two time series $X$ and $Y$, their wavelet transforms are $W_n^X(s)$ and $W_n^Y(s)$. The cross-wavelet spectrum is defined as
$$
W_n^{XY}(s)=W_n^X(s)W_n^{Y*}(s)
$$
where $W_n^{Y*}(s)$ denotes the complex conjugate of $W_n^Y(s)$. The cross-wavelet spectrum is complex, and cross-wavelet power is its magnitude $|W_n^{XY}(s)|$.

Confidence levels for cross-wavelet power are derived from the square root of the product of two chi-square distributions. Assuming both individual wavelet power spectra follow chi-square distributions with $\nu$ degrees of freedom, the probability density function is
$$
f_\nu(z)=\frac{2^{2-\nu}}{\Gamma(\nu/2)} z^{\nu-1} K_0(z) \tag{30}
$$
where $z$ is the random variable, $\Gamma$ is the Gamma function, and $K_0(z)$ is the modified Bessel function of the zeroth order. The cumulative distribution function is
$$
p=\int_0^{Z_\nu(p)} f_\nu(z) dz
$$
$Z_\nu(p)$ represents the confidence level corresponding to probability $p$. For a given probability $p$, this integral can be inverted to solve for $Z_\nu(p)$.

If the two time series have theoretical Fourier background spectra $P_k^X$ and $P_k^Y$ (e.g., the AR(1) red-noise spectrum in Eq. (16)), the statistical distribution of cross-wavelet power satisfies
$$
\frac{|W_n^X(s)W_n^{Y*}(s)|}{\sigma_X\sigma_Y} \Rightarrow Z_\nu(p)\sqrt{P_k^X P_k^Y} \tag{31}
$$
where $\sigma_X$ and $\sigma_Y$ are the standard deviations of $X$ and $Y$, respectively.
- For $\nu=1$ (real-valued wavelets): $Z_1(95\%) = 2.182$
- For $\nu=2$ (complex wavelets): $Z_2(95\%) = 3.999$

The example uses Niño3 SST and SOI with the Paul ($m=4$) wavelet. The Paul wavelet has better time localization but poorer frequency localization than the Morlet wavelet. The cross-wavelet power reveals strong covariance between Niño3 SST and SOI across the 2–8 yr band. The 95% confidence contour is calculated from Eq. (31) using red-noise backgrounds with $\alpha=0.72$ for Niño3 SST and $\alpha=0.61$ for SOI.

##### Wavelet coherence and phase
Fourier coherence is the squared cross-spectrum normalized by individual power spectra, ranging from 0 to 1, measuring frequency-dependent cross-correlation. However, Liu (1994) showed that **unsmoothed wavelet coherence equals unity everywhere for all times and scales**, which is trivial and useless. Fourier analysis solves this by smoothing the cross-spectrum before normalization. For wavelet analysis, the optimal smoothing strategy (typically time-domain smoothing) is ambiguous; smoothing degrades time localization and weakens the core advantage of wavelet transform.

Liu (1994) proposed plotting co-spectrum, quadrature spectrum, and coherence phase. The coherence phase is defined as
$$
\phi=\tan^{-1}\big[\Im\{W_n^{XY}(s)\}/\Re\{W_n^{XY}(s)\}\big]
$$

For Niño3 SST and SOI, the phase difference stays within $160^\circ$–$200^\circ$ over the 2–8 yr period band, nearly $180^\circ$ out of phase ($\pm20^\circ$). This anti-phase feature is consistent with cross-wavelet power variations. During low-variance epochs such as 1920–1960, phase differences become more random.

