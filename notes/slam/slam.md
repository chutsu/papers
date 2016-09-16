# SLAM

## Types

- **Feature-Based Methods**: Fundamental idea behind feature-based approaches
  (both filtering-based and keyframe-based) is to split the overall problem of
  estimating geometric information from images into two steps. First, a set of
  feature observations is extracted from the image. Second, the camera position
  and scene geometry is computed as a function of these feature observations
  only.

  While this decouping simplifies the overall problem, it comes with an
  important limitation: Only information that conforms to the feature type can
  be used. In particular, when using keypoints, information contained in
  straight or curved edges is discarded. Several approaches have been made in
  the past to remedy this by including edge-based or even region based
  features. Yet since the estimation of the high-dimensional feature space is
  tedious, they are rarely used in practice. To obtain dense reconstructions,
  the estimated camera poses can be used to subsequently reconstruct dense
  maps, using multiview stereo.

- **Direct Methods**: Direct visual odometry (VO) methods circumvent this
  limitation by optimizing the geometry directly on the image intensities,
  which enables using all information in the image. In addition to higher
  accuracy and robustness in particular in environments with little keypoints,
  this provides substantially more information about the geometry of the
  environment, which can be very valuable for robotics or augmented reality
  applications.

  While direct image alignment is well-established for RGB-D or stereo sensors,
  only recently monocular direct VO algorithms have been proposed. In
  semi-dense depth filtuering formulation was proposed which significantly
  reduces computational complexity, allowing real-time operation on a CPU and
  even on a modern smartphone. By combining direct tractking with keypoints,
  high frame rates can be achieved on embedded systems. All these approaches
  hoever are pure visual odometries, they only locally track the motion of the
  camera and do not build a consistent, global map of the environment including
  loop closures.

- **Pose Graph Optimization**: Well known SLAM technique to build a consistent,
  global map: The world is represented as a number o keyframes connected by
  pose-pose constraints, which can be optimized using a generic graph
  optimization framework like g2o.
