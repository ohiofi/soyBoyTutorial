# Unity 2D Platformer: Super Soy Boy

This game is a **2D platformer** with tight controls. You&#39;ll navigate Soy Boy around obstacles while running, jumping, and wall sliding. He&#39;s in a big hurry to reach a steaming bowl of noodles at the end of the level.

You&#39;ve already learned how to manipulate sprites, create animators, and use 2D colliders. In this game you will also learn specialized physics, joints, and zone effector components.

To get started, download and extract the Starter project. Open the Game scene in Unity.

Complete the following steps to finish your first platformer game, Super Soy Boy.

## A Few Sprites

1. Locate the **single-block.png** sprite in the Sprites sub-folder then drag and drop it into the **Scene** window. You might wonder what&#39;s up with the white sprites in the Sprites folder. The answer is simple: you can easily color them dynamically in-game, or in the editor with your own color choice, without having to create several sprites for each color.
2. The single-block.png sprite will serve as a basic block for the ground (platform) that Soy Boy runs along. With that in mind, change the color to **59443CFF** then close the color picker. You will change the size and position later.
3. Select the **SuperSoyBoySpriteSheet.png** in the Sprites folder then switch **Sprite**** Mode **to** Multiple **. Click** Apply**.
4. Click the **Sprite Editor** button and **slice By Cell Size**. Set the pixel sizes to 48x40.
5. You will see 3 rows of sprites; the top row contains all the **running** animation sprites, the middle row has **jumping** , and the bottom has one **sliding** sprite. Select each sprite in the sprite editor individually and name accordingly.
    * Top row: run\_0, run\_1, run\_2, …, run\_14
    * Middle row: jump\_0, jump\_1, jump\_2, …, jump\_14
    * Bottom row: slide\_0

Click **Apply** to commit the slice changes to your sprite. **Close** the Sprite Editor.

1. In the **Project Browser** , click the expansion triangle for the **SuperSoyBoySpriteSheet.png** to reveal the sliced sprites.
2. Click and drag the **run\_0** sprite into the **Scene** view. In the **Hierarchy** , set the name to **SoyBoy**. This game object is the main character for your game.
3. Create one more sprite in your game by dragging the **top-left-corner-small.png** sprite into the **Scene** view. Enter the hex value **59443CCF**. If you run the game right now, gravity is not in play and we have not employed any physics engine attributes yet.

## Orthographic Camera

1. Now to attach a script to the camera so that it smoothly follows Soy Boy. Create a new **Scripts** folder in the project folder. Create a new **C# Script** and name it **CameraLerpToTransform** then double –click to open it in the editor.
    *	Declare the field variables to let you specify the target the camera will track, the tracking speed, and the camera’s bounds. All of these variable are public.
    ```c#
    public Transform camTarget;
    public float trackingSpeed, minX, minY, maxX, maxY;
    ```
    * Remove the **Start()** and **Update()** methods and enter the following **FixedUpdate()** method to allow the camera to track the player. The null check makes sure that a valid Transform component was assigned to the camTarget and the Vector2.Lerp performs a linear interpolation between two vectors by the third parameter&#39;s value.
    ```c#
    if (cameraTarget != null)
    {
        var newPos = Vector2.Lerp (transform.position, cameraTarget.position, Time.deltaTime * trackingSpeed);
		var camPos = new Vector3 (newPos.x, newPos.y, -10f);
		var v3 = camPos;
		var clampX = Mathf.Clamp (v3.x, minX, maxX);
		var clampY = Mathf.Clamp (v3.y, minY, maxY);
		transform.position = new Vector3 (clampX, clampY, -10f);
	}
    ```
    * Save the script and return to Unity.
2. Select the **Main Camera** in the Hierarchy. Drag the **SoyBoy** GameObject onto the **camTarget** field on the **CameraLerpToTransform** script component, which is now attached to Main Camera.
3. You&#39;ll also need to set the **minX** and **minY** values to -100, the **maxX** and **maxY** to 100, and the **Tracking Speed** to 2.5.
4. Save and then run the game. Using the **Scene** view along with the move tool, move Soy Boy around and notice how smoothly the camera follows the sprite.

## Adding 2D Physics

