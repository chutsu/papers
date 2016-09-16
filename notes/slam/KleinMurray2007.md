# Parallel Tracking and Mapping for Small AR Workspaces

Estimating camera pose in an unknown scene has been attempted by adpating SLAM
algorithms, the authors attempt to adapt SLAM algorithms for AR applications.
Most AR approaches assume a comprehensive map is available but often this is
not available, this lead to the development of a class of techniques known in
the AR context as extensible tracking in which the system attemps to add
previosuly unknown scene elements to its intial map, and these then provide
registration even when the original map is out of sensing range.

The logical extension of extensible tracking is to track in scene without any
prior map, and this is the focus of the paper.


Method overview:

- Tracking and Mapping are separated and executed on parallel threads
- Mapping is based on keyframes, which are processed using batch techniques
  (Bundle Adjustment)
- Map is densely initialized from a stereo pair (5 point algorithm)
- New points are initialized with an epipolar search
- Lear numbers (thousands) of points are mapped


Author argues that their problem is more difficult than EKF-SLAM and FastSLAM,
in particular both are for robots that can provide odometry information and
move at arbitrarily slow speeds, finally in both of these methods the mapping
and tracking are intimately linked, so current camera pose and position of
every landmark are updated together at every single video frame. These
Mono-SLAM methods go to great lengths to remove data association errors,
starting with covariance-driven gating ("active search"), they must further
perform binary inlier/outlier rejection with Joint Compatibility Branch and
Bound (JCBB) or Random Sample Consensus (RANSAC), both of which are not robust
enough for the authors.

The rationale for having separate threads for Tracking and Mapping is to avoid
processing every single video frame, incremental tracking wastes processing time
on redundant information, the authors approach concentrates on smaller number
more useful keyframes. Incremental mapping is replaced with a computationaly
expensive but highly accurate batch method called Bundle Adjustment.


Tracking Procedure:

1. A new frame is acquired from the camera, and a prior pose estimate is
generate from a motion model
2. Map points are projected into the image according to the frames prior pose
estimate.
3. A small number (50) of the coarsest-scale features are searched for in the
image.
4. The camera pose is updated from these coarse matches.
5. A larger number (1000) of points is re-projceted and searched for in the
image.
6. A final pose estimate for the frame is computed from all matches found.




Limitations:
- rapid camera motions produce large levels of motion blur which can decimate
  corner features
- has trouble initializing stereo algorithm, no explaination was given why, the
  intialization step is simply repeated  until the algorithm is initialized.
- insertion of incorrect information occurs when tracking has failed but the
  tracking quality heuristics have not detected this failure.
- not designed to close large loops (loop closure)

