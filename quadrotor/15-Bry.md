While very detailed models are available for the quadrotor and fixed-wing
vehicles, capturing complex aerodynamic effects and detailed input respose
characteristics, we instead focus on planning with particular simplified
representations. Specifically, we focus on representations for these
vehicles that are differentially flat. For our purposes, the result of
differentially flatness is an algebraic mapping from a set of flat output
variables and their derivatives to the vehicle staes and inputs.

The mapping between vehicle states and inputs and the  at output space is
what makes differential flatness useful for trajectory planning. The
challenge in trajectory optimization and motion planning for dynamical
systems is to find a (minimum cost) sequence of control inputs that will
carry the system between specified initial and terminal states subject to
the differential equations of the system. This problem can be framed as
a numerical optimization or graph search, but in either case, it involves
an expensive search over the set of possible input sequences. The
advantage of differential flatness in the context of trajectory planning
is that a trajectory can be specified directly in the flat output space,
and the control inputs needed to execute that trajectory can be obtained
as an algebraic function of the trajectory and its derivatives. For many
systems, it may be much easier to specify and optimize a trajectory in the
flat output space than it is to perform a search over the input space.
