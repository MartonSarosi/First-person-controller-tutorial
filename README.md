# First-person-controller-tutorial

This shows how to make a simple first person controller in Unity 3D.

## 1. Setting up the scene

First we're going to create a new scene called 'First Person Controller Scene'.

Add a plane to the scene for the player to walk on, and create an empty game object which we are going to name 'First Person Player'.
add component: character controller, change radius to 0.6, height to 3.8 in Char con.
Add a capsule as a child to FPP and change scale to 1.2, 1.9, 1.2. Remove capsule collider because character controller acts as a collider.
Optional: add two new materials, make the ground brown and the player blue.
Drag the Main Camera under the FPP. Reset the transform of the camera and move it up almost to the top of the player but still leaving a bit of space so that if we jump into a ceiling, the camera doesn't clip through. Make sure that the camera is inside our capsule object so that the capsule is not visible to the camera itself.

## 2. Looking around

If we move our mouse on the X axis, meaning looking around horizontally we want our player to rotate around the Y axis.

![image](https://user-images.githubusercontent.com/79841064/195070189-b084a72a-4ccb-40ef-92bc-111cef569c98.png)

However if we move our mouse on the Y axis, meaning looking up or down, we don't want our entire player to rotate

![image](https://user-images.githubusercontent.com/79841064/195070828-61cf84f0-f0d2-41d2-afac-dd3dd6e18b33.png) 

only the camera.

![image](https://user-images.githubusercontent.com/79841064/195071112-8c679eaa-0267-4c59-920c-b3631f83ccee.png)
