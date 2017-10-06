# IMU Preintegration on Manifold

- Addresses optimization-based VIO computational complexity by
preintegrating inertial measurements between selected keyframes.
- Allows accurate summarization of hundreds of inertial measurements into
a single relative motion constraint

## Contributions

1. Preintegration of IMU measurements consists of combining many inertial
   measurements between two keyframes into a single relative motion
   constraint. This paper presents a preintegration theory that properly
   addresses the manifold structure of the rotation group and allows for
   analytic derivation of all Jacobians.

2. The paper frames the preintegrated IMU model into a factor graphy
   perspective, which enables the design of a constant time VIO pipeline.

3. Implementation resulting with an update rate of 10ms
