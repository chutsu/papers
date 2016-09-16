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
