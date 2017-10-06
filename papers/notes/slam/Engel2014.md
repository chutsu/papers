# LSD-SLAM: Large-Scale Direct Monocular SLAM

- Direct (feature-less) monocular SLAM algorithm
- Allows for large-scale consistent maps
- Obtained by filtering over a large number of pixelwise small-base line stereo
  comparisons
- Direct tracking method explicitly detects scale drifts
- Probabilistic solution to include effect of noisy depth values into tracking


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
