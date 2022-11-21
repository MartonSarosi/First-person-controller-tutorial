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

Lastly, we will give TP a tag of TP, and TP2 a tag of SecondTP.

![image](https://user-images.githubusercontent.com/79841064/203150880-f40e3d57-5660-4101-9f33-946dc8e3e564.png)
![image](https://user-images.githubusercontent.com/79841064/203150910-82d0abc3-105c-43ee-a094-66577c9f0c80.png)

## 2. Making the script

Create a new script and attach it to your player. I will call the script Teleport.
Inside the script we can delete the Start and Update functions.

First we will declare some variables.
```.cs
    public GameObject thePlayer;
    public Transform startTP;
    public Transform secondTP;
```

Next we will create an OnTriggerEnter function to check if the player has come in contact with one of the teleporters, and they it have, we will change the player's location to the other teleporter location.
```.cs
private void OnTriggerEnter(Collider collision)
    {
        if (collision.gameObject.CompareTag("TP"))
        {
            thePlayer.transform.position = secondTP.transform.position;
        }

        if (collision.gameObject.CompareTag("SecondTP"))
        {
            thePlayer.transform.position = startTP.transform.position;
        }

    }
```

## 3. Finishing up

Coming back to our scene, now we need to assign a couple things in the inspector. Go to your player/character controller where the script is and assing the player and the TP locations in their respective slots.

![image](https://user-images.githubusercontent.com/79841064/203151579-2bb4c57a-7601-46b9-b0e8-d043c146c2f6.png)

We're good to go. If you press play, you should see that if you walk onto the purple platform (TP), it will teleport you onto the other yellow platform (TP loc2), and vice versa.
