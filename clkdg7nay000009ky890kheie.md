---
title: "Kalman Filter"
seoDescription: "Intuition behind Kalman Filter"
datePublished: Sat Jul 22 2023 03:26:23 GMT+0000 (Coordinated Universal Time)
cuid: clkdg7nay000009ky890kheie
slug: kalman-filter
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700599118912/852a7b89-d938-4f2a-9ae2-4d8c0f1578bc.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1700599131444/651dddd2-5789-4dc8-bc7b-79d7526da635.png
tags: kalman-filter

---

It's an iterative algorithm to reduce noise (or uncertainty) and arrive at a really good approximate value efficiently.

Kalman filter has these high-level components:-

* **PREDICTION** - Predict the next state of the object.
    
* **MEASUREMENT** - Measure the current state of the object.
    
* **CONFIDENCE** - How good (or bad) the PREDICTED or MEASUREMENT values
    

# Intuition

Kalman filters (or variants) have the following intuition.

**CURRENT STATE**

* Suppose the current time is 3:00 pm
    

**PREDICTION over MEASUREMENT**

* After a few mins, we look at the watch again and we PREDICT it was 3:04 pm, but showed 3:55 pm!!
    
* We know the MEASUREMENT is wrong and have every reason to believe the PREDICTION is more accurate and set the watch to be 3:04 pm. Our CONFIDENCE in the MEASUREMENT is shaken, we believe it is so noisy to show 3:55 pm
    

**MEASUREMENT over PREDICTION**

On the other hand, if the measurement shows 3:05 pm, we have greater confidence that the MEASUREMENT is correct, since it's more or less in line with our expectations and since it's measured, that should be more accurate. Here we trust or more CONFIDENCE in the MEASUREMENT.

---

# 3 Main Calculations

There are 3 main calculations involved in the calculation of Kalman Filter

* Calculate the Kalman Gain
    
* Calculate the current estimate
    
* Calculate the error in the current estimate
    

Before we show the calculations, here are all the definitions used below

* \\(EST_{t-1}\\) = Previous Estimate
    
* \\(EST_{t}\\) = Current Estimate
    
* MEA = Measurement
    
* \\(E_{mea}\\) = Error in measurement
    
* \\(E_{est}\\) = Error in the estimate
    
* KG = Kalman Gain
    

## Calculate the Kalman gain

KG = \\({{E_{est}} / {(E_{est} + E_{mea})}}\\)

* Measurements are more accurate if KG is close to 1
    
* Estimates are more correct if KG is close to 0
    

## Calculate the current estimate

* \\(EST_{t} = EST_{t-1} \\) \+ KG \* \\( [MEA - EST_{t-1}] \\)
    

## Calculate the error in the estimate

* \\(E_{est_{t}}\\) = \\({E_{mea} * E_{est_{t-1}}} / {E_{mea} + E_{est_{t-1}}}\\)
    
* \\(E_{est_{t}}\\) = \\([1-KG] E_{est_{t-1}}\\)
    

What does this mean?

If measurement comes with very little error, then we are closing in on the true value. \\(E_{mea}\\) is low, then KG is high (close to 1). Then (1-KG) is very low. That means its drops the value of \\(E_{est_{t-1}}\\) significantly, hence moving towards the "true" value. Intuitively it makes sense, measurements are good, and the system should converge to a good estimate. Note that, in the real world, no matter how good the measurement is there is always noise, it will never be perfect.

The opposite is also true. Measurements are unstable, then \\(E_{mea}\\) is large, KG is low, and 1-KG is high (or close to 1). Then we can trust the measurements coming, so we do not move the needle as much and try to keep the value close to the current error in estimate value as much as possible. Here is a fantastic video explanation [here](https://www.youtube.com/watch?v=X9cC0o9viTo&list=PLX2gX-ftPVXU3oUFNATxGXY90AULiqnWT&index=4)

---

# Code

If you are interested to tinker with the code, here is the link to the Google Colab.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg align="left")](https://colab.research.google.com/drive/1B57ZdTiCcen3ay45pACVLCArkZK6Cc51?usp=sharing)

# **References**

[Michel Van Biezen Explanation for Kalman Filter](https://www.youtube.com/watch?v=PZrFFg5_Sd0&list=PLX2gX-ftPVXU3oUFNATxGXY90AULiqnWT&index=5)