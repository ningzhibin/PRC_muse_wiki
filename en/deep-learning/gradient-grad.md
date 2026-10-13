# What Is a Gradient: Derivatives, Partial Derivatives, and `grad` in Code

Simply put, a gradient is a derivative. To be more precise, it is a **"partial derivative."**

There is a subtle distinction between the strict mathematical concept and the naming conventions in deep-learning code; it helps to map them onto each other.

## The strict mathematical definitions

- **Derivative**: usually refers to differentiating a function of a single variable.
- **Partial derivative**: when a function has multiple variables, differentiating with respect to just one of them (treating the others as constants).
- **Gradient**: a vector (array) that bundles the partial derivatives of all variables together, pointing in the direction of the function's steepest increase.

## In code (Micrograd, PyTorch, etc.)

- A neural network's loss function is a multivariate function of thousands or hundreds of millions of variables (the weights).
- At the macro level, we are computing the gradient of the whole network.
- But down at each individual `Value` node, its internal `grad` attribute stores only one component of that global gradient vector — the partial derivative of the final result with respect to itself.

So for brevity and uniformity, deep-learning frameworks simply call this per-variable "partial derivative" `grad` ("gradient"). Mentally translating it as "the derivative of the final result with respect to this variable" is completely accurate.
