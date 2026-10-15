## Torrence and Compo (1998)
[A Practical Guide to Wavelet Analysis](https://paos.colorado.edu/research/wavelets/bams_79_01_0061.pdf)


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