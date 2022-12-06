# Opening door tutorial

This tutorial will show you how to make a door open on trigger by using a script and animations.

## 1. Setting up the scene

Assuming we a player controller in the scene, first we will create a simple door object. Of course you should use your own prefab.
We will create a couple cubes and a door object in our scene.

![image](https://user-images.githubusercontent.com/79841064/205897528-287d487a-6c03-4e64-a71b-bdec584b3eda.png)

It is important to also create a Door_Hinge empty gameobject. The door itself will sit on this so we can rotate it when we want the door to open. Don't forget to set the point of the Door_Hinge as pivot and not centre in the top right corner of the scene window.

![image](https://user-images.githubusercontent.com/79841064/205898169-4f2efadb-1c88-4540-9394-a4127f4645b7.png)

We will also add a Box Collider to Door_Hinge, we will use this collider to detect the player and open the door if we walk into the collider. We need to set this collider to trigger and make it about this big:

![image](https://user-images.githubusercontent.com/79841064/205906382-56d5333d-ca9b-4a33-9cd6-4a4dbd99390a.png)

## 2. Creating the animations

Now we will need to create an animation controller for the Door_Hinge object, and three animations. One where the door is closed, one where it is opens, and one where it stays open. First we will create the one where it opens. I called this animation Door_Opening. Press the record button in the animation window, go to the 1 minute mark on the timeline and set the Y rotation to 110. It should look like this:

![image](https://user-images.githubusercontent.com/79841064/205899221-dfc75276-8590-4bdb-a16d-e6045c7a3071.png)

Now we will create a new animation, call it Door_Open, and copy the keyframe from the Door_Opening animation at the 1 minute mark, and paste it in for the Door_Open animation at the 0 second mark.

![image](https://user-images.githubusercontent.com/79841064/205899887-80134735-a6d2-40b2-a8c3-04a0ba0a2675.png)

We will do the same thing for the last animation, Door_Closed. For this we copy the first keyframe from Door_Opening, and paste it in for Door_Closed.

In the animator window we need to set the Door_Closed animation as the default state, and make transitions from one to another like so:

![image](https://user-images.githubusercontent.com/79841064/205900440-1848aea5-3779-48ac-90a0-16000c8c2bc7.png)

We will also create a new parameter, a bool, and called it PlayerOpen.

![image](https://user-images.githubusercontent.com/79841064/205900666-63038cf5-693e-4544-bd78-ddd5bf079a80.png)

We need to add this parameter as a condition to the first, third transition. For the one between Door_Closed and Door_Opening we leave it on true.

![image](https://user-images.githubusercontent.com/79841064/205901115-647a9d1f-f174-41ee-b579-2aaa8544aede.png)

For the one between Door_Open and Door_Closed, we set it to false.

![image](https://user-images.githubusercontent.com/79841064/205901250-641de0b9-f057-4b71-9e2e-3cf886798db3.png)

## 3. Writing the script

First we will need to decleare the animator and reference it in the Start function.

```.cs
    public Animator Animator;
    
    // Start is called before the first frame update
    void Start()
    {
        Animator = GetComponent<Animator>();
    }
```

Then we need to detect the player entering and leaving the collider.

```.cs
    private void OnTriggerEnter(Collider other)
    {
        if(other.tag =="Player")
        {
            Animator.SetBool("PlayerOpen", true);
        }
    }

    private void OnTriggerExit(Collider other)
    {
        if(other.tag == "Player")
        {
            Animator.SetBool("PlayerOpen", false);
        }
    }
```

## 4. Finishing up

Coming back to Unity, we just need to put our script on the Door_Hinge object, drag Door_Hinge into the animator slot, and put the Player tag on your character controller if you haven't already done so.

![image](https://user-images.githubusercontent.com/79841064/205908413-87d89dd1-2c6f-422e-a763-fc096e12ee55.png) 

Now the door should open whenever you come into the collider and close when you leave it.

![image](https://user-images.githubusercontent.com/79841064/205909577-823ac57b-b32f-45db-b0cb-fa16207b6f5f.png)
