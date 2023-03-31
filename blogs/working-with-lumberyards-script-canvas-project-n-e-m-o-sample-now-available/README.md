# Working with Lumberyard’s Script Canvas: Project N.E.M.O Sample Now Available

<p><img class="aligncenter size-large wp-image-1778" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/BlogHeader1200x400_Final-1024x341.jpg" alt="" width="1024" height="341"></p> 
       <p>Today, we’re excited to introduce a ready-to-use Amazon Lumberyard sample called Project N.E.M.O to help you get started with <a href="https://github.com/awsdocs/amazon-lumberyard-user-guide/blob/master/doc_source/script-canvas-intro.md" target="_blank" rel="noopener">Lumberyard Script Canvas</a>&nbsp;, our visual scripting tool.</p> 
       <p>We first gave you a sneak peek of Project N.E.M.O at GDC 2019. And following the release of <a href="https://github.com/awsdocs/amazon-lumberyard-release-notes/blob/master/archive/1.19/index.md" target="_blank" rel="noopener">Lumberyard 1.19</a>, we’re happy to give you the full download so you can quickly get to grips with some of the features and workflows used to script common game play functions.</p> 
       <p>Project N.E.M.O (Nautical Emergency Maneuvers and Operations)</a> is a complete game level sample that showcases some of the latest capabilities of Script Canvas. It’s a short obstacle navigation experience using a 3rd person camera to control a mini sub through an underwater mine field along with pickups to collect along the way.</p> 
       <p>&nbsp;</p> 
       <p><a href="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/nemo-ly2.gif"><img loading="lazy" class="aligncenter wp-image-1810 size-full" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo-ly2.gif" alt="Project N.E.M.O." width="1280" height="720"></a></p> 
       <p>When we first approached the challenge of building a new scripting system in Lumberyard, we wanted to build something that would benefit developers of all sizes – regardless of skill or expertise. Something that users with limited scripting skills could easily master, and at the same time provide a highly powerful tool for veteran game developers. On a technical level, it was also important that graphs stay small and focused on the immediate tasks, while providing the ability to talk across other scripts as needed.</p> 
       <p>Using these goals, we’ve created a scripting solution that offers two key elements. First, scripts are self-contained. Which means they allow clear direction to understand what functions are called. Second, they are easy to re-use or replace. Script files are their own file format and accessed by other entities or other events. Should it be required, this is a simple swap within an entities component properties. Node based scripting can get large and unwieldly when you need to address every form of functionality. Our approach allows for event driven scripts, letting you produce small scripts that don’t require complex logic to maintain states.</p> 
       <p>Below we’ve outlined the different game functions we created for N.E.M.O. It covers submarine functionality, mines and squid behavior, HUD display, pickups, world space UI, game ending and script events.</p> 
       <h2>Submarine Slice</h2> 
       <p>The submarine slice is the most complex element of this sample as it consists of all the visual, behavior, and player control elements.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1780" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo1.png" alt="" width="731" height="512"></p> 
       <p>The entity hierarchy for the Submarine is as follows:</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1781" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo2.png" alt="" width="308" height="350"></p> 
       <p>Entities are grouped within parent entities to keep the submarine’s hierarchy organized and tidy. The different behaviors and effects that make up the submarine slice are grouped accordingly. Here’s a description of each:</p> 
       <ul> 
        <li><strong>Meshes</strong> – This holds purely visual entities, they do not have any behavior, physics, or special effects.</li> 
        <li><strong>Effects</strong> -This holds all entities that have a particle effect component.</li> 
        <li><strong>Environment</strong> – This holds entities that are used to improve the art environment around the submarine.</li> 
        <li>I<strong>llumination</strong> – Underwater games tend to be dark. To make the submarine really standout we added many lights around it for cosmetic purposes.</li> 
        <li><strong>Weapons</strong> -These entities have the components necessary for weapon behaviors, capturing user input and spawning projectiles.</li> 
        <li><strong>Camera</strong> – The player’s camera is bound at a distance from the submarine in order to control how the game looks.</li> 
        <li><strong>Propeller</strong> – This is an animated element of the submarine. When it moves forward the propeller spins clockwise. The behavior is procedurally provided in Script Canvas.</li> 
        <li><strong>Physics</strong> – This holds the elements required to perform collision detection against other physical entities.</li> 
        <li><strong>Bubblers</strong> – These effect entities have a particle component that emits bubbles as the submarine moves, and adjusts depending how fast it moves. The submarine is saved as a slice, this is a good practice to use when making complex entities as it allows you to create new instances of objects, as well as override parameters. And through slices, you can create varieties of objects.</li> 
        <li><strong>Movement</strong> – The submarine’s movement is generated by directly modifying the entity’s transform. This was a design decision in order to keep the demo easy to understand. However, there is a tradeoff. Because of this style of movement the submarine does not collide against the environment.&nbsp;In addition to player movement, we are also doing some procedural animation on the submarine itself; this gives the submarine a subtle “floating” motion to simulate being underwater.</li> 
       </ul> 
       <h2>Mine Slice</h2> 
       <p>Mines are explosive objects that spawn randomly from a <strong>Random Timed Spawner</strong> that spans the length of the playable space. They are physical objects that rely on gravity and buoyancy in order to give a realistic “falling through water” effect, as well as create hazards for the player to destroy or navigate around.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1782" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo3.png" alt="" width="731" height="849"></p> 
       <p>The Script Canvas graph for mines is responsible for detecting a collision and spawning an explosion when it happens.</p> 
       <p>When a mine explodes it also needs to deactivate itself in order to release the entity.</p> 
       <p>If the mine is destroyed by a player’s projectile&nbsp; it spawns the a world space UI and sends the DisplayWorldScore Script Event in order to update the value of the score to display and update the player’s score on the HUD.</p> 
       <h2>Squid Slice</h2> 
       <p>The squid consists of two parts. The bottom part (the tentacles) are a separate entity from the top part. This allows the rotation of the tentacles separately from the body.</p> 
       <p>The body of the squid has a spawner component from which it randomly spawns projectiles.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1783" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/Nemo4.png" alt="" width="731" height="758"></p> 
       <p>Squids move vertically throughout their lifetime, if destroyed by the player they will spawn an explosion and deactivate themselves, otherwise they will continue moving vertically until their lifetime expires at which point they’ll deactivate themselves.</p> 
       <h2>HUD (Heads Up Display)</h2> 
       <p>The HUD in this demo consists of four elements; the game logo on the top left corner that fades out over time when the game starts, the score on the top right, ammo count on the bottom left, and the health on the bottom right.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1784" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo5.png" alt="" width="731" height="556"></p> 
       <p>In addition to the HUD there is also a damage feedback screen that is faded into view briefly when the player takes damage from mines or squid projectiles.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1785" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo6.png" alt="" width="731" height="700"></p> 
       <h2>HUD UI Canvases</h2> 
       <ul> 
        <li><strong>Logo.uicanvas</strong> Displays the game’s logo on the top right when the game starts and fades away.</li> 
        <li><strong>HUD.uicanvas</strong> Manages the display of the player’s health, ammo &amp; score.</li> 
        <li><strong> Damage.uicanvas</strong>&nbsp;Displays a full screen image to communicate that the player has taken damage.</li> 
        <li><strong>GameOver.uicanvas</strong>&nbsp;Displays the end of game screens (Success or Failure) if the player loses all health or crosses the trigger at the end of the level.</li> 
       </ul> 
       <h2>Pickups</h2> 
       <p>Pickups allow the player to collect a score. It works by using a trigger area as the mechanism to detect if a player has collided with it. The “On Area Entered” event from the trigger component handles the Script Canvas graph. It’s used to notify the game that a pickup occurred, and to spawn a world space UI entity that displays the score the player will receive for collecting this pickup.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1786" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo7.png" alt="" width="731" height="941"></p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1787" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo8.png" alt="" width="731" height="766"></p> 
       <h2>World Space UI</h2> 
       <p>When the player collects coins or destroys squids an entity that displays the score is spawned briefly.</p> 
       <p>This uses render to texture in order to render UI in world space, the entity consists of the following components:</p> 
       <ul> 
        <li><strong>Look At Component</strong> – Aligns the entity to another specified entity, in this case, the player’s camera.</li> 
        <li><strong>Mesh</strong> – Uses a primitive_plane with a material override, the worldui000.mtl material. Notice that the Diffuse texture map is set to $worldscore, we will use this in order to bind the UI Canvas render to target with the texture map in this material.</li> 
        <li><strong>Script Canvas</strong> – Controls the movement of the entity, updating the UI canvas with the value of the score, fading out and destroying the entity.</li> 
        <li><strong>UI Canvas on Mesh</strong> – Allows us to specify a Render target override in order to use the render to target feature, we set this to $worldscore to match the Diffuse texture map of the material.</li> 
        <li><strong>UI Canvas Asset Ref</strong> – The UI Canvas for the score display.</li> 
       </ul> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1788" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo9.png" alt="" width="849" height="679"></p> 
       <h2>Game End</h2> 
       <p>Game end is determined in two ways, the first is if the player’s health reaches 0, in this case we display a mission failed screen.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1789" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo10.png" alt="" width="731" height="499"></p> 
       <p>The player crossing an “end of game” trigger determines mission success.</p> 
       <p><img loading="lazy" class="aligncenter size-full wp-image-1790" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo11.png" alt="" width="731" height="527"><img loading="lazy" class="aligncenter size-full wp-image-1791" src="/blogs/working-with-lumberyards-script-canvas-project-n-e-m-o-sample-now-available/images/nemo12.png" alt="" width="731" height="558"></p> 
       <h2>Script Events</h2> 
       <p>Script Events communicate information &amp; data across entities and Script Canvas graphs. They allow us to create an event sent directly, or as a general broadcast and received then handled by. Below are descriptions of each:</p> 
       <h3>Game Events (Broadcast)</h3> 
       <p>This script event is used to communicate game level events, when the player crosses the end of level trigger or when the player’s health reaches 0. This displays the mission success/failed screens as well as displays the player’s total score.</p> 
       <p><strong>Game Over &lt;Score, Success&gt;</strong></p> 
       <p>Used to notify that the game is over. The score and the players success (or not), displays the correct screen.</p> 
       <h3>Player Events (Broadcast)</h3> 
       <p>These are game time events related to player actions:</p> 
       <p><strong>OnWeaponFire &lt;AmmoCount:Number&gt;</strong><br> When the player fires, this communicates to the HUD the remaining player’s ammo.</p> 
       <p><strong>Collision &lt;Force:Number, Type:String&gt;</strong><br> When the player collides against a mine or enemy projectile, this communicates the force of the collision and the type of damage.</p> 
       <p><strong>Score &lt;Value:Number&gt;</strong><br> When the player collects a coin, this communicates the points received in order to update the player’s score.</p> 
       <h3>WorldUI (EntityId)</h3> 
       <p>These are events used to communicate with the world space UI element that is spawned when the player collects coins or defeats squids.</p> 
       <p><strong>DisplayWorldScore &lt;Score:Number&gt;</strong><br> This communicates to the world UI entity the value it should display depending on the score received for either picking up a coin or destroying a squid. It addresses directly to the world UI entity in order for it and only it to update and display this value.</p> 
       <h3>Pickup (Broadcast)</h3> 
       <p><strong>OnPickup &lt;Tag,&nbsp;Value&gt;</strong><br> Used to notify that the player has picked up an item, what kind of item and the value is used to determine the score.</p> 
       <h3>Input Bindings</h3> 
       <h3>Strafe</h3> 
       <p>This input binding controls the left/right movement of the submarine, it is bound to the gamepad’s left thumbstick X axis, and the A and D keys on the keyboard.</p> 
       <p><strong>gamepad:</strong> gamepad_thumbstick_l_x</p> 
       <p><strong>keyboard:</strong> keyboard_key_alphanumeric_A</p> 
       <p>Event value multiplier -1.0</p> 
       <p><strong>keyboard:</strong> keyboard_key_alphanumeric_D</p> 
       <h3>Pitch</h3> 
       <p>This input binding controls the up/down movement of the submarine, it is bound to the gamepad’s left thumbstick Y axis, and the Q and E keys on the keyboard</p> 
       <p><strong>gamepad:</strong> gamepad_thumbstick_l_y</p> 
       <p><strong>keyboard:</strong> keyboard_key_alphamumeric_Q</p> 
       <p>Event value multiplier: =-1.0</p> 
       <p><strong>keyboard:</strong> keyboard_key_alphanumeric_E</p> 
       <h3>Accelerate</h3> 
       <p>This input binding controls the forward and backwards motion of the submarine. It is bound to the gamepad’s right trigger and the W and S keys on the keyboard</p> 
       <p><strong>gamepad:</strong> gamepad_trigger_r2</p> 
       <p><strong>keyboard:</strong> keyboard_key_alphamumeric_W</p> 
       <p><strong>keyboard:</strong> keyboard_key_alphanumeric_S</p> 
       <p>Event value multiplier: = -1.0</p> 
       <h3>ShootBoth</h3> 
       <p>This input binding controls the ability to shoot projectiles from the submarine. It is bound to the gamepad’s A button, the space bar on the keyboard and the left mouse button</p> 
       <p><strong>gamepad:</strong> gamepad_button_a</p> 
       <p><strong>keyboard:</strong> keyboard_key_edit_space</p> 
       <p><strong>mouse:</strong> mouse_button_left</p> 
       <h2>Install Steps</h2> 
       <p>To Install the N.E.M.O sample, copy the downloaded .zip file to your Lumberyard 1.19 dev folder.&nbsp; Unzip the files. Open Project Configurator and follow the <a href="https://github.com/awsdocs/amazon-lumberyard-user-guide/blob/master/doc_source/configurator-projects.md" target="_blank" rel="noopener">user guide</a> to switch to the N.E.M.O project. You’ll need to do a quick rebuild for content to work.</p> 
       <p>The Lumberyard team is working hard to get new features too you and there’s much more on the way. As always, we want your help in driving our roadmap and providing suggestions for future features, improvements, and learning materials.&nbsp; Send us feedback at Lumberyard-feedback@amazon.com.</p> 
       <p>&nbsp;</p> 
       <p>This blog was co-authored by Lumberyard Creative Director Christo Vuchetich, and Software Development Engineer Luis Sempe.</p> 
       
