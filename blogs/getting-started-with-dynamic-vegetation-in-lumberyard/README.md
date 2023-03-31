# Getting Started with Dynamic Vegetation in Lumberyard: Sample Now Available

<p><img class="aligncenter size-large wp-image-1925" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/DynVeg_BlogHeader_Image-1024x341.jpg" alt="" width="1024" height="341"></p> 
       <p>Amazon Lumberyard 1.19 introduced over 150 new features, bug fixes, and improvements so you can create beautiful, dense worlds, faster. Included with this release is Dynamic Vegetation, our new workflow to help you procedurally generate a diverse and detailed biome faster and more efficiently than manually placing and painting vegetation.</p> 
       <p>The Dynamic Vegetation workflow enables you to define and distribute customized plant vegetation across large areas of game levels with greater consistency and editing ability. Built upon our component entity system, this tool uses a diverse library of component types to help custom design and propagate the type of vegetation biomes required for modern game development.</p> 
       <p>To help you quickly get to grips with this tool, we’ve created a quick start guide.
       <h2>About Dynamic Vegetation</h2> 
       <p>Lumberyard customers told us that it’s difficult and time-consuming to create high-fidelity content in large-scale game environments. As game world sizes have increased exponentially, manual workflows like painting or placing individual plant instances doesn’t scale well with production needs. When we approached the challenge of building a new vegetation system, we worked directly with our customers to identify essential requirements including:</p> 
       <ul> 
        <li>Solving for scaled environment sizes</li> 
        <li>Consistent visual fidelity and quality</li> 
        <li>Blending proceduralism with ability to art direct the outcome</li> 
        <li>WYSIWYG live editing and rapid iteration</li> 
        <li>Environment content that can be altered dynamically</li> 
       </ul> 
       <p>We built the new workflow for vegetation upon the <a href="https://github.com/awsdocs/amazon-lumberyard-user-guide/blob/master/doc_source/component-intro.md" target="_blank" rel="noopener">Lumberyard component entity system</a>. This adds flexibility and unlocks easier world building options. The Dynamic Vegetation workflow allows you to author both smaller areas of vegetation like a garden, to large biomes distributed across vast areas of a game level. The diverse vegetation component library provides you with the mechanisms needed to author visually complex vegetation biomes for modern high-fidelity games.</p> 
       <p>Once biomes are established, you can define vegetation coverage of the entire level, or specified regions on the terrain, mesh entities, roads, rivers, water volumes, and even arbitrary shapes. Once placement areas are defined, simple modifications to the components and parameters for each biome entity allows for real-time updates and edits. The workflow supports multiple biome layers with rule sets that can mask, blend, or exclude other dynamic vegetation biome entities in the level.</p> 
       <p>The resulting visual consistency and easy modification allows for a more reliable course of production from concept to final product. Because every biome is defined using the entity component and slice system, settings are saved exactly as designed and can be used in other levels, or distributed as desired across different areas of a large world. Edits and changes to any vegetation slice are then updated across all instances and maintain consistent visual direction.</p> 
       <h2>Getting Started</h2> 
       <p>The Dynamic Vegetation workflows allow users to create artistically expressive vegetation areas through our library of vegetation components.</p> 
       <p>There are two primary types of areas:</p> 
       <p><strong>Background layer</strong> <em>Coverage</em> intended to cover large&nbsp;swaths or the entire level with default ground cover.</p> 
       <p><strong>Foreground layer</strong>&nbsp;<em>Clusters</em> intended for smaller local areas, which replace the ground cover they are placed over.</p> 
       <p>This quick start guide will cover the basics of dynamic vegetation. To familiarize you&nbsp;with the basic components and workflow, we will walk you through creating a simple version of each primary layer. Once you’re through the basics, we encourage you to try out some of the other components.</p> 
       <p>Note: This quick start guide assumes at least some basic familiarity with the Lumberyard Editor. If a term or a tool is unfamiliar you may want to go through the tutorials and samples in the Lumberyard general <a href="https://github.com/awsdocs/amazon-lumberyard-getting-started-guide/blob/master/doc_source/intro.md" target="_blank" rel="noopener">‘Getting Started’</a> and <a href="https://github.com/awsdocs/amazon-lumberyard-user-guide/blob/master/doc_source/lumberyard-intro.md" target="_blank" rel="noopener">‘User Guide’.</a></p> 
       <h2>Setup</h2> 
       <p>We have provided examples of the tasks outlined in this document, you may use them as reference and to follow along or compare your results.&nbsp; Those examples are provided in the DyanmicVegetationSample project. Before we get started make sure that the project is activated:</p> 
       <ol> 
        <li>Unzip the ‘DynamicVegetationSample.zip’ file</li> 
        <li>Move the project folder ‘DynamicVegetationSample’ into your Lumberyard 1.19 location:[drive]:\Amazon\Lumberyard1.19\dev\DynamicVegetationSample</li> 
        <li>Start the ‘Lumberyard Project Configurator’</li> 
        <li>Click to select the ‘DynamicVegetationSample’ project</li> 
        <li>Then in the upper-right, click on the button labeled ‘Set as Default’</li> 
        <li>Close the ‘Project Configurator’</li> 
        <li>The project will need to be built before you can run it.</li> 
       </ol> 
       <h2>New Level</h2> 
       <h3>Steps:</h3> 
       <ol> 
        <li>Start the Lumberyard Edtor.exe</li> 
        <li>On the launch screen, use the button labeled ‘New Level…’ to create a new empty level.</li> 
        <li>We named ours: 
         <ol type="a"> 
          <li>ExampleBiome</li> 
         </ol> </li> 
        <li>We used ‘Heightmap Resolution’: 
         <ol type="a"> 
          <li>128 x 128</li> 
         </ol> </li> 
        <li>For ‘Texture Dimensions’: 
         <ol type="a"> 
          <li>512 x 512</li> 
         </ol> </li> 
       </ol> 
       <h3>Result:</h3> 
       <h3><img loading="lazy" class="alignnone wp-image-1824 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img01.png" alt="" width="390" height="225"></h3> 
       <h3>Level Inspector ‘Vegetation System Settings’ Component</h3> 
       <p>Let’s do one more setup step and use the ‘Level Inspector’ to add a ‘Vegetation System Settings’ component to the level, this component will allow us to configure the system:</p> 
       <ol> 
        <li>Open the ‘Level Inspector’ with this menu 
         <ol type="a"> 
          <li>Editor &gt; Tools &gt; Level Inspector</li> 
          <li>Note: we recommend docking this behind the Entity Inspector</li> 
         </ol> </li> 
        <li>Use the ‘Add Component’ button and add the ‘Vegetation System Settings’ component</li> 
        <li>Turn on the setting ‘Override Instance Time Slicing’, this improves the live editing performance of vegetation while in the editor.</li> 
       </ol> 
       <p>Note: you’ll also see we modified a few more numbers that allow you to configure the draw distance (view area grid size), density of placement (Sector Point Density), and control over vegetation LOD switching (LOD Distance Ratio, View Distance Ratio).<br> <img loading="lazy" class="alignnone wp-image-1827 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img02.png" alt="" width="514" height="516"></p> 
       <p>&nbsp;</p> 
       <h2>Creating Basic Coverage (Grass micro-biome)</h2> 
       <p>Let’s create a grass biome patch.</p> 
       <h3>Steps</h3> 
       <ol> 
        <li>Create an Entity and name it BasicCoverage 
         <ul> 
          <li>Make sure it’s positioned to the center of the terrain</li> 
          <li>That would be setting the Entity’s Translate to: <pre>x 64.0 m, y 64.0 m, z 32.0 m</pre> </li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1832 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img03.png" alt="" width="455" height="122"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1833 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img04.png" alt="" width="517" height="468"></p></li> 
        <li>In the ‘Entity Inspector’ panel use the ‘Add Component’ palette to add a ‘Vegetation Layer Spawner’ Component 
         <ul> 
          <li>This is the core component that initializes the ‘planting engine’ to spawn vegetation.</li> 
          <li>That component have additional component dependencies that are required to make it functional – the system will guide you through these needs.</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1835 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img05.png" alt="" width="506" height="238"></p></li> 
        <li>The first thing it requires is a shape which will define the area where to plant 
         <ul> 
          <li>In the ‘Entity Inspector’ panel, on the ‘Vegetation Layer Spawner’, use the button labeled ‘Add Required Component’, then select to add the ‘Vegetation Reference Shape’.</li> 
          <li>This shape type actually just references another entity that has a shape (allowing you to re-use shapes, which is common to this workflow, and we will get to that later in this exercise)</li> 
          <li>So in the next section we will want to add another entity that has an actual Shape-type Component, which shape will inform what area to plant in.</li> 
          <li>(but continue here briefly for now so we consolidate steps)</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1836 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img06.png" alt="" width="506" height="305"></p></li> 
        <li>Next the entity will require a ‘Vegetation Asset List’ which will define a set of what to plant 
         <ul> 
          <li>In the ‘Entity Inspector’ panel, on the ‘Vegetation Spawner’, use the on the button labeled ‘Add Required Component’, three options will come up – for this example select ‘Vegetation Asset List’</li> 
          <li>This component defines the list of vegetation models to place within this area, along with some metadata about each type (this metadata can later be used by other components to define placement behavior procedurally)</li> 
          <li>Right now let’s just add a single item to this list</li> 
          <li>In the ‘Vegetation Asset List’ there is a currently empty list called ‘Embedded Assets’, it is already populated with a single empty entry which is expanded (we just need to use this to specify an asset to plant.)</li> 
          <li>There is a field labeled ‘Mesh Asset’ (which is currently empty) to add a mesh click on the button labeled “…” which will open a dialog labeled ‘Pick Static Mesh’</li> 
          <li>You can use this dialog to browse the list of .cgf assets that can be placed</li> 
          <li>Use the Search Field at the top of the dialog and type in ‘grass’ and select one of the models, we used ‘am_grass_03_seeds_group.cgf’</li> 
          <li>You will see this entire level fill with that grass type (not very interesting yet, but it is a start!)</li> 
          <li>The entire level fills, because our ‘Reference Shape’ is empty (we will fix that next.)</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1837 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img07.png" alt="" width="506" height="447"></p></li> 
        <li>The last thing we want to do in this set this ‘Vegetation Layer Spawner’ Component to ‘Layer Priority: background’<br> Result:<br> <img loading="lazy" class="alignnone wp-image-1839 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img08.png" alt="" width="580" height="334"></li> 
       </ol> 
       <h2>Let’s Make it a Patch</h2> 
       <h3>Steps</h3> 
       <ol> 
        <li>Create a Child Entity with a Box Shape 
         <ul> 
          <li>Select the parent entity ‘BasicCoverage’</li> 
          <li>Right-click and select ‘Create Child Entity’ from the context menu</li> 
          <li>Rename the child entity ‘32m_Box(Patch)’</li> 
          <li>In the Entity Inspector, use the ‘Add Component’ button to create a ‘Box Shape’</li> 
          <li>Use the Box Shape: Dimensions to change the size of the box: <pre>x: 32.0 m, y: 32.0 m, z: 16.0 m</pre> </li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1843 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img09.png" alt="" width="454" height="146"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1844 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img10.png" alt="" width="513" height="642"></p></li> 
        <li>Set the Box as the Placement Area 
         <ul> 
          <li>Select the parent entity ‘BasicCoverage’ in the Entity Outliner</li> 
          <li>In the Entity Inspector use the <a href="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png"><img loading="lazy" class="alignnone size-full wp-image-1846" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png" alt="" width="20" height="20"></a> on the ‘Vegetation Reference Shape’ and pin a reference to the child entity ‘32m_Box(Patch)’</li> 
          <li>You will now notice that the vegetation planting is confined to the referenced Box Shape from the child entity ‘32m_Box(Patch)’</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1847 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img12.png" alt="" width="454" height="138"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1848 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img13.png" alt="" width="506" height="59"><br> Result:<br> <img loading="lazy" class="alignnone wp-image-1849 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img14.png" alt="" width="580" height="334"></p></li> 
       </ol> 
       <h2>Let’s Get Procedural</h2> 
       <p>We can begin to make this area more visually interesting and craft a biome by adding some procedural noise and rules to it.</p> 
       <h3>Steps</h3> 
       <ol> 
        <li>Extend the list of assets to be planted 
         <ul> 
          <li>With the parent entity ‘BasicCoverage’ still selected</li> 
          <li>In the Entity Inspector use the ‘Vegetation Asset List’ to add some more vegetation assets to the list of things to plant in this area.</li> 
          <li>On the upper-right edge of the line ‘Embedded Assets’ there is a <img loading="lazy" class="alignnone wp-image-1851 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img16.png" alt="" width="20" height="20"> button. Click that button twice, to add two new 2 new entries.</li> 
          <li>Add the following ‘Mesh Assets’ to those entries: <pre>Am_grass_tall_02_group</pre> <pre>Am_grass_tall_01_group</pre> </li> 
          <li>Notice that what is planted doesn’t change; we need a way to describe how to select what to plant from the list</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1852 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img15.png" alt="" width="506" height="491"></p></li> 
        <li>Weighted Selection 
         <ul> 
          <li>With the parent ‘BasicCoverage’ still selected, with the Entity Inspector use the ‘Add Component’ button to add the ‘Vegetation Asset Weight Selector’.</li> 
          <li>This component will allow us to reference a ‘gradient signal’ like procedural noise, and we can use that signal to drive selection.</li> 
          <li>To do that, we will need to build a procedural ‘gradient signal’ generator we can reference in this component.</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1854 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img17.png" alt="" width="506" height="153"></p></li> 
        <li>Create the FastNoise (procedural noise) 
         <ul> 
          <li>With the parent entity ‘BasicCoverage’ selected, in the Entity Outliner right-click and select ‘create child entity’</li> 
          <li>Rename this entity ‘FastNoise’</li> 
          <li>With this new child entity selected, in the Entity Inspector use the ‘Add Component’ button to add a ‘FastNoise Gradient’ component.</li> 
          <li>This component requires a few other components to function, let’s add those now using the ‘Add Required Component’ add the ‘Gradient Transform Modifier’ component.</li> 
          <li>We also require a shape, so use the ‘Add Required Component’ again and select the ‘Vegetation Reference Shape’ component. Now the components will operate properly. Expand the ‘Preview’ to see the noise signal being generated.</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1855 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img18.png" alt="" width="454" height="156"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1856 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img19.png" alt="" width="393" height="574"></p></li> 
        <li>Tune the Noise Visuals 
         <ul> 
          <li style="list-style-type: none"> 
           <ul> 
            <li>Before we hook this up to the ‘Weight Selector’ let’s first tune the noise to be a little more interesting.</li> 
            <li>Make the following changes to the parameters of the FastNoise component: 
             <ul> 
              <li>Set ‘Noise Type’ to ‘Simplex Fractal’</li> 
              <li>Expand the ‘FastNoise Advanced Settings’</li> 
              <li>Set the ‘Fractal Type’ to ‘Billow’</li> 
              <li>Set the ‘Frequency’ to ‘0.1’</li> 
             </ul> </li> 
           </ul> </li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1857 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img20.png" alt="" width="395" height="427"></p></li> 
        <li>Connect the noise to ‘Weight Selector’ 
         <ul> 
          <li>In the Entity Outliner select the parent entity ‘BasicCoverage’</li> 
          <li>In the Entity Inspector use the <a href="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png"><img loading="lazy" class="alignnone size-full wp-image-1846" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png" alt="" width="20" height="20"></a> button on the ‘Vegetation Asset Weight Selector’ and pin a reference to the child entity ‘FastNoise’</li> 
          <li>You should see the planting pattern change, as the values of the FastNoise are driving the selection criteria.</li> 
          <li>If you expand the ‘Advanced’ settings on the ‘Weight Selector’, and ‘Enable Levels’, you can tweak the inbound noise signal which will further tune the selection.</li> 
          <li>Set the ‘Input Mid’ value to ‘1.3’</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1860 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img21.png" alt="" width="453" height="164"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1859 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img22.png" alt="" width="506" height="388"></p></li> 
        <li>Pattern based Selection and Placement.<br> The area should now look something like this image:<br> <img loading="lazy" class="alignnone wp-image-1861 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img23.png" alt="" width="772" height="445"></li> 
       </ol> 
       <h2>Procedural Modifiers</h2> 
       <p>We can continue to add additional rules to this ‘Layer Spawner’ to refine its look-and-feel. These bolt-on’s are Filter and Modifier type components. Before we add those, we’re going to make one more child entity with a different noise pattern we will use to hook them up.</p> 
       <h3>Steps</h3> 
       <ol> 
        <li>Repeat the steps for ‘FastNoise’ to create ‘RandomNoise’ 
         <ul> 
          <li>Right-click on ‘BasicCoverage’ and ‘create child entity’</li> 
          <li>Rename this child ‘RandomNoise’</li> 
          <li>Add the ‘FastNoise’ and it’s required components, just like the previous noise use the ‘Reference Shape’ and use it to pin a reference to the ‘32m_Box(Patch)’</li> 
          <li>Set the ‘Noise Type’ to ‘White Noise’ (this is a type of random noise)</li> 
          <li>That’s it…</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1864 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img24.png" alt="" width="454" height="180"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1863 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img25.png" alt="" width="395" height="497"></p></li> 
        <li>Add the Modifier Components 
         <ul> 
          <li>With the parent entity ‘BasicCoverage’ selected, add the following components: 
           <ul> 
            <li>– ‘Vegetation Scale Modifier’</li> 
            <li>– ‘Vegetation Rotation Modifier’</li> 
            <li>– ‘Vegetation Position Modifier’</li> 
           </ul> </li> 
          <li>Let’s go through setting up each of these individually</li> 
         </ul> </li> 
        <li>Scale Modifier 
         <ul> 
          <li>Simply use the <a href="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png"><img loading="lazy" class="alignnone size-full wp-image-1846" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png" alt="" width="20" height="20"></a> button on the ‘Scale Modifier’ to pin a reference for the ‘Gradient Entity Id’ to ‘RandomNoise’.</li> 
          <li>Set the ‘Range Min’ to ‘0.75’</li> 
          <li>Set the ‘Range Max’ to ‘1.25’</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1867 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img26.png" alt="" width="455" height="179"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1868 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img27.png" alt="" width="492" height="188"></p></li> 
        <li>Rotation Modifier 
         <ul> 
          <li>Use the <img loading="lazy" class="alignnone wp-image-1846 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png" alt="" width="20" height="20"> button on the ‘Rotation Modifier’ to pin a reference for the ‘Rotation Z’ ‘Gradient Entity Id’ to ‘RandomNoise’.</li> 
          <li>This component already has good default settings, so no configuration is necessary.</li> 
          <li>(Don’t change Rotation X, or Y)</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1869 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img28.png" alt="" width="455" height="179"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1870 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img29.png" alt="" width="492" height="242"></p></li> 
        <li>Position Modifier 
         <ul> 
          <li>This one requires a little bit more configuration to unlock its full potential.</li> 
          <li>Use the <img loading="lazy" class="alignnone wp-image-1846 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png" alt="" width="20" height="20"> button on the ‘Position Modifier’ ‘Postion X’:’Gradient’:’Gradient Entity Id’ to pin a reference for ‘RandomNoise’.</li> 
          <li>Use the <img loading="lazy" class="alignnone wp-image-1846 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png" alt="" width="20" height="20"> button on the ‘Position Modifier’ ‘Postion Y’:’Gradient’:’Gradient Entity Id’ to pin a reference for ‘RandomNoise’.</li> 
          <li>Under the ‘Position Y’ expand the ‘Advanced’ settings group and ‘Enable Transform’</li> 
          <li>Then put in the following settings:<br> <img loading="lazy" class="alignnone wp-image-1866 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img32.png" alt="" width="413" height="91"></li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1871 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img30.png" alt="" width="455" height="179"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1872 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img31.png" alt="" width="506" height="654"></p></li> 
       </ol> 
       <h2>Let’s Do a Visual Comparison</h2> 
       <p>Below is a visual comparison of the same ‘Layer Spawner’ before and after the modifiers where applied and set up.</p> 
       <p>Before:</p> 
       <p><img loading="lazy" class="alignnone wp-image-1875 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img33.png" alt="" width="658" height="380"></p> 
       <p>After:</p> 
       <p><img loading="lazy" class="alignnone wp-image-1874 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img34.png" alt="" width="658" height="380"></p> 
       <h2>Save as Slice</h2> 
       <p>OK we are nearly done, there are just two things left – 1) save our patch as a slice, and 2) extend its placement bounds to fill the world.</p> 
       <ul> 
        <li style="list-style-type: none"> 
         <ul> 
          <li>Right-click on ‘BasicCoverage’ and select ‘Create Slice…’</li> 
          <li>We saved our example to the following path:</li> 
         </ul> </li> 
       </ul> 
       <pre>dev\DynamicVegetationSample\Levels\DynamicVegetationSample\Slices\BasicCoverage.slice</pre> 
       <h2>Fill Er’ Up!</h2> 
       <p>Now let’s create a ‘World Box’ and fill up the world.</p> 
       <h3>Steps</h3> 
       <ol> 
        <li>Create the WorldBox 
         <ul> 
          <li>In the Entity Outliner create a new entity and rename it ‘World Box’</li> 
          <li>Position its Transform at: <pre>x: 64.0 m, y: 64.0 m, z: 32.0 m</pre> </li> 
          <li>Add Component: <pre>Box Shape</pre> </li> 
          <li>Set it’s dimensions to: <pre>x:128, y: 128, z:64</pre> </li> 
          <li>That should look like this<br> <img loading="lazy" class="alignnone wp-image-1879 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img37.png" alt="" width="656" height="378"></li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1877 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img35.png" alt="" width="455" height="211"></p> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1878 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img36.png" alt="" width="506" height="169"></p></li> 
        <li>Override Placement area with WorldBox 
         <ul> 
          <li>Select the ‘BasicCoverage’ slice</li> 
          <li>In the Entity Inspector use the <a href="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png"><img loading="lazy" class="alignnone size-full wp-image-1846" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img11.png" alt="" width="20" height="20"></a> button of the ‘Vegetation Reference Shape’ to create a reference override, by pinning the reference to ‘WorldBox’</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1880 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img38.png" alt="" width="454" height="204"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1881 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img39.png" alt="" width="492" height="54"></p></li> 
       </ol> 
       <p>You should see the placement of vegetation extend across the entire world – and that is a wrap. We hope you see the potential and can think of the many ways this workflow could be leveraged to quickly fill up your world with interesting vegetation content.</p> 
       <p>Debug Boxes Outlines:<br> <img loading="lazy" class="alignnone wp-image-1882 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img40.png" alt="" width="656" height="378"></p> 
       <p>Box Outlines Hidden:<br> <img loading="lazy" class="alignnone wp-image-1883 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img41.png" alt="" width="656" height="378"></p> 
       <h2>Creating a Basic Foreground Cluster</h2> 
       <p>Let’s create a dead tree to place in the foreground layer, with a blocker and local placement area around its base that will override the grass coverage in the background layer.<br> <img loading="lazy" class="alignnone wp-image-1886 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img42.png" alt="" width="965" height="557"></p> 
       <h3>Steps</h3> 
       <ol> 
        <li>Create an Entity and name it DeadTreeWithBasicCluster 
         <ul> 
          <li>Make sure it’s positioned to the center of the terrain. That would be setting the Entity’s Translate to: <pre>x 64.0 m, y 64.0 m, z 32.0 m</pre> </li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1890 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img43.png" alt="" width="364" height="88"></p></li> 
        <li>Add a Mesh Component 
         <ul> 
          <li>In the ‘Mesh Asset’ field, use the <img loading="lazy" class="alignnone wp-image-1891 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img45.png" alt="" width="20" height="20"> button and load the asset named ‘am_dead_tree’</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1889 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img44.png" alt="" width="421" height="135"></p></li> 
        <li>Create a child entity named ‘BasicCluster’<br> Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1892 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img46.png" alt="" width="364" height="110"></li> 
        <li>Add Vegetation Asset List component 
         <ul> 
          <li>Add the following assets to the element list: 
           <ul> 
            <li>Am_grass_flower_red_group</li> 
            <li>Am_grass_03_seeds_group</li> 
            <li>Am_rocks_small_01</li> 
            <li>Am_grass_tall_01_group</li> 
           </ul> </li> 
          <li>You’ll see the box fill up, so like the biome, we need to setup a mechanism to drive selection for asset placement.</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1893 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img47.png" alt="" width="407" height="144"></p></li> 
        <li>Add Vegetation Layer Spawner component 
         <ul> 
          <li>Set the ‘Layer Priority’ to: ‘Foreground’, this setting is what allows this area override the grass in the background layer.</li> 
         </ul> </li> 
        <li>Add a Box Shape Component 
         <ul> 
          <li>Set the Box Shape: Dimensions to: <pre>x: 13.0 m, y: 13.0 m, z: 13.0 m</pre> </li> 
          <li>This contains the placement area</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1894 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img48.png" alt="" width="407" height="324"></p></li> 
        <li>Create a child entity of BasicCluster named RandomNoise 
         <ul> 
          <li>We will setup a random noise gradient signal on this entity, and use that to feed a weighted selection</li> 
         </ul> </li> 
        <li>Add the Random Noise Gradient</li> 
        <li>Add the Gradient Transform Component</li> 
        <li>Add a Vegetation Reference Shape 
         <ul> 
          <li>Pin the ‘Shape Entity id’ to reference the parent ‘BasicCluster’.</li> 
          <li>Now reselect ‘BasicCluster’ so we can extend it.</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1895 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img49.png" alt="" width="359" height="132"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1896 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img50.png" alt="" width="329" height="484"></p></li> 
        <li>Add a Vegetation Asset Weight Selector component 
         <ul> 
          <li>Pin the ‘Gradient Entity Id’ to reference the ‘RandomNoise’ child entity.</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1897 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img51.png" alt="" width="364" height="131"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1898 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img52.png" alt="" width="407" height="148"></p></li> 
        <li>Add a Vegetation Rotation Modifier 
         <ul> 
          <li>Pin the ‘Gradient Entity Id’ to also reference the ‘RandomNoise’ child entity.</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1901 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img53.png" alt="" width="364" height="131"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1900 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img54.png" alt="" width="407" height="242"></p></li> 
        <li>Add a Vegetation Scale Modifier 
         <ul> 
          <li>Pin the ‘Gradient Entity Id’ to also reference the ‘RandomNoise’ child entity. 
           <ul> 
            <li>Set ‘Range Min’ to 0.6</li> 
            <li>Set ‘Range Max’ to 0.85</li> 
           </ul> </li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1902 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img55.png" alt="" width="364" height="131"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1903 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img56.png" alt="" width="407" height="188"></p></li> 
        <li>Add a Vegetation Position Modifier 
         <ul> 
          <li>Pin the ‘Gradient Entity Id’ for both ‘Position X’ to also reference the ‘RandomNoise’ child entity.</li> 
          <li>Do the same for ‘Position Y’ gradient</li> 
          <li>Don’t forget to expand ‘Advanced’ and ‘Enable Transform’ and edit the inbound signal to get better random results without requiring an additional noise entity. Set scale: <pre>x:-1.0, y: -1.0, z: 1.0</pre> <p>and</p> <pre>rotate: z:45.0</pre> </li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1904 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img57.png" alt="" width="364" height="131"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1905 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img58.png" alt="" width="407" height="649"></p></li> 
        <li>Add a Vegetation Slope Alignment Modifier 
         <ul> 
          <li>You can leave this with its default values (everything gets full slope alignment.)</li> 
          <li>Go ahead and hide the ‘BasicCoverage’ grass biome</li> 
          <li>You should see a boxy placement like this now<br> <img loading="lazy" class="alignnone wp-image-1908 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img61.png" alt="" width="662" height="475"></li> 
          <li>And we would like to confine the placement to something more organic like a ring shape</li> 
          <li>(next step)</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1906 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img59.png" alt="" width="364" height="131"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1907 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img60.png" alt="" width="407" height="188"></p></li> 
        <li>Create a child entity named ImageGradient 
         <ul> 
          <li>We will setup an image asset based gradient signal on this entity, and use that to feed distribution filter</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1910 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img62.png" alt="" width="364" height="154"></p></li> 
        <li>Add the Image Gradient component 
         <ul> 
          <li>You’re missing a few components to make this one work</li> 
         </ul> </li> 
        <li>Add the Gradient Transform Component</li> 
        <li>Add a Vegetation Reference Shape 
         <ul> 
          <li>Pin the ‘Shape Entity Id’ to reference the parent ‘BasicCluster’.</li> 
         </ul> </li> 
        <li>Select the Image Gradient component again 
         <ul> 
          <li>Under image assets load the asset: <pre>‘blured_circle_gsi’</pre> <p>found under the path:</p> <pre>Gems/Vegetation/ImageGradients/LargeWorldExamples</pre> </li> 
          <li>ProTip: add the suffix ‘_gsi’ to any image to inform the Asset Processor to cook it into data the Dyn Veg system can use. This suffix stands for ‘Gradient Signal Image’.</li> 
          <li>Now reselect ‘BasicCluster’ so we can extend it again</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1911 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img63.png" alt="" width="421" height="633"></p></li> 
        <li>Add a Vegetation Distribution Filter 
         <ul> 
          <li>This component uses a threshold on the inbound signal to filter out placement.</li> 
          <li>Pin the ‘Gradient Entity Id’ to reference the ‘ImageGradient’ child entity.</li> 
          <li>Instead of boxy placement we can see that the placement has been confined to circular area around the stump.<br> <img loading="lazy" class="alignnone wp-image-1914 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img66.png" alt="" width="662" height="475"></li> 
          <li>Go ahead and unhide the ‘BasicCoverage’ grass biome</li> 
          <li>We can clearly see that this local area around the stump is overriding and placing new vegetation.<br> <img loading="lazy" class="alignnone wp-image-1915 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img67.png" alt="" width="662" height="475"></li> 
          <li>(We also turned off the debug rendering of the Boxes and other Shapes.)</li> 
          <li>There is one more step we’d like to cover…</li> 
          <li>One additional tweak we can make is to create a ‘Blocker’ area at the base, this will keep both the foreground and background layers from placing vegetation.</li> 
         </ul> <p>Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1912 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img64.png" alt="" width="364" height="153"><br> Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1913 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img65.png" alt="" width="407" height="206"></p></li> 
        <li>Create a new child entity named Blocker<br> Entity Outliner:<br> <img loading="lazy" class="alignnone wp-image-1916 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img68.png" alt="" width="364" height="175"></li> 
        <li>Add the Cylinder Shape component 
         <ul> 
          <li>Set the Height to 4.0 m and Radius to 1.6 m</li> 
          <li>Set the Shape Color</li> 
         </ul> <p>Entity Inspector:<br> <img loading="lazy" class="alignnone wp-image-1917 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img69.png" alt="" width="421" height="264"></p></li> 
        <li>Add the Vegetation Layer Blocker 
         <ul> 
          <li>Set the ‘Layer Priority’ to : ‘Foreground’. We can see that this has created a small clear patch at the base of the stump.<br> <img loading="lazy" class="alignnone wp-image-1918 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img70.png" alt="" width="662" height="475"></li> 
          <li>(If you toggle the visibility of the shape entity in the Entity Outliner, when hidden you’ll see the blocker doesn’t function and the patch is filled back in)</li> 
          <li>And if we turn off the debug drawing of Shapes in the viewport, we can see the final results.<br> <img loading="lazy" class="alignnone wp-image-1919 size-full" src="/blogs/getting-started-with-dynamic-vegetation-in-lumberyard/images/img71.png" alt="" width="662" height="475"></li> 
          <li>This ‘DeadTreeWithBasicCluster’ entity, can now be saved as a slice allowing you to instantiate many copies to be scattered across and environment.</li> 
          <li>This is how we build a library of interesting modular environment pieces.</li> 
         </ul> </li> 
       </ol> 
       <p>&nbsp;</p> 
       <p>That’s it, you’ve completed the first steps to learn how to use our new Dynamic Vegetation system. In time, we’ll add more advanced lessons and dive into the huge range of possibilities that are available.</p> 
       <p>As always, we want to hear from you. Send your comments, questions, or even feature requests via <a href="https://gamedev.amazon.com/forums/index.html">our forums</a>.</p> 
       <p>&nbsp;</p> 
       <p>This blog was co-authored by Lumberyard Creative Director Christo Vuchetich, and Product Manager, Jonny Galloway.</p> 
       
