# Laser tutorial

In this tutorial I will showcase how to make a laser beam in Unity with a simple script and using LineRenderer inside Unity.

## 1. Setting up the scene

First we will add a new cube to our scene, call it LaserShooter, and resize it. (Or if you have your own model, use that.)

![image](https://user-images.githubusercontent.com/79841064/203997819-c636f285-3833-45b0-b7ae-5175c8c58d36.png)

We'll add a LineRenderer component to this object a new script called Laser.

## 2. The script

Inside the script we will first declare some variables.

```.cs
    private LineRenderer lr;
    public Transform startPoint;
```
