# First-person-controller-tutorial

This shows how to make a simple first person controller in Unity 3D.

## 1. Setting up the scene

First we're going to create a new scene called 'First Person Controller Scene'.

Add a plane to the scene for the player to walk on, and create an empty game object which we are going to name 'First Person Player'.
add component: character controller, change radius to 0.6, height to 3.8 in Char con.
Add a capsule as a child to FPP and change scale to 1.2, 1.9, 1.2. Remove capsule collider because character controller acts as a collider.
Optional: add two new materials, make the ground brown and the player blue.
