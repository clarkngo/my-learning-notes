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
