# Unified Inverse Depth Parameterization for Monocular SLAM

An issue with monocular slam has been the intialization, since information
from multiple images acquired during motion must be combined to achieve
accurate depth estimates. This has led algorithms to deviate from
desirable Gaussian uncertainty representation of the probabilistic
filters. In this paper, a inverse parameterization is introduced which
permits efficient and accurate representation of uncertainty during
undelayed initalization and beyond all within the standard EKF. The key
concept is direct parameterization of inverse depth, where there is a high
degree of linearity. Importantly, the parameterization can cope with
features which are far from the camera (little parallax during motion).

A monocular cmaerea is a projective sensor which measures the bearing of
image features. To infer the depth of a feature the camera must observe it
repeatedly as it translates through the scene, each time capturing a ray
of light from the feature to its optic center. The angle between the
captured rays is the feature's parallax, this is what allows its depth to
be estimated.

A particle approach was used to represent a feature's depth coordinate
until conversion to Gaussian representation when the distribution had
collasped sufficiently . This "delayed" style of initialisation meant that
observations of features were not used to update the camera pose estimate
until their conversion into fully initialized features.
