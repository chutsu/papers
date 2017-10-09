# Quadrotor


## LTI Systems

Linear time-invariant theory, commonly known as LTI system theory, comes from
applied mathematics and has direct applications in NMR spectroscopy,
seismology, circuits, signal processing, control theory, and other technical
areas. It investigates the response of a linear and time-invariant system to an
arbitrary input signal. Trajectories of these systems are commonly measured and
tracked as they move through time (e.g., an acoustic waveform), but in
applications like image processing and field theory, the LTI systems also have
trajectories in spatial dimensions. Thus, these systems are also called linear
translation-invariant to give the theory the most general reach.

LTI models can be expressed in the standard state equation form:

\begin{equation}
    \dot{x} = Ax + Bu
\end{equation}

\begin{equation}
    y = Cx + Du
\end{equation}


Control design objectives:
- Stability
- Tracking
- Robustness


Three cases
- Asymptotically Stable
- Unstable
- Critically Stable (does not blow up, but does not go to zero either)

Using eigen-values on the transition matrix A, it informs us how it "acts" in
different directions (eigen-vectors).

## Quadrotor Model

\begin{align}
    x &= \begin{bmatrix}
        x_{1}
        & x_{2}
        & x_{3}
        & x_{4}
        & x_{5}
        & x_{6}
        & x_{7}
        & x_{8}
        & x_{9}
        & x_{10}
        & x_{11}
        & x_{12}
    \end{bmatrix}^{T} \\
    x &= \begin{bmatrix}
        \eta
        & \omega
        & \varepsilon
        & \dot{\varepsilon}
    \end{bmatrix}^{T} \\
    \eta &= \begin{bmatrix} \psi & \theta & \phi \end{bmatrix}^{T} \\
    \omega &= \begin{bmatrix} p & q & r \end{bmatrix}^{T} \\
    \varepsilon &= \begin{bmatrix} x_{I} & y_{I} & z_{I} \end{bmatrix}^{T}
\end{align}

\begin{align}
    \dot{x_{1}} &= x_{4} + x_{5} S_{1} T_{2} + x_{6} C_{1} T_{2} \\
    \dot{x_{2}} &= x_{5} C_{1} - x_{6} S_{1} \\
    \dot{x_{3}} &= sec(x_{2}) (x_{5} S_{1} + x_{6} C_{1}) \\
    \dot{x_{4}} &= -((I_{z} - I_{y} / I_{x}) x_{5} x_{6} -
        (k_{\tau} x_{4} / I_{x}) + (1 / I_{x}) \tau_{p} \\
    \dot{x_{5}} &= -((I_{x} - I_{z} / I_{y}) x_{4} x_{6} -
        (k_{\tau} x_{5} / I_{y}) + (1 / I_{y}) \tau_{q} \\
    \dot{x_{6}} &= -((I_{y} - I_{x} / I_{z}) x_{4} x_{5} -
        (k_{\tau} x_{6} / I_{z}) + (1 / I_{z}) \tau_{r} \\
    \dot{x_{7}} &= x_{10} \\
    \dot{x_{8}} &= x_{11} \\
    \dot{x_{9}} &= x_{12} \\
    \dot{x_{10}} &= (-k_{t} x_{10} / m) +
        (1 / m)(C_{1} S_{2} C_{3} + S_{1} S_{3}) \tau_{f} \\
    \dot{x_{11}} &= (-k_{t} x_{11} / m) +
        (1 / m)(C_{1} S_{2} S_{3} - S_{1} C_{3}) \tau_{f} \\
    \dot{x_{12}} &= (-k_{t} x_{12} / m) +
        (1 / m)(C_{1} C_{2}) \tau_{f} - g
\end{align}

\begin{align}
    T_{i} &= tan(x_{i}) \\
    S_{i} &= sin(x_{i}) \\
    C_{i} &= cos(x_{i})
\end{align}

\begin{align}
    \begin{bmatrix}
        \tau_{f} \\
        \tau_{p} \\
        \tau_{q} \\
        \tau_{r} \\
    \end{bmatrix}
    =
    \begin{bmatrix}
        1 & 1 & 1 & 1 \\
        0 & -l & 0 & l \\
        -l & 0 & l & 0 \\
        -d & d & -d & d
    \end{bmatrix}
    \begin{bmatrix}
        f_{1} \\
        f_{2} \\
        f_{3} \\
        f_{4}
    \end{bmatrix}
\end{align}