1. Setting up the colliders: Select the **single-block** GameObject in your heirarchy. In the Inspector, click **Add Component** and choose **Rigidbody2D**. Enable **Is Kinematic** because Soy Boy won&#39;t need to move any terrain – the blocks will be locked in place (static) and will only be moved by its transform rather than the physics engine.
2. Add a **Box Collider 2D** to the **single-block** GameObject.
3. Add a **Rigidbody2D** to **SoyBoy**. Do not change the default Rigidbody2D settings.
4. **SoyBoy** also needs an **Edge Collider 2D**. The Edge Collider 2D needs to have the following settings changed:
    * Using the Inspector panel, expand the Points node on the Edge Collider 2D component and change the size to 5 then press Enter.
    * Apply the following to each element&#39;s X and Y values:
    ```
      Element 0: (X:-0.23, Y:0.195)
      Element 1: (X:-0.23, Y:-0.192)
      Element 2: (X:0.225, Y:-0.192)
      Element 3: (X:0.225, Y:0.195)
      Element 4: (X:-0.23, Y:0.195)
    ```
    * If you select SoyBoy in the Scene view, you should see a green collider edge around him.


5. Select the **top-left-corner-small** GameObject in the Hierarchy and add a **Rigidbody2D** to it. These blocks will be part of the platform, so enable the body type **Kinematic**.
6. Add a **Polygon Collider 2D** to the **top-left-corner-small** GameObject.
7. Locate **buzzsaw.png** in the Sprites folder and drag it into the **Scene** view.
8. Select **buzzsaw** in the Hierarchy window, and add a **Rigidbody2D** and **Circle Collider 2D** component. Make sure the rigidbody is **Kinematic**.

## 2D Animation

1. In the Hierarchy, select **SoyBoy** then open the **Animation** editor (from the Window menu, choose Animation).
2. With the **SoyBoy** GameObject selected and the **Animaiton** window open, navigate to the Sprites folder in the **Project Browser**. Expand **SuperSoyBoySpriteSheet.png** to reveal the sliced sprites.
3. Select **run\_0** , press and hold **Shift** then click **run\_14** to select all. Drag and drop all selected sprites onto the **RunAnimation** window timeline.
4. Float the Animation editor and position it next to the Scene view. In the Scene view, make sure SoyBoy is above the single-block. Click the **Play** button in the **animation** window to see what SoyBoy looks like when running.
5. With the Animation window still open, follow the same process as above and create a new **JumpAnimation** with sprites **jump\_0** through **jump\_14**.
6. Make a third animation called **SlideAnimation** and place the only slide sprite, **slide\_0** into the timeline.
7. Make a final animation for SoyBoy called **IdleAnimation** and place the **run\_0** sprite into the timeline.

## Layers and Sorting

1. Remember, arranging sprites is similar to working with layers when you&#39;re editing photos. Unity uses the order to sort sprite visibility first. Then within each layer, it sorts sprites further with and order number such as 0, 1, 2, and so on, with 0 being the very bottom sub-layer. Unity places new sprites on the Default sorting layer with an Order in Layer number of 0. If you set a stack of sprites in the same location but don&#39;t change this, some sprites might be hidden while others randomly appear and disappear.
2. Navigate to the **Edit\ProjectSettings\Tags**** and Layers** to bring the Tags and Layers editor in the Inspector.
3. Expand the **Sorting Layers** node. Click the + button three times to add three sorting layers. Name them **Background** , **Hazards** , and **Foreground**.
4. Make sure the order is as follows:
    * Default
    * Background
    * Hazards
    * Foreground
    * UI
Remember the higher in the stack, the lower it renders on the screen. For instance, the Background layer will render near the bottom of the stack, and the UI layer will render above everything else.

### What do the layers contain?
 * Default – a default layer provided by Unity
 * Background – Background sprites, such as mountains, scenery or background blocks that don&#39;t obstruct the player&#39;s path will go there.
 * Hazards – Hazards and traps such as the saw blade will go in this layer.
 * Foreground – Soy Boy will be placed here along with most level objects.
 * UI – another default layer that is specified for rendering UI elements such as buttons and canvases

### Reorganize

1. Reorganize the sprites with the Sorting Layers.
    * Select the SoyBoy GameObject in the scene. In the Inspector, look for the Sprite Renderer component. Change the Sorting Layer from Default to Foreground and change the Order in Layer to 1.
    * Select buzzsaw and adjust the settings in the Sprite Renderer to use the Hazard Sorting Layer with an Ordering In Layer value of 1.
    * Select the single-clock and top-left-corner-small GameObjects and change the Sorting Layer to Foreground and Ordering In Layer to 0.
