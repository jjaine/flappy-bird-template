# Flappy Bird with Unity

Final project for codebar festival 2022 workshop.

Art assets from Freepik:
- <a href="https://www.freepik.com/macrovector">Ground and icicle created by macrovector - www.freepik.com</a>
- <a href="https://www.freepik.com/">Bird created by freepik - www.freepik.com</a>

Font from Google fonts:
- <a href="https://fonts.google.com/specimen/Fredoka+One">Fredoka One</a>

Step-by-step:

### Setup sprites
1. Open project
2. Drag `bird_down` sprite from Assets > Sprites to the FlappyScene hierarchy
3. Rename the object to Bird
4. Bring in the ground similarly, drag the position so that it's at the bottom
5. Modify camera to solid color instead of skybox
6. Set up sorting layers for sprites: obstacles, ground, bird & add sprites there

### Setup colliders & first script
7. Add colliders to sprites: PolygonCollider2D for the bird, BoxCollider2D for the ground + adjust if necessary with edit mode
8. Create Bird script in the Scripts folder and add it to the Bird game object
9. Open script in VSCode by double clicking
10. Create `isDead` boolen variable and use that in the `Update()` method to check if the game is over or not
11. Create `rb` Rigidbody2D variable and get the `RigidBody2D` component to that in Start
12. Create a serialized float for `upForce`
13. In input, with `Input.GetMouseButton(0)` check if the player has pressed the mouse button and    
    * set `rb.velocity` to zero to reset the velocity
    * use `rb.AddForce` to add the force to the y coordinate
14. Get back to Unity, set `upForce` in the Inspector to 200, test if flapping works
15. Add Unity built-in `OnCollisionEnter2D` method to `Bird.cs` to check for collision with ground, and set `isDead` to true

### Set up animations
16. Create an Animations Folder in Assets
17. Open Animation Window in Unity, select Bird game object and hit the Create button, name the animation Idle and save to Animations
18. Add property, expand Sprite Renderer category, add Sprite with the + add the bottom, and expand the sprite in the view
19. Delete second keyframe
20. Select the sprite by dragging over it, hit Ctrl+C (Cmd+C on macOS) to copy, select the Idle dropdown menu and select Create New Clip...
21. Save the new clip as Flap, hit Ctrl+V (Cmd+V on macOS) to paste
22. Hit the record button and then drag the `bird_up` sprite from the Project view to the Bird game object's Sprite Renderer
23. Open the Animator window, set up conditions for the animations:
    * open the Parameters list on the list, create a Trigger parameter called Flap
    * right-click on Idle, then Make Transition and drag it to Flap
    * edit the transition by clicking on the arrow, remove Has Exit Time checkbox tick
    * add the Flap trigger to the Conditions in the Inspector
    * make a transition from Flap to Idle with Has Exit Time checked
24. Set the triggers from code:
    * create `anim` Animator variable and get the `Animator` component to that in Start
    * use `anim.SetTrigger` to change to the Flap state of the animator when the mouse button is pressed

### Game over & score texts
25. Right-click in the hierarchy and create a Text game object from UI > Text – Text Mesh Pro
26. Import the TMP Essentials when suggested
27. Change the Canvas UI Scale Mode to Scale With Screen Size and use 1920 x 1080 as the reference resolution
28. The EventSystem game object won't be needed as we're not going to interact with UI, so delete that
29. Rename then the Text (TMP) game object to Score, set the text of it to Score: 0 and change the font to the one included in the project (Fredoka One)
30. Set the font size to a larger one, set the wrapping to disabled and align the text in the middle center
31. Set the anchor point on the Canvas (hold down Alt key in the anchor view), and lift the text up a bit so that's in a nice position at the bottom
32. Duplicate the Score game object with Ctrl+D (Cmd+D on macOS) and rename the duplicated one to GameOver, set the text to Game Over and the color to black
33. Anchor it to the top and adjust the y position to a nice position at the top
34. Duplicate the score text once more, rename it to FlapToRestart and set it as a child game object to the GameOver one
35. Set the text to "Flap to restart!", reset the y position and adjust it nicely below the game over text, and also decrease the font size a bit
36. Hide the GameOver GameObject from the checkbox in the Inspector

### Game Controller
37. Create an empty GameObject by right-clicking in the hierarchy and selecting Create Empty, and name it GameController
38. Create GameController script in the Scripts folder and add it to the GameController game object
39. Create a method that handles the situation when the bird has died, birdDied in the GameController
40. Create a serialized GameObject for `gameOverText`
41. Set the `gameOverText` to active in the birdDied method
42. Create another serialized variable of type Bird for the bird script
43. Then make the Bird script's `isDead` variable a publicly gettable one (change to `IsDead`)
44. Start checking the bird's status in the game controller's Update method and call the birdDied method when the bird dies
45. Make a check that only calls the SetActive if the game object is not active already
46. Set the references to the script in Unity

