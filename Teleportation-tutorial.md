# Teleportation tutorial

This tutorial will take you through making a teleporter in Unity.

## 1. Setting up the scene

For this tutorial I will assume you have your own character controller and an environment (at the very least a plane) where you can walk around.
First, we will add a cube to our scene. I will name this cube TP. You can resize this cube or remove the mesh renderer depending on your needs. I will resize it to make it look like a platform and make it purple.

![image](https://user-images.githubusercontent.com/79841064/203146990-9d591b9e-5161-475f-a216-f5ea04f0409b.png)

We also have to make sure that the collider of this object is set to Is Trigger. 

![image](https://user-images.githubusercontent.com/79841064/203147117-330b931e-b335-42a6-9dc1-9caaa0057bd4.png)

Next, we will duplicate the purple cube, make this one yellow, rename it to "TP location", move it next to our TP object, make it a child object of TP and remove the box collider component from this new object.
Our scene should look like this: 

![image](https://user-images.githubusercontent.com/79841064/203148368-8c5e3915-29b9-4eea-92da-adb7be284573.png)

Now we can duplicate TP, call it TP2 and call TP location in TP2 TP location2, like this:

![image](https://user-images.githubusercontent.com/79841064/203148935-9540eb51-824e-45d0-a440-4d64defd4209.png)

Move TP2 into a different location, however far you want.

![image](https://user-images.githubusercontent.com/79841064/203149263-f5143d4a-0542-494d-a3e7-a768c695831f.png)

## 2. Making the script

Create a new script and attach it to your player. I will call the script Teleport.
Inside the script we can delete the Start and Update functions.
First we will declare some variables.
```.cs
    public GameObject thePlayer;
    public Transform startTP;
    public Transform secondTP;
```