2. Use the move tool to arrange the sprites in the Scene view so they overlap slightly, like below to check your ordering. Foreground sprites are on top, and the Hazard layer is farthest back.


## Prefabs and Resources

1. You have used Prefabs in previous projects, but using Resources is a new concept introduced in this project. Unity has a Resources class that lets you access assets in your project via code – any assets placed in a sub-folder of the Assets folder that bears the name Resources csn be accessed via **Resources.Load()** methods.
2. Create a new folder called **Resources** into the Assets folder.
3. Create a new sub-folder called **Prefabs**.
4. One-at-a-time, drag the **single-block** , **top-left-corner-small** , **buzzsaw** , and **SoyBoy** GameObjects from the Hierarchy, and then drop them into the new **Prefabs** folder. This is a way to keep field values as you change levels in your game.

## Scripting Basic Controls

1. Before we can test the scripts we are going to enter, we need a ground for SoyBoy to run on.
    * In the **Scene** view, make 7 clones of the **single-block** GameObjects prefab and set them next to each of the in a line. Make sure the **Transform Y position** is the same for all 8 single-blocks.
    * Place **SoyBoy** above the row of blocks.
2. In the **Project Browser** , right-click the **Scripts** folder and create a new **C# Script**. Name it **SoyBoyController**. Double-click it to open the script in your editor.
3. At the top of the script, just above the **public class SoyBoyController** definition, add the following:
   ```c#
    [RequireComponent(typeof(SpriteRenderer),typeof(Rigidbody2D,typeof(Animator))]
    ```
    This line ensures that the script only applies to GameObjects that have the minimum components required to work.

4. Now add the following variables inside the **SoyBoyController** definition above the **Start()** method.
	```c#
   	public float speed = 14f;
	public float accel = 6f;
	private Vector2 input;
	private SpriteRenderer sr;
	private Rigidbody2D rb;
	private Animator animator;
	```
5. Add a new method called Awake() above the Start() method. This method will ensure all component references are cached when the game starts.
	```c#
	sr = GetComponent<SpriteRenderer> ();
	animator = GetComponent<Animator> ();
	rb = GetComponent<Rigidbody2D> ();
	```
6. Now make sure SoyBoy knows which direction to face by using the Input class. Add the following code to the **Update()** method. If the x input is greater than 0 the player faces right, so the sprite gets flipped on the X-axis. Otherwise, the player must be facing left, so the sprite is set not to flip.
	```c#
	input.x = Input.GetAxis ("Horizontal");
	input.y = Input.GetAxis ("Jump");
	if (input.x > 0f)
	{
		sr.flipX = false;
	}
	else if (input.x <0f)
	{
		sr.flipX = true;
	}
	```
7. Add the newly created script to the SoyBoy GameObject.
8. Open the **SoyBoyController** script again and delete the **Start()** method then add the **FixedUpdate()** code below.
	```c#
	var acceleration = accel;
	var xVelocity = 0f;
	if (input.x == 0)
	{
		xVelocity = 0f;
	}
	else
	{
		xVelocity = rb.velocity.x;
	}
	rb.AddForce (new Vector2 (((input.x * speed) - rb.velocity.x) * acceleration, 0));
	rb.velocity = new Vector2 (xVelocity, rb.velocity.y);
	```


9. SoyBoy now runs, but doesn&#39;t jump yet. However, if you run off the end of the ground blocks you might notice that SoyBoy rotates or spins. This happens when force is applied to a collider edge. To fix this you need to lock the RigidBody&#39;s Z rotation. Select the **SoyBoy** GameObject, and expand the **Constraints** node in the **RigidBody2D** component. Check **Freeze Rotation**  for **Z** to lock this rotation in 2D.
10. One more change for falling, which will be noticed more during the jump effect. Navigate to **Edit\Project Settings\Physics2D**. Change **Y** to **-30** in the Gravity settings.
11. Lastly, select **SoyBoy** GameObject in the Hierarchy and change the **Collision Detection** on the **RigidBody2d** component to **Continuous**.
12. Save the scene and then play the game. You should see SoyBoy fall faster when he goes off the end of the floor.



