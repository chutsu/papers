- Nonlinear adaptive controller
- Simulation only
- Backstepping technique mixed with Multi-Layer Preceptron Neural Network
- MLP is trained online (is this not dangerous?)
- Author claims dynamic model and all parameters not required for controller
- Simulation shows good results
- Suspiciously good
- Hard to digest the Lyapunov stability proof

Nonlinear adaptive controller for quadrotor by using the backstepping technique
mixed with neural networks, this controller has only been tested in simulation.
Prior knowledge of all physical parameters and exact models were not required,
the authors claim online adapation of neural networks and parameters are used
to compensate unmodeled quadrotor dyanmics, and under certain releaxed
aussumptions the control scheme can gauarantee all singals in the closed loop
system are Uniformly Ultimately Bounded (UUB).

Various advanced control techniques have been mentioned, namely exact
linearization, sliding mode control and backstepping technique, but the author
did not give a convincing answer why backstepping is better, only that it
"seems" to be more applicable for underactuated systems.
