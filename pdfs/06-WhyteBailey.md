# SLAM: Part I The Essential Algorithms

This paper describes the probabilistic form of the SLAM problem, essential
solution methods and significant implementations.

The landmark paper by Smith, Self and Cheeseman [SmithEtAl1990]
bootstraped SLAM research, this paper showed that as a mobile robot moves
through an unknown environment taking relative observations of landmarks,
the estimates of these landmarks are all necessarily correlated with each
other because of the common error in estimated vehicle location
[LeonardDurrant1991]. The implication of this was profound: A consistent full
solution to the combined localisation and mapping problem would require a joint
state composed of the vehicle pose and every landmark position, to be updated
following each landmark observation. In turn, this would require the estimator
to employ a huge state vector (of order the number of landmarks maintained in
the map) with computation scaling as the square of the number of landmarks.

Crucially, this work did not look at the convergence properties of the map
or its steady-state behavior. Indeed, it was widely assumed at the time
that the estimated map errors would not converge and would instead exhibit
a random walk behavior with unbounded error growth. Thus, given the
computational complexity of the mapping problem and without knowledge of
the convergence behavior of the map, researchers instead focused on
a series of approximations to the consistent mapping problem solution
which assumed or even forced the correlations between landmarks to be
minimized or eliminated so reducing the full filter to a series of
decoupled landmark to vehicle filters. Also for these reasons, theoretical work
on the combined localisation and mapping problem came to a temporary halt, with
work often focused on either mapping or localisation as separate problems.

The conceptual break-through came with the realisation that the combined
mapping and localisation problem, once formulated as a single estimation
problem, was actually convergent. Most importantly, it was recognised that
the correlations between landmarks, that most researchers had tried to
minimize, were actually the critical part of the problem and that, on the
contrary, the more these correlations grew, the better the solution. The
structure of the SLAM problem, the convergence result and the coining of
the acronym 'SLAM' was first presented in a mobile robotics survey paper
presented at the 1995 International Symposium on Robotics Research.


## SLAM Problem Formulation
In robotic mapping, simultaneous localization and mapping (SLAM) is the
computational problem of constructing or updating a map of an unknown
environment while simultaneously keeping track of an agents location within
it. While this initially appears to be a chicken-and-egg problem there are
several algorithms known for solving it, at least approximately, in tractable
time for certain environments.

![SLAM](imgs/slam.png)

Consider a mobile robot moving through an environment taking relative
observations of a number of unknown landmarks using a sensor located on the
robot. At a time instant $k$, the following quantities are defined:

- $x_{k}$: State vector describing the pose and more of the robot
- $u_{k}$: Control vector applied at time $k - 1$ to drive the robot to a state
  $x_k$ at time $k$.
- $m_{i}$: A vector describing the location of the $i^{\text{th}}$ landmark
  whose true location is assumed time invariant.
- $z_{ik}$: An observation taken from the vehicle of the location of the
  $i^{\text{th}}$ landmark at time $k$. When there are multiple landmark
  observations at any one time or when the specific landmark is not relevant to
  discussion, the observation will be written simply as $z_{k}$.

In addition the following sets are also defined:

- $X_{0:k} = {x_{0}, x_{1}, ..., x_{k}} = {X_{0:k - 1}, x_{k}}$: History of
  robot locations
- $U_{0:k} = {u_{0}, u_{1}, ..., u_{k}} = {U_{0:k - 1}, u_{k}}$: History of
  control inputs
- $m = {m_{1}, m_{2}, ..., m_{n}}$: Set of all landmarks
- $Z_{0:k} = {z_{0}, z_{1}, ..., z_{k}} = {Z_{0:k - 1}, z_{k}}$: Set of all
  landmark observations

The most important insight in SLAM was to realize that the correlations between
landmark estimates increase monotonically as more and more observations are
made. Practically, this means that knowledge of the relative location of
landmarks always improves and never diverges, regardless of robot motion. In
probabilistic terms, this means that the joint probability density on all
landmarks $P(m)$ becomes monotonically more peaked as more observations are
made.

This convergence occurs because the observations made by the robot can be
considered as "nearly independent" measurements of the relative location
between landmarks. Relative location of observed landmarks is independent of
the coordinate frame of the robot and successive observations from this fixed
location would yield further independent measurements of the relative
relationship between landmarks. The term "nearly independent" measurement is
appropriate because the observation errors will be correlated through
successive vehicle motions.




## Solutions to the SLAM Problem

By far the most common representation is in the form of a state-space model
with additive Gaussian noise, leading to the use of the extended Kalman filter
(EKF) to solve the SLAM problem. One important alternative representation is to
describe the vehicle motion model as a set of samples of a more general
non-Gaussian probability distribution. This leads to the use of the
Rao-Blackwellised particle filter, or FastSLAM algorithm to solve the SLAM
problem. While EKF-SLAM and FastSLAM are the two most important solution
methods, newer alternatives, which offer much potential, have been proposed
including the use of the information-state form.




### References

- [R. Smith, M. Self, and P. Cheeseman. Estimating uncertain spatial
  relationships in robotics. In I.J. Cox and G.T. Wilfon, editors,
  Autonomous Robot Vehicles, pages 167–193. SpringerVerlag,
  1990.][SmithEtAl1990]

- [J.J. Leonard and H.F. Durrant-Whyte. Simultaneous map building and
  localisation for an autonomous mobile robot. In Proc. IEEE Int. Workshop
  on Intelligent Robots and Systems (IROS), pages 1442–1447, Osaka, Japan,
  1991.][LeonardDurrant1991]

[SmithEtAl1990]: https://arxiv.org/pdf/1304.3111.pdf
[LeonardDurrant1991]: https://marinerobotics.mit.edu/sites/default/files/Leonard91iros.pdf
