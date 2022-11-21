# First-person controller tutorial

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

To implement the movement we will make a script called playerMovement and attach it to the First Person Player. We will need a referance to our player controller, a speed value, get some input on the horizontal and vertical axises and of course use Time.deltaTime so that the speed which the player is moving by won't be affected by how good their computer specs are.
```cs
public CharacterController controller;

    public float speed;

    // Update is called once per frame
    void Update()
    {
        float x = Input.GetAxis("Horizontal");
        float z = Input.GetAxis("Vertical");

        Vector3 move = transform.right * x + transform.forward * z;

        controller.Move(move * speed * Time.deltaTime);
    }
```
Now will just have to set our speed in the Unity editor and also drag our controller in the right slot. ![image](https://user-images.githubusercontent.com/79841064/197738582-ae32bcb1-c675-4d2e-9f34-c691db7c2cb8.png)

## 4. Gravity

First add some stairs and a slope to our scene that we can walk up.
![image](https://user-images.githubusercontent.com/79841064/197741693-d4f07430-1e89-4f03-ada4-9c6dbaa87146.png)

We will need to set the Step Offset in the Character Controller to 0.7 so we actually walk up the stairs.
![image](https://user-images.githubusercontent.com/79841064/197742133-03d9e58a-b29a-49c8-85bf-709d50b6cf13.png)

Now we're able to walk up the stairs but there's no gravity applied.
![image](https://user-images.githubusercontent.com/79841064/197742768-44629be9-7fa5-462f-8827-92af1f407cd0.png)

We will start by implementing our velocity (This all goes into the playerMovement script)
First add a reference for the velocity and  assign a float for the gravity at the top.
```cs
    Vector3 velocity;
    public float gravity = -9.81f;
```
Now inside Update we will increase our velocity on the y axis.
```cs
        velocity.y += gravity * Time.deltaTime;
        controller.Move(velocity *Time.deltaTime);
```
At this point we are constantly building up velocity so we're going to add an empty game object called GroundCheck to the Player.
![image](https://user-images.githubusercontent.com/79841064/197746491-3dba82da-f618-45ba-b194-dd01692a78df.png)

Now going back to our playerMovement script we will need a reference for our groundCheck, a float for our groundDistance, a LayerMask because we want to control what kind of objects to check for and finally we will assign a bool that will store if we are grounded or not.
```cs
    public Transform groundCheck;
    public float groundDistance = 0.4f;
    public LayerMask groundMask;
    bool isGrounded;
```
Now in our update we will add a line to create a small invisible sphere at the bottom of our Player, with the radius specified and if it collides with anything that is in our groundMask, than isGrounded is going to be true.
```cs
    isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);
```
Finally we will be making sure that if we are on the ground our velocity stays 0, but because our sphere might register before we completely hit the ground we will actually set the velocity to -2 and this is all in the Update function.

```cs
        if(isGrounded && velocity.y < 0)
        {
            velocity.y = -2f;
        }
```
Now inside the Unity editor first we will create a new layer called Ground, then we will go to our player, drag our GroundCheck object into the Ground Check slot and set the Ground Mask to Ground. You should also select the plane and the stairs and slope and change their layer to ground. (Baisically anything that the player is going to be walking on).

![image](https://user-images.githubusercontent.com/79841064/197752676-e9c15d3f-0437-400a-9aff-eae3e87e7523.png)

## 5. Jumping

First we will assign a new float called jumpHeight that we'll be able to change inside the edior.
```cs
    public float jumpHeight;
```
Then we will put this if statement inside our Update function to get the player jumping.
```cs
if(Input.GetButtonDown("Jump") && isGrounded)
        {
            velocity.y = Mathf.Sqrt(jumpHeight * -2 * gravity);
        }
```

That will do it for this tutorial. Now we have a working first-person controller that can look around, move in any direction and jump.
