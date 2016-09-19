- Minimum snap trajectory generation
- Many previous quadrotor controllers are linear and based on hover conditions
- Exploring full state space using reachability algorithms, incremental search
  or LQR-tree based search is impractical for quadrotors (search space is too
  big).
- Machine learning techniques do not really consider trajectory planning with
  obstacles
- Author develops flight trajectories that leverage dynamics of the system
  instead of viewing it as a constraint


Many quadrotor controllers are derived from linearization of the model around
hover conditions and are stable only under reasonably small roll pitch angles.
Exploring full state space using reachability algorithms, incremental search
techqniues or LQR-tree based seawrch is impractical for a dynamic system with 6
degrees of freedom, the search space is too large.

While machine learning techniques have been successful in learning models using
data from human pilots and in improving performance using reinforcement
learning, these approaches do no appear to lend themselves to motion planning
or trajectory generation in environments with obstacles.

With Model Predicitve Control, guarantees of convergence are only available
when linearized model is fully controllable or if Lyapunov function can be
synthesized, as such it appears to be difficult to directly apply MPC to
trajectory generation of a quadrotor.

The author believes that it is necessary to develop flight plans that leverage
dynamics of the system instead of simply viewing the dynamics as a constraint
on the system.

They show a quadrotor with 4 inputs is differentially flat. Flatness in systems
theory is a system property that extends the notion of controllability from
linear systems to nonlinear dynamical systems. A system that has flatness
property is called a flat system. Flat systems also have a (fictitous) flat
output, which can be used to explicitly express all states and inputs in terms
of the flat output and a finite number of its derivatives.

Defined keyframe as a position in space along the trajectory with a yaw angle.
A trivial trajectory that satisfy time and space constraint in a generated
trajectory is inefficient because it has infinite curvatutre at the keyframes
which requries the quadrotor to stop. The authors method generates an optimal
trajectory that smoothly transitions through the keyframes at the given times
while staying within safe corridors.
