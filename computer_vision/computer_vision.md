# Descriptors

In general, the problem we are focusing on is that of comparing two image
patches and measuring their similarity. Given two such patches, how can we
determine their similarity? We can measure the pixel to pixel similarity by
measuring their Euclidean distance, but that measure is very sensitive to
noise, rotation, translation and illumination changes. In most applications we
would like to be robost to such change. For example, in the image alignment
application, we would like to be robust to small view-point changes that means
robustness to rotation and to translation.

This is where patch descriptors come in handy. A descriptor is some function
that is applied on the patch to describe it in a way that is invariant to all
the image changes that are suitable for our application (e.g. rotation,
illumination, noise, etc). A descriptor has a "built-in" distance metric which
can be used to determine the similarity, or distance between two computed
descriptors. So to compare two image patches, we compute their descriptors and
measure their similarity by measuring their descriptor similarity.

Common pipeline for patch descriptors is:

1. Detect keypoints in image
2. Describe each region around a keypoint as a feature vector, using a
descriptor
3. Use the descriptors in the applciation, or compare descriptors by using the
descriptors distance function.


## Types of Descriptors

- Patch Descriptors
- Binary Descriptors


### Binary Descriptors
Binary descriptors has its advantage rooted at its performance compared to
patch descriptors, this is achieved by only performing intensity comparisons.
The hamming distance as a distance measure between two binary descriptor is
performed using a single computational instruction, as the hamming distance
equals the sum of the XOR operation between the two binary descriptors, this is
the common line and rational behind binary descriptors.

In general binary descriptors are composed of three parts:

1. Sampling pattern
2. Orientation compensation
3. Sampling pairs

Consider a small patch centered around a keypoint, we would like to describe it
as a binary string. First we take a sampling pattern around the keypoint, for
example - points spread on a set of concetric circles. Next, choose, say 512
pairs of points on this sampling pattern. Now go over all the pairs and compare
the intensity value of the first point in the pair with the intensity value of
the second point in the pair - if the first value is larger then the second,
write 1 in the string, otherwise write 0. That is it! After going through all
512 pairs, we have a 512 character string, composed of 1s and 0s that encode
the local information around the keypoint.
