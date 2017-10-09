# Visual SLAM: Why Filter?

There are generally two different approaches to real time Structure From Motion
(SFM) or SLAM problems:

- **Filtering Methods**: marginalize out past poses and summarise the
  information gained over time with a probability distribution.

- **Keyframe Methods**: retain the optimisation approach of global bundle
  adjustment, but computationally must select only a small number of past
  frames to process.

In this paper a rigorous analysis of the relative advantages of filtering and
sparse bundle adjustmentfor sequential visual SLAM. In a series of Monte Carlo
experiments the accuracy and cost of visual SLAM is investigated.

The accuracy is measured in terms of entropy reduction as well as Root Mean
Square Error (RMSE), and while efficiency of bundle adjustment versus filtering
using combined cost / accuracy measures is also investigated.


## Filtering vs Bundle Adjustment (BA)

The general problem of SLAM / SFM can be psoed in terms of inference on a
graph. In real-time SLAM, this graph network will continuously grow as new pose
and measurement variables are added at every time step, and new feature
variables will be added whenever new parts of a scene are explroed for the
first time.

Although various parametric and non-parametric inference techniques have been
applied to SFM and SLAM problems (e.g. particle filters), the most generally
sucessful methods in both filtering and optimisation have assumed GAussian
distributions for measurements and ultimately state-space estimation;
equivalently we could say that they are least squares methods which minimises
in the reprojection error. BA in SFM, or thje EKF and variants in SLAM all
manipulate the same types fo matrices representing Gaussian means and
covariances. The clear reason is the special status of the Gaussian as the
central distribution of probability theory which makes it the most efficient
way to represent uncertainty in a wide range of practical inference. We
therefore restrict our analysis to this domain.

In the filtering approach, features which may be measured again in the future
are retained. The result is a graph which stays relatively compact; it will not
grow arbitrarily with time, adn will not grow at all during repeated movement
in a restricted area, adding persistent feature variables only when new areas
are explored. The downside is that the graph quickly becomes fully
inter-connected, since every elimination of a past pose variable cuases fill-in
with new links between every pair of feature variables to which it was joined.

In the keyframe approach, solving the graph from scratch time after time as it
grows, but to sparsify it by removing all by a small subset of past poses. In
some applications it is sensible for the retained poses to be ina  sliding
window of the most recent camera positions, but more generally they are a set
of intelligently or heuristically chosen keyframes. The other poses, and all
the measurements connected to them, they do not contriubte to estimates.
Compared to filtering, this approach will produce a graph which has more
elements (since many past poses are retained), but importantly for inference
the lack of marginalisation means that it will remain sparsely inter-connected.
The result is that graph optimisation remains relatively efficient, even if the
number of features in teh graph and measured from the keyframes is very high.

The key question is whether it makes sense to summarise the information gained
from historic poses and measurements or to discard some of those measurements
in such a way that repeated optimisation from scratch becomes feasible, and
propagating a probability distribution through time is unnecessary.


The analysis was performed using covariance back-propagation starting from the
ground truth solution and assuming the best for filtering; that the accuracty
of BA and filtering is identical. The main result was: "Increasing the number
of observations N increases the accuracy , while increasing the number of
intermediate keyframes M only has a minor effect. Considering the cost of BA
(linear in N) to the cost of filering (cubic in N), it ceomes clear that BA is
more efficient, especially when high accuracy is requried".
