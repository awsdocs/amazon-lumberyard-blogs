#  The Future of the Lumberyard Build System

<p><em>Authored by Esteban Papp, Senior Software Development Engineer on the Amazon Lumberyard team.</em></p> 
       <p>There’s a Spanish proverb I think about a lot while developing for Amazon Lumberyard: “<em>En casa de herrero, cuchillo de palo.</em>” In English, it translates to: “<em>In the blacksmith’s house, a wooden knife.</em>”</p> 
       <p>Why this particular proverb?</p> 
       <p>It refers to the fact that sometimes, despite someone may specialize in a certain trade, they don’t apply their skills and experience in that trade to common, everyday tasks. They may fold and hammer steel all day to make great tools for their customers, but when they come home, the tools they use may be crude, inefficient, and disposable — they’re just enough to get the job done. After all, a wooden knife can still cut bread!</p> 
       <p>Recently, one big area we have been working on within Lumberyard is improving developers’ productivity. We have been listening, understanding, and working to reduce the usage of “wooden knives” within Lumberyard — after all, we use these “wooden knives”, too. This not only applies to Lumberyard’s developers, but also to all those customers that use and modify Lumberyard’s source code for their games. In this blog post, we will focus on just one (very large!) wooden knife in our kitchen: the Lumberyard build system.</p> 
       <p>First, let me share a little history of our dubious woodworking. When we branched CryEngine to start Lumberyard, we kept the build system CryEngine used: WAF. Over time we did some improvements but it carried a lot of issues with it over the years. We kept building on top of WAF, and we never got enough data to justify changing it with something else. The wooden knife was still cutting bread, even if things were getting messier. And the reality is, simply changing the build system to another one wouldn’t make things magically faster or easier.</p> 
       <p>After years of developing with WAF, we ended with a build system that was hard to understand, maintain, improve and overall, difficult to use. We received a lot of evidence of this pain point from forums, internal customer feedback, and our own developers. Finally, it became clear that we needed a better knife.</p> 
       <p>The next step was to decide on what sort of knife we were going to forge and how we were going to deliver it to our customers. To embark on this task, we started to deeply analyze what our current problems were:</p> 
       <ul> 
        <li>Lumberyard is a big codebase, with approximately 4 million lines of C++ code.</li> 
        <li>Lumberyard builds target 8 distinct platforms.</li> 
        <li>Lumberyard builds have multiple configurations, from debug builds to performance-oriented ones. We also have variations: test and “dedicated” (server). These variations were also intermixed. In total we have 11 different build configurations. Not all configurations are available on each platform.</li> 
        <li>We have multiple types of builds, including <em>monolithic</em> builds and <em>uber</em> builds.</li> 
        <li>We have different build specifications (“specs”) for different target groups. Some of those groups are outdated.</li> 
        <li>Iterative workflows are not cheap. Doing things like compiling one file comes with an unexpected tax.</li> 
        <li>The build system and our extensions are not easy to understand, debug, modify and/or fix.</li> 
        <li>Lastly, we have no great IDE integration story. Major productivity features in different IDEs like Visual Studio, XCode, and Android Studio do not work or work in unexpected/slow ways.</li> 
       </ul> 
       <p>All in all, across a very large code base, multiple build specifications with a complex matrix of behaviors, slow workflows and poor environment integration, we were really feeling the effects of entropy. Internal and external developers started to feel the pain of working with a wooden knife.</p> 
       <p>We set out some goals:</p> 
       <ul> 
        <li>Improve developer productivity. Make frequent and iterative workflows fast.</li> 
        <li>Reduce maintenance overhead.</li> 
        <li>Reduce developer onboarding time spent understanding the build system, and increase the amount of developers that can contribute to the build system.</li> 
        <li>Reduce the amount of build-related issues that surface around releases.</li> 
       </ul> 
       <p>We defined the following tenets when considering the new build model:</p> 
       <ul> 
        <li><strong>Familiarity</strong>: leverage what most developers may already be familiar with. Reduce the ramp-up time to address issues.</li> 
        <li><strong>Workflow-first</strong>: focus on and improve developer’s frequent tasks. Favor small fast iterations.</li> 
        <li><strong>Community support</strong>: leverage on build-system community support to help with some issues. Then, Lumberyard support can focus more on the engine. If we can defer some of the support to other communities, we can get answers quicker to our customers and keep our developers more focused.</li> 
        <li><strong>Maintainability</strong>: reduce the maintenance burden we have on the build system.<br> There are lots of pieces that are constantly changed in the current build system, among others, each different version of toolset, compiler and IDE sometimes requires adaptation.</li> 
        <li><strong>Integration with IDEs and tools</strong>: we want our customers to use the IDEs of their choosing and leverage productivity tools that are already available.</li> 
        <li><strong>Simplicity</strong>: As much as we can. Lumberyard is a big codebase that supports multiple platforms and different ways of compiling. Features inheritably create complexity, however, we can centralize those complexities instead of spread it across every file.</li> 
       </ul> 
       <p>We considered this a “start from scratch” situation. Instead of trying to adapt what we have to what we want, we thought about what we wanted and then “worked backwards” in the Amazon style. Without the constraints of having to keep anything around, we decided the best approach would be to replace the build system altogether.</p> 
       <p>However, we also realized that a lot of issues we had were not specific nor resolved by a build system. Most issues were a consequence of _how_ we were using the build system. With a blank slate, we could design tools and abstractions that solved those issues while keeping the complexity low.</p> 
       <h1>Towards a sharp solution</h1> 
       <p>Ideally, if we were starting from scratch and selecting a universal build system to meet our overall needs, we would not pick any at all. Instead, we would use the canonical build system recommended for each platform. For example, we’d use MSBuild for Windows, XCode for Mac, and Makefile/Ninja for Linux. Why? Well, we have multiple platforms to support and we need a build system that works well across all of them. Using the SDK’s tools reduces the amount of things to maintain and “glue” to fit things together.</p> 
       <p>This lead us to an obvious choice: <strong>CMake</strong>. Despite being other alternatives out there, CMake has a large community, is widely used and is familiar to most developers. As a result, <strong>the next major version of Lumberyard will use CMake</strong> to generate build system files that each platform will use to build with the recommended/expected build system (a.k.a. the CMake way).</p> 
       <p>CMake + a native platform toolchain + platform-specific SDK tools = the best developer experience across platforms, as the math goes. Windows developers can use Visual Studio or any other Windows-specific IDE out there, and all of them work with MSBuild since that is the “expected” build system to use in Windows. Similarly, Mac developers can use XCode since that is the preferred IDE. Similarly, any tool that boasts about working in Mac has to understand XCode files. For Android developers, Android Studio will have better integration since its Gradle build system has support for native code CMake scripts.</p> 
       <p>I imagine that many of you are not surprised that we chose to base our new build system on the bedrock of CMake. That is exactly our hope and our goal: that, as customers, you feel this decision is “expected” and that you are willing to embrace “expected” developer workflows. We know that not everyone will agree and some will be impacted. We also acknowledge there will be a ramp up time for those that are used to WAF.</p> 
       <p>For those surprised, know that we did our homework and have made what we feel is a worthy conversion, focused on the Lumberyard of today, and not the Cry-based Lumberyard of 5 years ago. In fact, we have not just replaced our build system; we have re-designed how developers work and removed as many rocks from the road as possible for you to really improve your productivity. We feel that we not only made a sharp knife, but also added a serrated side for those stubborn meals.</p> 
       <p>We have been able to solve all our problems without modifying the CMake codebase. This means that you will be able to use the publicly available versions of CMake, and we intend to keep it that way. You can update CMake to take advantage of its new features and improvements without waiting on us.</p> 
       <p>As you may expect, solving the complexities we have in a game engine cannot be achieved without introducing some complexity. To manage this complexity, we have introduced some levels of abstraction. This has been done with the CMake language itself by creating CMake functions. These functions handle most of the burden of setting up a target and making it work within the engine.</p> 
       <p>Here is an example of a portion of what the Camera Gem looks like:</p> 
       <pre><code class="lang-cpp">ly_add_target(
    NAME Camera.Static STATIC
    NAMESPACE Gem
    FILES_CMAKE
        camera_files.cmake
    INCLUDE_DIRECTORIES
        PRIVATE
            Source
    BUILD_DEPENDENCIES
        PUBLIC
            Gem::Atom_RPI.Public
            AZ::AtomCore
        PRIVATE
            Legacy::CryCommon
)

