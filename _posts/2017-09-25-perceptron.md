---
layout: post
title: Perceptron Learning Algorithm
description: How to implement a simple Perceptron in Python to learn logical gates.
excerpt: Understanding and implementing the Perceptron learning algorithm.
permalink: /blog/perceptron-learning-algorithm/
---
I recently learned the Perceptron learning algorithm. Machine learning as it is today is relatively new but the Perceptron, one of the simplest learning algorithms was developed in the late 50s! Neural networks that drive cars, describe images or generate music are just collections of more sophisticated Perceptrons. Pretty cool. I implemented a basic perceptron from scratch that "learns" logic gates like AND, OR, NOT etc.

This algorithm is used for **supervised learning of a binary classifier**. For example, given the grades of a student in several courses (the features) predict whether a student will get an Admit or Reject (class labels) from a university. The perceptron takes as input these 'features' that describe the data and produces a classifier that can predict a positive or negative class label (Yes/No or Admit/Reject or 1/0) given some new data.

### What is a Perceptron made of?

<p align="center">
<img src="/images/Perceptron.png" width="460"><br>
<sup><a href="https://towardsdatascience.com/what-the-hell-is-perceptron-626217814f53" target="_blank">Source: TowardsDataScience</a></sup>
</p>

**Inputs [x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>N</sub>].** These are features of the data. These could be a set of [GRE-Grade, TOEFL-Grade, GPA] for an admission decision classifier or [0, 1, 1, 0] for a logic gate classifier, for example.

**Weights [w<sub>1</sub>, w<sub>2</sub>, ..., w<sub>N</sub>].** Weights are assigned to each feature which the Perceptron learning algorithm modifies in order to give more or less importance to certain features.

**Bias.** This is an additional attribute used along with the set of features to give our Perceptron some freedom to shift the decision boundary to improve the quality of a classifier. Bias is represented as an additional feature with value 1 and weight w<sub>0</sub>.

**Activation function.** This is the function that produces a classification (Admit/Reject or 1/0 etc). One common activation function is the Unit Step Activation function that produces 1 if the input is positive and produces 0 if it is negative.

<p align="center">
<img src="/images/PerceptronStepFunction.png" width="300"><br>
<sup><a href="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ac/HardLimitFunction.png/800px-HardLimitFunction.png" target="_blank">Step Activation Function (Wikibooks)</a></sup>
</p>

### What does a Perceptron do?

Essentially, a Perceptron takes the inputs, multiplies them by their weights, adds the bias, passes this value through the Activation Function and produces a classification.

<p align="center">
<b>Activation(w &middot; x) = y</b>
</p>

Where **w** is the weight vector, **x** is the feature vector, **w &middot; x** is the dot product **∑w<sub>i</sub>x<sub>i</sub>** of the features and weights and **y** is the predicted class.

If the Perceptron could learn values of the weights, then given a new data point we could just plug in its feature vector into this equation to produce a classification.

### How does it "learn"?

The Perceptron algorithm starts out with a random set of weights. These weights are updated at each pass through the training data using this equation:

<p align="center">
<b>w' = w + &eta;(y<sub>expected</sub> - y<sub>predicted</sub>)x</b>
</p>

Where **w** is the current weight vector, **&eta;** is the learning rate (a number between 0.0 and 1.0 which we can tune), **y<sub>expected</sub>** is the "true" or actual class, **y<sub>predicted</sub>** is the class our algorithm predicted and **x** is the feature vector.

**Why does this work?** Here's a simple intuition.

The weights are left untouched when we make a correct prediction (y<sub>expected</sub> = y<sub>predicted</sub>). That's great. But let's say we predict 0 for a sample but the expected class was 1. According to this equation, something is being added to the weight vector and in the same direction as the feature vector **x**. This shifts the weight vector towards the feature vector (moving towards the positive sample). If we predict 1 and 0 was expected, something is being subtracted from the weight vector in the opposite direction of the feature vector which pushes **w** away from the direction of **x** (away from the negative sample).

If we repeat this weight update process enough number of times, we would have hopefully shifted the weight vector towards positive samples, reducing the angle between them and **w** to less than 90&deg;. This would make the dot product **w &middot; x** positive (cos(&theta;) is positive for angles less than 90&deg;). Similary we hope the angle between **w** and negative samples would have increased to greater than 90&deg; so that the dot product is negative.

This means that the activation function would produce 1 for the positive samples and 0 for negative samples. This is exactly what we want.

### Show me the code

Here's the code for Perceptron learning a logic gate. The complete implementation is [here](https://gist.github.com/amanps/ceb9c79615aaa97f069ee481cbdd1d12){:target="_blank"}.
{% highlight python %}
learning_rate = 0.2
epochs = 500

def learn(training_data, weights, epochs):
    for i in range(epochs):
        x, label = random.choice(training_data)
        dot_product = np.dot(weights, x)
        error = label - step_activation(dot_product)
        weights += learning_rate * error * x
{% endhighlight %}

The variable `epochs` describes the number of data samples we pick to update weights. Typically, datasets are bigger than the logic gates dataset used in my implementation, in which case we could just iterate through each sample of the training data to get the `x, label` pair and make the subsequent weight update.

Dataset for the OR logic gate. Each element is of type `([x1, x2, bias], label)`.
{% highlight python %}
training_data_OR = [(np.array([0,0,1]), 0),
                    (np.array([0,1,1]), 1),
                    (np.array([1,0,1]), 1),
                    (np.array([1,1,1]), 1)]
{% endhighlight %}

And the simple step activation function:
{% highlight python %}
def step_activation(x):
    return 0 if x < 0 else 1
{% endhighlight %}

The decision boundary learned by the OR logic gate. This is a plot between x<sub>1</sub> and x<sub>2</sub> and the decision boundary is found by setting y = 0 in the equation **w &middot; x = y** that is,

<p align="center">
<b>w<sub>1</sub>x<sub>1</sub> + w<sub>2</sub>x<sub>2</sub> + w<sub>0</sub> = 0</b>
</p>

<p align="center">
<img src="/images/PerceptronOR.png" width="340"><br>
</p>

As we can observe, the decision boundary perfectly separates the negative samples `(0, 0)` from the positive samples `[(0, 1), (1, 0), (1, 1)]`.

The Perceptron learning algorithm **works for datasets that are linearly separable**. For example the XOR dataset is not linearly separable and hence no simple perceptron can produce a decision boundary that slices the dataset.

<p align="center">
<img src="/images/PerceptronXOR.png" width="340"><br>
<sup><a href="https://matlabgeeks.com/tips-tutorials/neural-networks-a-multilayer-perceptron-in-matlab/" target="_blank">Source: Matlab Geeks</a></sup>
</p>

Try to draw a line that classifies these 4 points. I'll wait.

[Check out my implementation](https://gist.github.com/amanps/ceb9c79615aaa97f069ee481cbdd1d12){:target="_blank"} of this algorithm from scratch to see the decision boundary of XNOR tripping in all sorts of directions, unable to come up with a linear separation.
