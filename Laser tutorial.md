# Laser tutorial

In this tutorial I will showcase how to make a laser beam in Unity with a simple script, using Raycast, and using LineRenderer inside Unity.

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
Inside the start function we also need to get a referance to our LineRenderer

```.cs
void Start()
    {
        lr= GetComponent<LineRenderer>();
    }
```

In the update method we will set the position of the laser as our starPoint, than we will use Raycast to check where the laser hits, and make that location the point where the laser stops.

```.cs
void Update()
    {
        lr.SetPosition(0, startPoint.position);
        RaycastHit hit;
        if (Physics.Raycast(transform.position, -transform.right, out hit))
        {
            lr.SetPosition(1, hit.point);
        }
    }
```

You can always use other if statements so that the laser damages or kills enemies and/or the player when it comes into contact with them. In simple demo you could just destroy the player object or enemy. That would look like this:

```.cs
void Update()
    {
        lr.SetPosition(0, startPoint.position);
        RaycastHit hit;
        if (Physics.Raycast(transform.position, -transform.right, out hit))
        {
            lr.SetPosition(1, hit.point);
            
            if (hit.transform.tag == "Player")
            {
                lr.SetPosition(1, hit.point);
                Destroy(hit.transform.gameObject);
            }
        }
    }
```

If you already have a separate script that tracks the player's health and the damage he takes you could call that function from inside the if statement.

To finish off will need to add an else statement so that the laser doesn't go endlessly into the distance if it doesn't hit anything, but stops after a given distance. You can change how far this distance is, for me it's 5000.

```.cs
void Update()
    {
        lr.SetPosition(0, startPoint.position);
        RaycastHit hit;
        if (Physics.Raycast(transform.position, -transform.right, out hit))
        {
            lr.SetPosition(1, hit.point);

            if (hit.transform.tag == "Player")
            {
                lr.SetPosition(1, hit.point);
                Destroy(hit.transform.gameObject);
            }
        }
        else lr.SetPosition(1, -transform.right * 5000);
    }
```

## 3. Back to Unity

First we will need to assign startPoint in the inspector. For this we will create an empty GameObject that is a child of our LaserShooter. Call it StartPoint and drag it into the Start Point slot in the inspector.

![image](https://user-images.githubusercontent.com/79841064/204008126-92e953b7-2ec5-4146-99e7-1703b3010cf7.png)

Finally, we need to adjust a couple settings for the LineRenderer. First we need to decrease the width, for me it is as close to 0 as posibble but for you it could be different, and we need to assign a material for the LineRenderer.

![image](https://user-images.githubusercontent.com/79841064/204008469-b51ff259-9a4d-4658-a2df-92ac92039b60.png)

Now if you press play, your laser should be shooting from the start position and stopping whenever an object intercepts it.
