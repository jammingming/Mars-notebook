## Torrence and Compo (1998)
[A Practical Guide to Wavelet Analysis](https://paos.colorado.edu/research/wavelets/bams_79_01_0061.pdf)

### Wavelet Analysis
Assume that one has a time series, $x_n$ , with equal time spacing $δt$ and $n = 0 … N − 1$. Also assume that one has a wave let function, $ψ_0 (η)$
Morlet wavelet
$$ψ_0 (η) = \pi^{-1/4}\mathrm{e}^{i\omega_0\eta}\mathrm{e}^{-\eta^2/2}$$
$$\hat\psi_0(\omega)=\pi^{-1/4}H(\omega)\mathrm{e}^{-\frac{(\omega-\omega_0)^2}{2}}$$
![[Pasted image 20261015143828.png]]
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
For nonorthogonal wavelet analysis, one can use an arbitrary set of scales to build up a more complete picture. It is convenient to write the scales as fractional pow ers of two: $$ \begin{align*} s_j &= s_0\, 2^{j\delta_j}, \quad j=0,1,\dots,J \\ J &= \delta_j^{-1}\log_2\big(N\delta t / s_0\big), \end{align*} $$
$s_0$ denotes the smallest resolvable scale while J sets the largest scale; $s_0$ is selected such that its equivalent Fourier period is roughly $2\delta t$. The parameter $\delta_j$ controls the density of scale sampling and depends on the wavelet’s spectral width. For the Morlet wavelet, 0.5 is the maximum $\delta_j$ maintaining adequate scale sampling, and smaller $\delta_j$ yields finer scale resolution. Other wavelets can adopt larger $\delta_j$.

### Cone of influence
Finite-length time series cause edge errors in wavelet power spectra because FFT assumes periodic data. Zero-padding (used here) extends the series length to the next power of two to reduce edge effects and accelerate FFT, yet it creates artificial discontinuities and suppresses power near endpoints at large scales. The cone of influence (COI) marks the region where edge contamination matters, defined by the e-folding time where edge-induced power falls by \(e^{-2}\). Features inside the COI may be artifacts rather than real signals. The COI width depends on wavelet shape: narrower wavelets have smaller COI. COI also helps distinguish isolated noise spikes from genuine periodic oscillations. Periodic data require no zero-padding and have no COI.

