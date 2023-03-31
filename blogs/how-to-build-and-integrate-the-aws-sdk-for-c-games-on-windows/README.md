# How to build and integrate the AWS SDK for C++ games on Windows

<p>Following on from our first post ‘Game developer’s guide to setting up the AWS SDK’, we’re now going to show you how to integrate the SDK for C++, the language used in major game engines like Unreal and Lumberyard.</p> 
       <p>To help you get up and running even faster, we’ve created a sample code package that is available for you to <a href="https://github.com/aws-samples/aws-sdk-getting-started-message-of-the-day" target="_blank" rel="noopener noreferrer">download from GitHub</a>.</p> 
       <h2>Setting up the client</h2> 
       <p>Using the sample code above, you’re now ready to run the code demo client. However, it doesn’t show you how to add the AWS C++ SDK in to your own game, so we’ll explain that here.</p> 
       <p>With Windows, using Visual Studio 2017 or higher, the easiest way to add the necessary libraries is with the NuGet package manager. This demo uses the AWSSDKCPP-IdentityManagent and AWSSDKCPP-Lambda packages. NuGet is helpful because it will pull in any other packages that your top level packages depend on, it adds all the libraries, includes the paths needed to build, and will copy any DLL’s needed to the output folder.</p> 
       <p>However, due to limitations of NuGet, it’s no longer officially supported by AWS and the last version of the AWS C++ SDK available from NuGet is 1.6. If you need features not found in the NuGet SDK, you can build the libraries yourself. You will also need to do this if using other operating systems. You can find more info in the <a href="https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/setup.html" target="_blank" rel="noopener noreferrer">developer guide</a>.</p> 
       <h2>Building the SDK</h2> 
       <p>Here we’ll focus on building the source from GitHub using the CMake/msbuild tools.</p> 
       <p>Note: this guide assumes you are using Visual Studio 2017 and you want to build 64-bit libraries.</p> 
       <ol> 
        <li>Go to the <a href="https://github.com/aws/aws-sdk-cpp" target="_blank" rel="noopener noreferrer">SDK source for C++</a> </li> 
        <li>To get the source, either download the zip file from GitHub or, if you have Git installed, run the command: <pre>git clone https://github.com/aws/aws-sdk-cpp.git</pre> <p>We encourage cloning the SDK as it will make it easier for you to update in the future.</p></li> 
        <li>If you don’t already have it, get a copy of <a href="https://cmake.org/" target="_blank" rel="noopener noreferrer">CMake</a> and install it.If you have Visual Studio 2017 or higher, there’s a good chance you already have CMake installed! To check, open the Developer Command Prompt for VS2017 (found in your Start Menu’s Visual Studio folder) and type <pre>cmake</pre> <p>. If you see the usage info, it’s installed. Be aware that you’ll need the command line version of git installed and in your path for the later CMake step to work. Visual Studio 2017 comes with a git client installed, however it’s not in your path by default. This can be found in the Visual Studio installation path, for example in the community version it would usually be in</p> <pre>C:\Program Files(x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\TeamExplorer\Git\cmd</pre> <p>Add that to your path and you should be good to go.</p></li> 
        <li>Once you have CMake installed, you’ll need to run a command line to perform the build itself. From your Start Menu, find the icon for “Developer Command Prompt for VS 2017” in your Visual Studio folder.Be sure to right click on the console icon to run as administrator or you will get errors at the end of the build.</li> 
        <li>Go to folder <pre>/&lt;path&gt;/aws-sdk-cpp-master/</pre> <p>and call</p> <pre>cmake</pre> <p>on it as below to create the make files for msbuild to operate on:</p> <pre>cmake -G "Visual Studio 15 2017 Win64"</pre> <p>If you remove the Win64 part, it will build 32 bit. You can find more info on build options here: https://cmake.org/cmake/help/v3.7/manual/cmake-generators.7.html</p></li> 
        <li>You can switch out versions and release types with option flags like: <pre>-DCMAKE_BUILD_TYPE=Release</pre> <p>Or</p> <pre>-DCMAKE_BUILD_TYPE=Debug</pre> </li> 
        <li>If you want to make specific pieces, you can specify them with the following flags (make sure to use semi-colons to separate the targets): <pre>-DBUILD_ONLY="aws-cpp-sdk-core;aws-cpp-sdk-lambda"</pre> </li> 
        <li>Next, build the libraries by calling: <pre>msbuild INSTALL.vcxproj /p:Configuration=Release</pre> <p>Which will build the libs in each folder.</p></li> 
       </ol> 
       <p>&nbsp;</p> 
       <h2>Adding the AWS C++ SDK in to your game</h2> 
       <p>Now that the AWS C++ SDK is built, you’ll need to add it to your project. The following instructions are for adding the libraries and including files to an existing project.</p> 
       <p>If you’re starting a new project, consider using CMake, and the export functionality to create your Visual Studio project with all the correct paths and libraries already set up. You can learn more in <a href="https://aws.amazon.com/blogs/developer/using-cmake-exports-with-the-aws-sdk-for-c/" target="_blank" rel="noopener noreferrer">this article.</a></p> 
       <p>For this example we will use</p> 
       <pre>AWSSDK\aws-sdk-cpp-master</pre> 
       <p>as the path that we unpacked and compiled the SDK with.</p> 
       <p>Set the correct configuration (Debug or Release) and Platform &nbsp;(x86 or x64) as required using Visual Studio’s project properties dialog box.</p> 
       <p>At this point you’ll have the libraries build in to locations like this:</p> 
       <pre>AWSSDK\aws-sdk-cpp-master\aws-cpp-sdk-core\Release\aws-cpp-sdk-core.lib</pre> 
       <p>The include directories will look like:</p> 
       <pre>AWSSDK\aws-sdk-cpp-master\aws-cpp-sdk-core\include</pre> 
       <p>You’ll also need to use the following preprocessor defines:</p> 
       <pre>USE_IMPORT_EXPORT
USE_WINDOWS_DLL_SEMANTICS</pre> 
       <p>If you don’t, you may experience this error when linking:</p> 
       <pre>error: LNK2001: unresolved external symbol "char const * const Aws::Http::CONTENT_TYPE_HEADER"</pre> 
       <p>As a last note, be sure to copy the DLLs to the folder with the executable.</p> 
       <p>Once you have all of that, you are ready to integrate the services you wish to use.</p> 
       <p>For more information, <a href="https://docs.aws.amazon.com/sdk-for-cpp/index.html" target="_blank" rel="noopener noreferrer">read the AWS C++ SDK documentation</a>.</p> 
       <h2>The code</h2> 
       <p>First, grab the <a href="https://github.com/aws-samples/aws-sdk-getting-started-message-of-the-day" target="_blank" rel="noopener noreferrer">sample package from GitHub.</a></p> 
       <p>Any time you use the AWS SDK for C++ you need to initialize it and then shut it down when you’re done, after you’ve ran your last AWS command.</p> 
       <pre><code class="lang-cpp">Aws::SDKOptions options;

Aws::Utils::Logging::LogLevel logLevel{ Aws::Utils::Logging::LogLevel::Error };
options.loggingOptions.logger_create_fn = [logLevel] {return make_shared&lt;Aws::Utils::Logging::ConsoleLogSystem&gt;(logLevel); };

Aws::InitAPI(options);
...
Aws::ShutdownAPI(options);
</code></pre> 
       <p>One little extra, we’ve also set the log level. Though it’s set to the default level right now (Error), if you want to see what the SDK is doing behind the scenes, set this to Trace. It’s great for learning a bit about how the SDK operates, as well as finding the occasional tricky error. For example, we used it to find why a token wasn’t being accepted and we were able to see what was being transmitted to AWS. It turned out the format was slightly inconsistent, and we’d never had spotted that from the debugger.</p> 
       <p>The next step is the code to allow your anonymous (unauthenticated) users to make calls to AWS resources we’ve authorized access for via IAM:</p> 
       <pre><code class="lang-cpp">auto credentialsProvider = Aws::MakeShared&lt;Aws::Auth::CognitoCachingAnonymousCredentialsProvider&gt;("CredentialsProvider", ACCT_ID, POOL_ID);</code></pre> 
       <p>The anonymous credential API in the SDK for C++ requires you to put in your account ID, along with the Amazon Cognito Pool ID. You can find the account ID in the AWS console by clicking “My Account” from the popup menu at the top right. Just paste it in to the settings.h file along with the pool ID from the steps above.</p> 
       <p>The <code>credentialsProvider</code> can now be passed in whenever you create a “client” for a particular AWS service.</p> 
       <pre><code class="lang-cpp">&nbsp;static shared_ptr&lt;Aws::Lambda::LambdaClient&gt; s_LambdaClient = Aws::MakeShared&lt;Aws::Lambda::LambdaClient&gt;("LambdaClient", credentialsProvider);</code></pre> 
       <p>This is a pattern used throughout the AWS SDK for C++. For every major system like Lambda, S3, Amazon Cognito, etc, you create a client. The client will be responsible for making calls to AWS and typically contains most of the AWS controlling methods. For example, the code for invoking an AWS Lambda looks like this:</p> 
       <pre><code class="lang-cpp">    Aws::Lambda::Model::InvokeRequest invokeRequest;
    invokeRequest.SetFunctionName("GetMOTD");
    invokeRequest.SetInvocationType(Aws::Lambda::Model::InvocationType::RequestResponse);
    std::shared_ptr&lt;Aws::IOStream&gt; payload = Aws::MakeShared&lt;Aws::StringStream&gt;("LambdaFunctionRequest");
    Aws::Utils::Json::JsonValue jsonPayload;
    jsonPayload.WithString("param1", param1);
    jsonPayload.WithInteger("param2", param2);
    jsonPayload.WithInteger("param3", param3);
    *payload &lt;&lt; jsonPayload.View().WriteReadable();
    invokeRequest.SetBody(payload);
    invokeRequest.SetContentType("application/javascript");

    auto outcome = s_LambdaClient-&gt;Invoke(invokeRequest);

    if (outcome.IsSuccess())
    {
        auto&amp; result = outcome.GetResult();
        Aws::Utils::Json::JsonValue resultPayload{ result.GetPayload() };
        auto jsonView = resultPayload.View();

        if (jsonView.ValueExists("body"))
        {
            cout &lt;&lt; "Response body: " &lt;&lt; jsonView.GetString("body") &lt;&lt; endl &lt;&lt; endl;
        }
        else
        {
            cout &lt;&lt; "Unable to parse response body!" &lt;&lt; endl &lt;&lt; endl;
        }

    }
    else
    {
        auto error = outcome.GetError();
        cout &lt;&lt; "Error invoking lambda: " &lt;&lt; error.GetMessage() &lt;&lt; endl &lt;&lt; endl;

    }
</code></pre> 
       <p>Again, throughout the AWS C++ SDK, this is the pattern you’ll use to make the calls. You’ll create a “request” specific to the method you’re going to call. In this case we want to invoke a Lambda function, so we’ll call the Invoke function on the Lambda client. The actual parameters are stored in an <code>InvokeRequest()</code>, which has various accessors and methods to make it easy to form the request.</p> 
       <p>Once the method is called, it returns an “outcome” which is specific to the method we invoked, in this case, <code>InvokeOutcome()</code> (but you can use auto to make your life easier by not having to remember all this.)</p> 
       <p>The outcome tells you if the call was successful or not, containing error information if not. If it’s successful you can then grab a “response” from the outcome, which is also specific to the method called <code>InvokeRequest()</code>. The outcome, much like the request, will contain accessors and methods to make it easy to figure out what the method returned.</p> 
       <p>It’s worth noting, AWS stands for “Amazon Web Services” which is why JSON is so prevalent here. The AWS SDK for C++ is pretty much a wrapper around a bunch of HTTP service calls. Fortunately, the SDK for C++ has a JSON library built in so you don’t need to find one.</p> 
