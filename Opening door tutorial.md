# Opening door tutorial

This tutorial will show you how to make a door open on trigger or... by using a script and animations.

## 1. Setting up the scene

Assuming we a player controller in the scene, first we will create a simple door object. Of course you should use your own prefab.
We will create a couple cubes and a door object in our scene.

![image](https://user-images.githubusercontent.com/79841064/205897528-287d487a-6c03-4e64-a71b-bdec584b3eda.png)

It is important to also create a Door_Hinge empty gameobject. The door itself will sit on this so we can rotate it when we want the door to open. Don't forget to set the point of the Door_Hinge as pivot and not centre in the top right corner of the scene window. 

![image](https://user-images.githubusercontent.com/79841064/205898169-4f2efadb-1c88-4540-9394-a4127f4645b7.png)

## 2. Creating the animations

Now we will need to create an animation controller for the Door_Hinge object, and three animations. One where the door is closed, one where it is opens, and one where it stays open. First we will create the one where it opens. I called this animation Door_Opening. Press the record button in the animation window, go to the 1 minute mark on the timeline and set the Y rotation to 110. It should look like this:

![image](https://user-images.githubusercontent.com/79841064/205899221-dfc75276-8590-4bdb-a16d-e6045c7a3071.png)