ly_add_target(
    NAME Camera ${PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE}
    NAMESPACE Gem
    FILES_CMAKE
        camera_shared_files.cmake
    INCLUDE_DIRECTORIES
        PRIVATE
            Source
    BUILD_DEPENDENCIES
        PRIVATE
            Legacy::CryCommon
            Gem::Camera.Static
)

if (PAL_TRAIT_BUILD_HOST_TOOLS)

    ly_add_target(
        NAME Camera.Editor MODULE
        NAMESPACE Gem
        FILES_CMAKE
            camera_editor_files.cmake
        COMPILE_DEFINITIONS
            PRIVATE
                CAMERA_EDITOR
        INCLUDE_DIRECTORIES
            PRIVATE
                Source
        BUILD_DEPENDENCIES
            PRIVATE
                Legacy::CryCommon
                Legacy::Editor.Headers
                AZ::AzToolsFramework
                Gem::Camera.Static
    )

endif()</code></pre> 
       <ul> 
        <li>Modules are DLLs that you can’t link against.</li> 
        <li>Camera.Static is a static library that shares code between the Camera and Camera.Editor modules.</li> 
        <li>We keep the list of files in a separate file, such as “camera_files.cmake”, to keep the config file small and easily maintained. The list of files is easy to differentiate.</li> 
        <li>INCLUDE_DIRECTORIES defines the private/public/interface folders of your target.</li> 
        <li>BUILD_DEPENDENCIES defines targets you depend on. If you depend on a header-only library, you will be using the public/interface include paths from it. If you depend on a static library, you link against it. If you depend on a module you will get public/interface libraries and include paths.</li> 
        <li>We defined “namespaces” in a way to organize our targets. They are not necessary but we encourage using them, especially as the amount of available targets grows.</li> 
        <li>PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE is a variable that expands to MODULE for non-monolithic builds and to STATIC for monolithic builds. We can define different values for different platforms.</li> 
       </ul> 
       <p>These targets are simple since they don’t have platform specific code and are available in all platforms. It also doesn’t have tests. To keep the size of this post small, I have not included other examples. Leave a comment and I can share some more complex ones!</p> 
       <p>Some other improvements we did as part of the CMake conversion:</p> 
       <ul> 
        <li>We reduced the 11 configurations we had to 3: debug, profile, and release. The “dedicated” variants (which were the ones used to build “Servers”) are now targets, and you can build a “ServerLauncher”. The “test” variants are also targets. This means you don’t have to switch between configurations and build the same thing multiple times.<br> A direct result to this is that when working with multiple variants, there is a ~3x build time improvement:</li> 
       </ul> 
       <p><img class="aligncenter wp-image-3248 size-large" src="/blogs/lumberyard-build-system/images/Build-System-Blog_image1-1024x470.png" alt="" width="1024" height="470"></p> 
       <p>Most developers worked with two configurations: profile and profile_test. Now they can work on profile and will be building half of what they were doing before.</p> 
       <ul> 
        <li>We no longer have “specs”. Instead, each target has the right dependencies defined.</li> 
        <li>We introduced another type of dependency that CMake doesn’t handle out of the box: “RUNTIME_DEPENDENCIES”. These are dependencies that we depend on at runtime. (For example: loading dynamically a library or spawning a process). This means that if you build your game, you will get all the dependencies built. If you build the editor, you will get all the Gems and dependencies built, including the Asset Processor.<br> This is VERY IMPORTANT, we now discourage to “build everything”, instead, developers are encouraged to iterate on smaller targets. For example, my preferred workflow is to set a unit test as a startup project, change the debugging parameter to run my suite/test, then iterate on the code in debug. Using things like “building this file”, “edit/continue” and “incremental linking” makes my iteration fast. Once I am happy with the change, I set the Editor as my startup project and run it. If my change is exercised by the game, I set GameLauncher as my startup project and debug it.</li> 
        <li>We removed <em>uber</em> builds. Instead, we are using <a href="https://cmake.org/cmake/help/latest/prop_tgt/UNITY_BUILD.html" target="_blank" rel="noopener noreferrer">CMake’s “Unity” builds</a>.</li> 
        <li>We removed pre-compiled headers from our engine. We had too many issues with them. We found that for most targets, using Unity builds takes care of most of the improvement. Pre-compiled headers showed a ~15% of improvement for “big” targets. We estimated that we could get 1-3% total gain. We did several clean ups and plan to do more to compensate for it.</li> 
        <li>There is no longer the notion of “enabling a Gem”. Instead, we have two Gem actions: building and loading. A game that depends on a specific Gem will do both. Now, just by indicating that dependency, we can generate the required files to make the game load the Gem correctly at runtime. We also are enabling ways to load Gems without building, and building without loading a Gem, to cover more complex scenarios.</li> 
        <li>Monolithic-type builds are no longer hardcoded in a configuration or platform. Instead, it is a CMake cache variable. You can create a “monolithic debug” to debug something further. They are also no longer a mix between static libs and “blobs of objs”. Doing monolithic makes PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE to be “STATIC” instead of “MODULE”. We also have some symbol-stripping prevention logic on the right places.</li> 
       </ul> 
       <p>Finally, the biggest improvement is in developer’s productivity. We have observed internally that developers are able to use Visual Studio and productivity tools more efficiently, as they get less false-positives from the build system and are able to move quicker with changes. A combination of multiple factors is at play:</p> 
       <ul> 
        <li>Faster small iterations, e.g. building one file is just whatever the compiler takes.</li> 
        <li>Less regeneration with CMake’s ZERO_CHECK and proper “regeneration dependency tracking”.</li> 
        <li>Better IDE integration. Projects have the properties, individual files override them if different. Developers can tweak properties on Visual Studio.</li> 
        <li>Better IDE integration in VS, as pressing F5 takes no time if it is already built.</li> 
        <li>Incremental linking in debug.</li> 
        <li>Edit/Continue in debug.</li> 
        <li>Allowing the developer to pick the target they want to build and debug (great to iterate on tests).</li> 
        <li>Cleanups we did.</li> 
        <li>And many others…</li> 
       </ul> 
       <p>There is so much to cover that we focused on only the most relevant pieces. Feel free to ask for more details in the areas that you are interested about.</p> 
       <p>We can’t wait to get our new “shiny knife” in your hands. We have been enjoying this internally and have noticed huge gains in our developer workflows. We can put the worn wooden knife of WAF back in the drawer; despite the fact that it brought us many positive moments as well as challenging us, it was time to move on.</p> 
       