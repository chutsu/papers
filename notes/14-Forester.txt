- Semi-direct monocular VO
- Real time on a CPU
- Idea is to continuously estimate a semi-dense inverse depth map for the
  current frame, which in turn is used to track motion of the camera using
  dense image alignment
- Operates directly on pixel intensities rather than features
- Probabilistic mapping method
- 55 fps on Odroid U2 (ARM 1.6Ghz), 300 fps on laptop (Intel i7, 8 cores,
  2.8Ghz)
- Did not have metric comparing the PTAM vs SVO to ground truth, alot of the
  focus was just saying we are faster than PTAM and had plots to show it
  tracked the ground truth better.

Key Concepts are:

- A probabilistic depth map representation
- Tracking based on whole-image alignment
- reduction on image regions which carry information
- Full incorporation of stereo measurement uncertainty

Approach is partially motivated by the basic principle that for most real
time applications, video information is abundant and cheap to come by.
Therefore the computational budget should be spent such that the expected
information gain is maximized. Instead of reducing the images to a sparse
set of feature observations however, the proposed method continuously
estimates a "semi-dense inverse depth map" for the current frame. i.e.
A dense depth map covering all image regions with non-negligible gradient.
Is is comprised of one inverse depth hypothesis per pixel modeled by
a Gaussian probability distribution. This representation still allows for
the use of whole image alignment to track new frames, while at the same
time greatly reducing computational complexity compared to volumetric
methods.

![SVO Architecture](imgs/svo_architecture.png)

Methods that simultaneously recover camera pose and scene structure from
video can be divided into two classes:

- **Feature based**: extract sparse salient image features in each image, match
  them in successive frames using invariant feature descriptors, robustly
  recover both camera motion and structure using epipolar geometry,
  finally refine the pose and structure through reprojection error
  minimization. Disadvantage includes reliance on detection and matching
  thresholds necessary for robust estimation.
- **Direct methods**: estimate structure and motion directly from intensity
  values in the image. The local intensity gradient magnitude and direction is
  used in the optimization compared to feature based methods that consider
  only distance to some feature location. Direct methods that exploit all
  the information in the image, even from areas where gradients are small
  have been shown to outperform feature-based methods in terms of
  robustness in scenes with little texture or in the case of camera
  defocus and motion blur. The computation of the photometric error is
  more intensive than the reprojection error, as it involves warping and
  integrating large image regions. However, since direct methods operate
  directly on the intensity values of the image, the time for feature
  detection and invariant descriptor computation can be saved.

The motion estimation thread implements semi-direct approach to relative
pose estimation. The first step is pose initialization through **sparse
model based image alignment**: the camera pose relative to the previous
frame is found through minimizing the photometric error between pixels
corresponding to the projected location of the same 3D points. The 3D
coordinates corresponding to the reprojected points are refined in the
next step through alignment of the corresponding feature patches. Motion
estimation concludes by refining the pose and the structure through
minimizing the reprojection error introduced in the previous feature
alignment step.

In the mapping thread, a probabilistic depth filter is initialized for
each 2D feature for which the corresponding 3D point is to be estimated.
New depth filters are initialized whenever a new keyframe is selected in
regions of the image where few 3D to 2D correspondences are found. The
filters are initialized with a large uncertainty in depth. At every
subsequent frame the depth estimate is updated in a Bayesian fashion. When
a depth filter's uncertainty becomes small enough, a new 3D point is
inserted in the map and is immediately used for motion estimation.


## Motion Estimation

SVO computes an initial guess of the relative camera motion and the
feature correspondences using direct methods and concludes with a feature
based nonlinear reprojection error refinement.

### Spare Model based Image Alignment

The maximum likelihood estimate of the rigid body transformation between
two consecutive camera poses minimizes the negative log-likelihood of the
intensity residuals.

At this step SVO **only seeks to find the camera pose that minimizes the
photometric error of all patches**, it does not optimize for 3D landmark
positions nor does it optimize for 2D pixel locations.

In contrast to previous works, where the depth is known for large regions
in the image, SVO only knows the depth at spare feature  locations. SVO
denotes small patches of 4x4 pixels around the feature point with
a vector.

### Relaxation Through Feature Alignment

The previous step aligned the camera with respect to the previous frame.
Through back projection, the found relative pose implicitly defines an
initial guess for the future positions of all visible 3D points in the new
image. Due to inaccuracies in the 3D points' positions and thus, the
camera pose, this initial guess can be improved. To reduce teh drift, the
camera pose should be aligned with respect to the map, rather than to the
previous frame.

So at this step we are optimizing / aligning the current camera frame 2D
pixel locations based on previous key frames (not camera frames). But by
doing so an affine warping is applied to the reference batch violating the
epipolar constraints to achieve a higher correlation between the feature
patches.

