# MLE Identification of Inertial Sensor noise Model Parameters

Accurate visual-inertial localization and mapping systems require accurate
calibration and good sensor error models. To this end, this paper presents
a simple offline method to automatically determine the parameters of
inertial sensor noise models.

A good sensor model often incorporates stochastic as well as deterministic
components. The stochastic part of the model includes errors such as
broadband noise, or a slowly (randomly) varying bias, and describes them
in a probabilistic sense. These errors cannot be calibrated for , and
limit the performance of the device fundamentally.

This paper emphasizes the importance of a good deterministic sensor
model, since a stochastic model alone usually leads to over-confidence in
the inertial data, especially when using uncalibrated sensors.



## Existing Methods

- Allan Variance (AV)
- Non-Parametric Power Spectral Density (PSD)

In practice the noise model parameters are usually determined manually by
"fitting" noise processes to estimates of the AV or the PSD. These
procedures are reliable, particularly those based on he Allan variance.
Computing the sample based AV is standardized, and by the nature of the
AV, the fitted noise models will be valid across the desired noise
strength and time scales.

- Manual procedures require expertise to map sample based AV to parameters
of a model

- Generalized Method of Wavelet Moments (GMWM)
  - Wavelet basis ("wavelet variance")
  - Sensor measurements are decomposed across different time scales


### Automatic Noise Model Parameter Identification

A small change in the identification problem allows us to successfully apply
classical maximum likelihood estimation to identify the parameters of the noise
model. The method performs well for composite noise processes that span many
strength- and time scales, such as weak MEMS gyroscope bias fluctuations buried
in broadband noise.


### Deterministic Errors and Calibration

The deterministic part of the inertial sensor model captures non-random effects
such as scale factor errors, axes misalignment or frequency response. These
non-idealities can, to some extent, be calibrated for, which often requires
a per-device (factory) calibration across temperature, or on-line calibration
mechanisms.




## Stochastic Sensor Models

The stochastic part of the sensor model captures errors which are considered
random, and describes them in a probabilistic sense. Good examples of random
errors are wideband "electronic noise", originating from transducer and
amplifier stages or fluctuations in drive frequency that results in slowly
varying random gyroscopic bias errors in MEMS devices.


### Spectral Density and Allan Variance

- **Power Spectral Density**: The power spectral density is a standard tool for
  the description of stationary stochastic processes, and it is commonly used
  to characterize noise processes in gyroscopes and accelerometers.

  - Welch's method (non-parametric method)
  - Autoregressive model (AR) + solving Yule-Walker equation (parametric method)

  Such classical methods struggle to capture weak processes with long
  correlation times, when they are buried deeply in broadband noise
  processes.

- **Allan Variance**: AV addresses the above issues and is an important
  tool in practice.


### Standard Noise Model

We now describe the most commonly used inertial sensor noise models, this
model combines "white noise", capturing rapidly fluctuating noise
processes, with an exponentially time-correlated process that captures
slowly varying noise processes (biases).

\begin{equation}
  z = x + v
  \dot{x} = - \dfrac{1}{\tau_{b}} + w
\end{equation}

Where $z$ is the modelled noise process, corrupting rate and acceleration
measurements. $v$ represents the white noise component of the model. $x$ is the
slowly varying (bias) process with correlation time $\tau_b$, "driven" by the
white noise process $w$. $w$ and $v$ are independent, zero mean, white Gaussian
noise processes of strength $\sigma_{b}$ and $\sigma_{w}$. Formally:

\begin{equation}
  E[ w(t_{1}) w(t_{2}) ] = \sigma_{b}^{2} \delta (t_{1} - t_{2}) \\
  E[ v(t_{1}) v(t_{2}) ] = \sigma_{b}^{2} \delta (t_{1} - t_{2})
\end{equation}

The model is populaar due to its relatively large expressive power despite the
small number of parameters. It can be incorporated into state estimation frameworks efficiently due to its Gauss-Markov properties.

Used to model gyro and accel in GPS aided INS or in the context of a sun-sensor aided EKF attitude estimator.

