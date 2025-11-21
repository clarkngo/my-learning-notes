---
title: Neural Networks
layout: default
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

### **Neural Network Training Checklist**

1.  **Initialize:** Assign random weights (usually small numbers close to 0).
2.  **Forward Pass (Hidden):** Calculate inputs $\times$ weights + bias. Apply **ReLU** to introduce non-linearity.
3.  **Forward Pass (Output):** Calculate final layer values. Apply **Sigmoid** (for binary) or **Softmax** (for multi-class) to get probabilities.
4.  **Calculate Loss:** Compare the prediction to the actual target using the **Loss Function**.
5.  **Backpropagation:** Calculate the gradient (find out which weights contributed to the error).
6.  **Optimizer Step:** The **Adam** optimizer updates the weights based on the gradient.
7.  **Repeat:** Do this for every batch until the **Epoch** is finished.

---

### **Key Components & Definitions**

#### **Inputs**
* **Definition:** The raw data fed into the network (e.g., pixels of an image).
* **Role:** Affects the initial score, but these values **never change**.

#### **Weights**
* **Definition:** The "strength" of the connection between neurons.
* **Role:** These are the **only** things the network can actually "learn" or change.

#### **Activation Functions**
* **General Role:** Curves the linear slope (adds non-linearity) so the model can learn complex shapes.
* **ReLU (Rectified Linear Unit):**
    * *Logic:* "If negative, make it 0. If positive, keep it."
    * *Use:* The standard for **Hidden Layers**. Fast and efficient.
* **Sigmoid:**
    * *Logic:* "Squish the number between 0 and 1."
    * *Use:* The standard for **Output Layers** (binary classification) to represent probability (e.g., 0.8 = 80% chance).

#### **Loss Function**
* **Definition:** The "Scorecard."
* **Role:** Calculates a single number representing **how wrong** the model's prediction was compared to the truth. (e.g., CrossEntropy).

#### **Optimizer**
* **Definition:** The "Hiker" finding the bottom of the valley.
* **Role:** Updates the weights to minimize the Loss.
* **Adam:**
    * *Specifics:* A "smart" optimizer that uses **Momentum** (keeps moving in the right direction) and **Adaptive Learning Rates** (changes step size based on terrain). It is the current industry standard.

#### **Epoch**
* **Definition:** One full cycle through the **entire** dataset.

#### **Batch Size**
* **Definition:** The number of samples processed before updating the weights once.
* **Role:** Small batches = noisy but fast updates. Large batches = stable but memory-heavy updates.

#### **Overfitting**
* **Definition:** The model memorizes the "noise" or specific details of the training data.
* **Symptom:** High accuracy on training data, low accuracy on new/test data. (The student memorized the answer key but fails the exam).

#### **Underfitting**
* **Definition:** The model is too simple to capture the pattern.
* **Symptom:** Poor accuracy on both training and test data. (The student didn't study enough or the subject is too hard).

## Number of Neurons for Layers
Deciding the number of neurons for a `Dense` layer in Keras is a mix of strict rules (for inputs/outputs) and experimentation (for hidden layers).

There is no single formula that gives the perfect number, but there is a standard process to find it.

### 1\. The "Fixed" Layers (No Choice)

For your first and last layers, the number of neurons is determined strictly by your data.

  * **Input Layer:** Must match the number of input features (columns) in your data.
      * *Example:* If your input is a spreadsheet with 10 columns (price, age, size, etc.), your input shape effectively expects **10** neurons.
  * **Output Layer:** Determined by what you are predicting.
      * **Regression (predicting a number):** 1 neuron (e.g., predicting house price).
      * **Binary Classification (Yes/No):** 1 neuron (with `sigmoid` activation).
      * **Multi-class Classification:** N neurons, where N is the number of classes (e.g., 10 neurons for identifying digits 0-9, with `softmax` activation).

-----

### 2\. The Hidden Layers (The "Art")

For the layers in the middle, you have to choose. Here are the three most common strategies, ranked from simplest to most advanced.

#### Strategy A: The "Funnel" (Most Common)

Many successful architectures follow a funnel shape, where the layers get smaller as they get deeper. The logic is that the network condenses information into higher-level features.

  * **Rule of Thumb:** Start with a size between your input and output size, and reduce it by half for each subsequent layer.
  * **Common Values:** Use powers of 2 because they align well with GPU memory (32, 64, 128, 256, 512).

> **Example:**
> Input (100 features) $\to$ Dense(64) $\to$ Dense(32) $\to$ Output(1)

#### Strategy B: The "Stretch Pants" Approach

If you aren't sure, it is better to make the layer **too big** than too small. A network that is slightly too large can still learn, but a network that is too small will never capture the complexity of the data (underfitting).

1.  Make the layer large (e.g., 512 neurons).
2.  Add a `Dropout` layer immediately after it to prevent overfitting.

<!-- end list -->

```python
model.add(layers.Dense(512, activation='relu'))
model.add(layers.Dropout(0.5)) # Kills 50% of neurons randomly during training
```

#### Strategy C: The "Mean" Heuristic

A simple starting point for a single hidden layer is to find the average size.

**The Rule:**
Add the number of input neurons to the number of output neurons, then divide the total by 2.

**Example:** If you have **10** inputs and **2** outputs, you would do (10 + 2) / 2 = **6** hidden neurons.
-----

### 3\. The "Scientific" Way (Keras Tuner)

Instead of guessing, you can write code to let Keras find the optimal number for you. This is called Hyperparameter Tuning.

You can use **Keras Tuner** to test a range of values (e.g., "try every value between 32 and 512") and report back which one resulted in the highest accuracy.

```python
import keras_tuner as kt

def build_model(hp):
    model = keras.Sequential()
    # Tune the number of units in the first Dense layer
    # Choose an optimal value between 32-512
    hp_units = hp.Int('units', min_value=32, max_value=512, step=32)
    
    model.add(layers.Dense(units=hp_units, activation='relu'))
    model.add(layers.Dense(10)) # Output layer
    model.compile(optimizer='adam', loss='mse')
    return model

tuner = kt.Hyperband(build_model, objective='val_loss', max_epochs=10)
tuner.search(x_train, y_train, validation_data=(x_val, y_val))
```

### Summary Checklist

| Layer Type | Neuron Count Rule |
| :--- | :--- |
| **Input** | Equal to number of features (columns). |
| **Hidden** | Start with **32**, **64**, or **128**. Try to keep it between Input and Output sizes. |
| **Output** | 1 (for regression/binary) OR Number of Classes (for multi-class). |
