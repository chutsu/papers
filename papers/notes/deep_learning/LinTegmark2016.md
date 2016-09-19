# Why does deep and cheap learning work so well?

Show how the success of deep learning depends not only on mathematics but also
on physics: although well known mathematical theorems guarantee that neural
networks can approximate arbitrary functions well, the class of functions of
practical interest can be approximated through "cheap learning" with
exponentially fewer parameters than generic ones, because they have simplifying
properties tracing back to the laws of physics. The exceptional simplicity of
physics based functions hinges on properties such as symmetry, locality,
compositionality, and polynomial log-probability, and we explore how these
properties translate into exceptionally simple neural networks approximating
both natural phenomena such as images and abstract representations thereof such
as drawings.


Function approximation covers most core sub-fields of machine learning,
including unsupervised learning, classification and prediction. When
investigating the quality of a neural network, there are several important
factors to consider:

- **Expressibility**: What class of functions can the neural networks express?
- **Efficiency**: How many resources (enurons, parameters, etc. Does the NN
  require?
- **Learnability**: How rapidly can neural network learn good parameters for
  function approximation?

The paper focuses on the first two as well as the following paradox: "How can
NN approximate functions well in practice, when the set of possible functions is
exponentially larger than the set of practically possible networks?"

NN performs a combinatorial swindle, replacing expoentiation by multiplication:
if there are say $n = 10^6$ inputs taking $v = 256$ values each, this swindle
cuts the number of parameters from $v^n$ to $v \times n$ times some constant
factor. We will show that this success of this swindle depends fundamentally on
physics: although NN only work well for an expoentially tiny fraction of all
possible inputs, the laws of physics are such that the data sets we care about
for ML are also drawn from an expoentially tiny fraction of all imaginable
datasets. Moreover, these two tiny subsets are remarkably similar, enabling
deep learning to work well in practice.