### Reloading the scene after game is over
47. Add a condition to game controller's Update method to check for player input when the bird is dead
48. Add needed using statement for `UnityEngine.SceneManagement` to be able to reload the scene
49. Add the scene reloading functionality to Update

### Background scrolling
50. Create ScrollingObject script in the Scripts folder and add it to the Ground game object
51. Add a RigidBody2D component to the Ground game object and set the type to Kinematic so that it's not affected by gravity and will only be moved from script
52. Create `rb` Rigidbody2D variable and get the `RigidBody2D` component to that in Start
53. Create a serialized float for `scrollSpeed` (talk about the setting the initial value and set it in the Inspector to -2.5)
54. Set the `rb.velocity` to the scroll speed in the x direction
55. Create a `stopScrolling` method for the scrolling object script
56. Create `bird` Bird variable and get the `Bird` script to that in Start via `GameObject.Find`
57. Call the `stopScrolling` method once the bird is dead
58. Create an empty game object in the hierarchy called Scenery, move the Ground underneath and duplicate the Ground two times (make sure that Scenery is at (0, 0, 0))
59. Move the first duplicate to the right by the amount indicator by the collider, and the second two times that
60. Create RepeatingBackground script in the Scripts folder and add it to the Ground game object
61. Create `boxCollider` BoxCollider2D variable and get the `BoxCollider2D` component to that in Start
62. Create `length` float variable and get the collider's length in x direction to that in Start
63. Create `repositionBackground` method and make the ground move to the left with an offset of `length * 3`
64. Check the position in Update and if in the x direction it's less than `-length`, then call the reposition method
65. Add the script to all ground game objects in the hierarchy

### Obstacles
66. Drag `icicle` sprite from Assets > Sprites to the FlappyScene hierarchy
67. Rename the object to Icicle, scale and position it to the top part
68. Setup a PolygonCollider2D for the Icicle game object
69. Duplicate the Icicle game object, change the scale in y to negative and position nicely
70. Create an empty game object and call it Icicles, and move the two Icicles to its children
71. Add a BoxCollider2D to the Icicles game object and position it in the center of the two icicles, and set its Is Trigger on
72. Add a Rigidbody2D component to the Icicles game object and make it Kinematic
73. Create Icicles script in the Scripts folder and add it to the Icicles game object
74. Use `OnTriggerEnter2D` to detect collision between the scoring area and the bird
75. Add a public method `SetScoreCallback` that takes in an Action called `scoreCallback` and stores that in a variable called `scoreCallback` in the Icicles class
76. Add `using System;` to top for using `Action`
77. Call the `scoreCallback` when the bird scores
78. Add the ScrollingObject script to the Icicles game object
79. Create a Prefabs folder in the Project and then drag the Icicles game object there to create a prefab
80. Delete the prefab from the scene

### Scoring callback
81. Add a method called `birdScored` to the game controller script and add there also a score variable that tracks the score
82. Add `using TMPro;` at the top and then create a serialized variable called scoreText of type TMP_Text and drag the score text object to that in the Inspector
83. Update the scoreText from the `birdScored` method

### Icicle pooling
84. Create IciclePool script in the Scripts folder and add it to the GameController game object
85. Create a serialized GameObject for `icilesPrefab`
86. Create a serialized int for `poolSize`
87. Set the variables in the Inspector
88. Create an array of GameObjects in the IciclePool script to store the icicles and initialize it in Start with the `poolSize`
89. Add a variable `poolPosition` at (-50, -50)
90. Instantiate the icicle game objects to the array in Start with a loop
91. Add variables for `spawnRate` (can be serialized), `timeSinceLastSpawned` and a public variable `Active`
92. Update `timeSinceLastSpawned` in the Update method with `Time.deltaTime`
93. Create `icicleIdx` int variable to store the column index
94. Create `spawnPositionX` to control where the new columns are spawned
95. If `timeSinceLastSpawned` is larger than the spawn rate, set it to zero, create random y position for the next column and set the position of the current column to that 
96. Lastly increase the `icicleIdx` and if it's larger than or equal to `poolSize`, set it back to zero
97. Add a public method `SetScoreCallback` that takes in an Action called `scoreCallback` and stores that in a variable called `scoreCallback` in the IciclePool class and add `using System;` to the top
98. Call the `SetScoreCallback` for each Icicle created in the Start by getting the Icicles component of the `icicles[i]` game object
99. Create an IciclePool variable for `iciclePool` in the game controller script get that in Start, and call its `SetScoreCallback` in Start to set the score callback to `birdScored`
100. Set the `iciclePool.Active` to false when the bird dies to stop the pool from recycling the icicles
101. Set the Script Execution Order in Edit > Project Settings so that GameController is executed before IciclePool to ensure that the callback is set correctly