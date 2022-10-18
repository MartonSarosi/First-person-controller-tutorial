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


We also want to limit the camera's rotation to 180 degrees, this is called clamping (flipping the camera to a point behind the player).

It's time to add a script to the Main Camera called mouseLook.
In  the script, first we will assign a public float for our mouse sensitivity, a transform for our body and a float called xRotation that will help us with clamping.
```.cs
public float mouseSens;
public Transform playerBody;
float xRotation = 0f;
```
and than we will have these lines so that we can rotate our camera on the X and the Y axis.
```.cs
void Update()
{
        float mouseX = Input.GetAxis("Mouse X") * mouseSens * Time.deltaTime;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSens * Time.deltaTime;

        playerBody.Rotate(Vector3.up * mouseX);

        xRotation -= mouseY;
        transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
}        
```
Now we will need to tell the camera that it cannot rotate behind the player's head, so we'll clamp it between -90 and 90 degrees. (adding the last line to the update function)
```.cs
void Update()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSens * Time.deltaTime;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSens * Time.deltaTime;

        playerBody.Rotate(Vector3.up * mouseX);

        xRotation -= mouseY;
        transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
        xRotation = Mathf.Clamp(xRotation, -90f, 90f);
    }
```
Now we're just going to add a line a of code to hide and lock our cursor to the centre of our screen so that when we're looking around, our cursor won't be visible and leave the screen.
```.cs
void Start()
    {
        Cursor.lockState = CursorLockMode.Locked;
    }
```
Finally, we are just going to assign a value to our sensitivity and drag our FPP from the Hierarchy into the Player Body slot.
![image](https://user-images.githubusercontent.com/79841064/196407291-10a7c1a9-7738-4399-a90f-b8b1bfd2d1fa.png)

## 3. Movement

To implement the movement we wil make a script called playerMovement and attach it to the First Person Player.
