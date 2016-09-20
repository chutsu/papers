# LTI Systems

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
