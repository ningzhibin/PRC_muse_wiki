# Gradient 到底是什么：导数、偏导数与代码里的 grad

简单来说，gradient 就是导数。如果说得更严谨一点，它是**"偏导数"（Partial Derivative）**。

在严格的数学概念和深度学习的代码命名习惯上，有一点微妙的区别，可以这样对应着理解。

## 数学里的严格定义

- **导数（Derivative）**：通常指只有一个变量的函数（一元函数）的求导结果。
- **偏导数（Partial Derivative）**：当函数有多个变量时，只针对其中某一个变量求导（把其他变量当成常数）。
- **梯度（Gradient）**：是一个向量（数组），把所有变量的偏导数打包在一起，指明了整个函数值变化最快的方向。

## 在 Micrograd（或 PyTorch）等代码中

- 神经网络的损失函数（Loss）是一个包含成千上万甚至几亿个变量（权重）的多元函数。
- 宏观上看，我们在求整个网络的梯度（Gradient）。
- 但落到每一个具体的 Value 节点上，它内部的 `grad` 属性，存储的其实只是全局梯度向量中的一个分量，也就是最终结果对它自己的那个偏导数。

所以，为了代码简洁和统一，深度学习框架通常把每个变量身上的这个"偏导数"直接简称为 `grad`（梯度）。在心里把它翻译成"最终结果对当前这个变量的导数"是完全准确的。

---

# English

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
