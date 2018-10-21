# LSD-SLAM: Large-Scale Direct Monocular SLAM

- Direct (feature-less) monocular SLAM algorithm
- Allows for large-scale consistent maps
- Obtained by filtering over a large number of pixelwise small-base line stereo
  comparisons
- Direct tracking method explicitly detects scale drifts
- Probabilistic solution to include effect of noisy depth values into tracking
- build large scale consistent maps in real time
- Novel direct tracking method that operates on sim(3), thereby
explicitly detecting scale drift
- Probabilistic solution to include effect of noisy depth values into
tracking


Monocular slam suffers from inherent scale ambiguity, where the scale of the
world cannot be observed with drifts over time, this is a major source of
error.

The advantage is that a monocular slam allows us to seamlessly switch between
differently scaled environments, such as indoors and outdoors.

Scaled sensor on the other hand, such as depth or stereo cameras have a limited
range at which they can provide reliable measurements.

A 3D similarity transform $S \in Sim(3)$ denotes rotation, scaling and
translation.

\begin{equation}
  S = \begin{bmatrix}
    s R & t \\
    0 & 1
  \end{bmatrix}
  \quad \text{with} \quad
  R \in SO(3),
  t \in R^{3},
  \quad \text{and} \quad
  R^{+}
\end{equation}


scale-ambiguity: the scale of the world cannot be observed and drifts over
time, being one of the major error sources.


LSD-SLAM:
- Not only tracks the motion of the camera locally (video odometry), but it
  also builds a consistent, large-sacle map of the environment
- Uses direct image alignemtn coupled with filtering based estimation of
  semi-dense depth maps
- Global map is represented as a pose graph consisting of keyframes as vertices
  with 3D similarity transforms as edges, elegantly incorporating changing
  scale of the environment and allowing to detect and correct accumulated
  drift
- Runs real time on CPU

## The complete method
The algorithm consists of three major components

1. tracking
2. depth map estimation
3. map optimization

- The **tracking** component continuously tracks new camera images. That is it
  estimates their rigid body pose with respect to the current keyframe, using
  the pose of the previous frame as initialization.
- The **depth map estimation** component uses tracked frames to either refine
  or replace the current keyframe. Depth is refined by filtering over many
  per-pixel, small-baseline stereo comparisons coupled with interleaved spatial
  regularization. If the camera has moved too far, a new keyframe is
  initialized by projecting points from existing, close by keyframes into it.
- Once a keyframe is replaced as tracking reference, and hence its depth map
  will not be refined further - it is incorporated into the global map by map
  optimization component. To detect loop closures and scale drift, a similarity
  transform to close by existing keyframes is estimated using scale aware,
  direct image alignment.


## Map representation

The map is represented as a pose graph of keyframe, each keyframe consists of:

- camera image
- inverse depth map
- variance of the inverse depth

Note that the depth map and variance are only defined for a subset of pixels,
containing all image regions in the vicinity of sufficiently large intensity
gradient, hence **semi-dense**.


## Depth Map Estimation

**Keyframe Selection**: if the camera moves too far away from the existing map, a new keyframe is created from the most recent tracked image. A threshold is applied to a weighted combination of relative distance and angle to the current frame:

\begin{equation}
  \text{dist}(\eta_{ji}) = \eta_{ji}^{T} W \eta_{ji}
\end{equation}

**Depth Map Creation**: once a new frame is chosen to become a keyframe, its
depth map is initialized by projecting points from the previous keyframe into
it, followed by one iteration of spatial regularization and outlier removal as
proposed.

**Depth Map Refinement**: tracked frames that do not become a keyframe are used
to refine the current keyframe: a high number of very efficient small baseline
stereo comparisons is performed for image regions where the expected stereo
accuracy is sufficiently large.


## Constraint Acquisition: Direct sim(3) Image Alignment

**Direct Image Alignment on sim(3)**. Monocular SLAM is in contrast to RGB-D or
Stereo-SLAM inherently scale ambivalent, i.e. the absolute scale of the world
is not observable. Over long trajectories this leads to scale drift, which is
one of the major sources of error. Further, all distances are only defined up
to scale, which causes threshold-based outlier rejection or parametrized robust
kernels (e.g Huber) to be ill defined).

Solved this problem by using the inherent correlation between scene depth and
tracking accuracy, where the depth map of each created keyframe is scaled such
that the mean inverse depth is one. In return edges between keyframe are
estimated as elements of sim(3), incorporating the scaling difference between
keyframe, and, in particular for large loop-closures, allowing an explicit
detection of accumulated scale drift.

LSD-SLAM proposes a novel method to perform **direct**, **scale-drift** aware
image alignment on sim(3), which is used to align two differently scaled
keyframe. In addition to the photometric residual $r_p$, we incorporate a depth
residual $r_d$ which penalizes deviations in inverse depth between keyframe,
allowing one to directly estimate the scaled transformation between them.


**Constraint Search**: After a new key frame is added to the map, a number of
possible loop closure keyframe is collected: We use the closest ten keyframe,
as well as a suitable candidate proposed by an appearance-based mapping
algorithm to detect large-scale loop closures. To avoid insertion of false or
falsely tracked loop closures a reciprocal tracking check is performed, where
each candidate keyframe we independently track $\eta_{jki}$ and $\eta_{ijk}$.
Only if the two estimates are statistically similar are they added to the
global map.

**Convergence Radius for sim(3) Tracking**: An important limitation of direct
image alignment lies in the inherent non-convexity of the problem, and hence
the need for a sufficiently accurate initialization. While for the tracking of
new camera frames a sufficiently good initialization is available (given by the
pose of the previous frame), this is not the case when finding loop closure
constraints, in particular for large loop closures.

One solution for this consists in using a very small number of key points to
compute a better initialization: Using the depth values from the existing
inverse depth maps, this requires aligning two sets of 3D points with known
correspondences, which can be done efficiently in closed form using e.g. the
method of Horn.