This alignment is solved using the inverse compositional Lucas Kande
algorithm.


### Pose and Structure Refinement

In the previous step, we have established feature correspondence with
sub pixel accuracy at the cost of violating the epipolar constraints. In
particular SVO has generated a reprojection residual that is not equal to
0, which on average is around 0.3 experimentally. In this final step, the
current camera pose are optimized to reduce the reprojection residuals
using the motion-only Bundle Adjustment. This can efficiently be solved
using an iterative nonlinear least squares minimization algorithm such as
Gauss Newton.

Subsequently the position of the observed 3D points are also optimized
through the reprojection error minimization.

Finally it is possible to apply local BA in which both pose of all close
keyframes as well as the observed 3D points are jointly optimized.


## Discussion

- The Sparse Model based Image alignment and pose and structure refinement
seems redundant as both optimize the 6 dof pose of the camera. Indeed, one
could directly start with the second step and establish feature
correspondence through Lucas-Kanade tracking of all feature patches
followed by  nonlinear pose refinement, but the processing time would be
higher. Further some features could be tracked inaccurately, the sparse
image alignment step satisfies implicitly the epipolar constraint and
ensures that there are no outliers.

- One may also argue that the sparse image alignment would be sufficient
to estimate the camera motion, however the authors of SVO found
empirically that using the first step only results in a significantly more
drift compared to using all three steps together. The improved accuracy is
due to alignment of he new image with respect to the keyframes and the
map, whereas sparse image alignment aligns the new frame only with respect
to the previous frame.


## Semi-Dense Depth Map Estimation

Estimate a semi-dense inverse depth map for current camera image, which in turn
can be used for estimating the camera pose of the next frame. This depth map is
continuously propagated from frame to frame, and refined with new stereo depth
measurements.

- adaptive baseline stereo comparisons, allows us to accurately estimate the
  depth both of close-by and far away regions.
- keep exactly one inverse depth hypothesis per pixel represented as Gaussian
  probability distribution

### Stereo Based Depth Map Update

First a subset of pixels are selected for which the accuracy of a disparity
search is sufficiently large. Three intuitive and efficient computable
criteria are used:

For each selected pixel we then individually select a suitable reference frame
and perform a one dimensional disparity search. Propagated prior knowledge is
used to reduce the disparity search range when possible, decreasing the
computational cost and eliminating false minima. The obtained inverse depth
estimate is then fused into the depth map

#### Reference Frame Selection

Reference frame is chosen such that it maximizes the stereo accuracy, while
keeping the disparity search range as well as the observation angle
sufficiently small.

- use the oldest frame the pixel was observed in
- where the disparity search range and observation angle do no exceed a certain
  threshold
- If disparity search is unsuccessful, the pixel's "age" is increased, such
  that subsequent disparity searches use newer frames where the pixel is likely
  to be still visible

#### Stereo Matching Method

Perform an exhaustive search for the pixel's intensity along the epipolar line
in the selected reference frame, and then perform a sub-pixel accurate
localization of the matching disparity. If a prior inverse depth hypothesis is
available, the search interval is limited by $d \pm 2 \sigma_{d}$, where $d$
and $\sigma_{d}$ denote the mean and standard deviation of the prior
hypothesis. Otherwise the full disparity range is searched.

#### Uncertainty Estimation

Uncertainty Propagation is used to derive an expression for the error variance
$\sigma^{2}_{d}$ on the inverse depth $d$. In general this can be done by
expression the optimal inverse depth $d*$ as a function of the noisy inputs.
Here we consider the images $I_{0}$ and $I_{1}$ themselves, their relative
orientation $\epsilon$ and the camera calibration in terms of a projection
function $\pi$

\begin{equation}
   d* = d(I_{0}, I_{1}, \epsilon, \pi)
\end{equation}

The error-variance of $d*$ is then given by

\begin{equation}
  \sigma^{2}_{d} = J_{d} \sum J^{T}_{d}
\end{equation}

Where $J_{d}$ is the Jacobian of $d$ and $\sum$ the covariance of the input
error.


Three major factors which determine the accuracy of a stereo observation
- **Photometric disparity error**: $\sigma^{2}_{\lambda(\epsilon, \pi)}$,
  depends on the magnitude of the image gradient along the epipolar line.

- **Geometric disparity error**: $\sigma^{2}_{\lambda(I)}$, depends on the
  angle between the image gradient and the epipolar line (independent of
  gradient magnitude)

- **Pixel to inverse depth ratio**: $\alpha$ depends on the camera
  translation, the focal length and the pixel's position.

These three simple to compute and purely local criteria are used to determine
for which pixel a stereo update is worth the computational cost. Further, the
computed observation variance is then used to integrate the new measurements
into the existing depth map.

