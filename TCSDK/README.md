
<html>
<body>
<p><img alt="alt tag" src="../res/ca_logo.png" /></p>
<p><img alt="alt tag" src="../res/Tag_Commander.jpg" /></p>
<h1 id="sdks-implementation-guide">SDK's Implementation Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>25/03/2019</em><br />
Release version : <em>4.3.1</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#sdks-implementation-guide">SDK's Implementation Guide</a></li>
<li><a href="#introduction">Introduction</a><ul>
<li><a href="#main-technical-specifications">Main Technical Specifications</a></li>
<li><a href="#dynamic-variables">Dynamic Variables</a></li>
<li><a href="#execution">Execution</a></li>
</ul>
</li>
<li><a href="#sdk-integration">SDK integration</a><ul>
<li><a href="#steps">Steps</a></li>
<li><a href="#integration-of-the-sdk-module">Integration of the SDK Module</a></li>
<li><a href="#gradle-additions">Gradle additions</a></li>
<li><a href="#android-permissions">Android permissions</a></li>
<li><a href="#compatibility">Compatibility</a></li>
</ul>
</li>
<li><a href="#using-the-sdk">Using the SDK</a><ul>
<li><a href="#initialisation">Initialisation</a></li>
<li><a href="#executing-tags">Executing tags</a></li>
<li><a href="#example">Example</a></li>
<li><a href="#product-tags">Product tags</a></li>
<li><a href="#using-proguard">Using ProGuard</a></li>
<li><a href="#install-referrer">Install Referrer</a></li>
<li><a href="#background-mode">Background Mode</a></li>
<li><a href="#deactivating-the-sdk">Deactivating the SDK</a></li>
</ul>
</li>
<li><a href="#troubleshooting">Troubleshooting</a><ul>
<li><a href="#debugging">Debugging</a></li>
<li><a href="#testing">Testing</a></li>
<li><a href="#network-monitor">Network monitor</a></li>
<li><a href="#common-errors">Common errors</a></li>
<li><a href="#common-errors-with-the-tagging-plan">Common errors with the tagging plan</a></li>
</ul>
</li>
<li><a href="#helpers">Helpers</a><ul>
<li><a href="#persisting-variables">Persisting variables</a></li>
<li><a href="#tcpredefinedvariables">TCPredefinedVariables</a></li>
</ul>
</li>
<li><a href="#kotlin">Kotlin</a></li>
<li><a href="#example-tcdemo">Example: TCDemo</a></li>
<li><a href="#migration-from-v2-and-v3-to-v4">Migration from v2 and v3 to v4</a></li>
<li><a href="#support-and-contacts">Support and contacts</a></li>
</ul>
</div>
<h1 id="introduction">Introduction</h1>
<p>Commanders Act enables marketers to easily add, edit, update, and deactivate tags on web pages, videos and mobile applications with little-to-no support from IT departments.</p>
<p>Instead of implementing several SDK's in the application, Commanders Act for mobile provides clients with a single SDK which sends data to our server which then create and send hits to your partners.</p>
<p>Owing to remote configuration tools, it is also possible to modify the configuration without having to resubmit your application.</p>
<p>The purpose of this document is to explain how to add the SDK module into your application.</p>
<h2 id="main-technical-specifications">Main Technical Specifications</h2>
<ul>
<li>Weight about 40 ko in your application.</li>
<li>Fully threaded and asynchronous.</li>
<li>Automatic enabling / disabling depending on the Internet reachability or battery level.</li>
<li>Offline mode (the hits are stored in the phone to be replayed at the next launch.)</li>
<li>Very low CPU and memory usage.</li>
<li>Dynamic variable storage. If a value never changes, it's possible to set it only once.</li>
<li>The state of the phone is easily accessed through the module (network connection type, name of the phone, geographical location.)</li>
<li>Background mode, in the case you really need to send data while the application is in background.</li>
</ul>
<h2 id="dynamic-variables">Dynamic Variables</h2>
<p>A dynamic variable is a combination of a name and a value. It is used to give the module data such as the name of the current screen or the product ID in a cart.</p>
<p>Dynamic Variables are implemented inside the application. They are replaced on the server at the time of the execution by the value transmitted.</p>
<p>A dynamic variable is formatted like this: <code>#SCREEN#</code>.</p>
<div class="warning"></div>

<blockquote>
<p>The dynamic variables are case sensitive. They should always be in
upper case. The dynamic variable both begins and ends with a <code>#</code>. Don't
forget them when setting your dynamic variables.</p>
</blockquote>
<p>Dynamic Variables has two purposes:</p>
<ol>
<li>Define information for analytics purposes. For instance, you would put <code>restaurants_list</code> in the <code>#SCREEN#</code> dynamic variable.</li>
<li>Test if conditions are met to fire a tag. For instance, if you set the <code>#EVENT#</code> to <code>click</code>, the tag with the condition <code>#EVENT# EQUAL 'click'</code> will be executed.</li>
</ol>
<h2 id="execution">Execution</h2>
<p>When you call the sendData method, a hit will be packaged and sent to Commanders Act's server.</p>
<p><img alt="alt tag" src="../res/sdk_scheme.png" /></p>
<h1 id="sdk-integration">SDK integration</h1>
<h2 id="steps">Steps</h2>
<p>You can divide the integration of TagCommander's SDK into the next few steps:</p>
<ol>
<li>Adding the TagCommander library to your Project.</li>
<li>Implementing TagCommander by tagging your application.</li>
<li>Verify that all tags are being sent.</li>
</ol>
<h2 id="integration-of-the-sdk-module">Integration of the SDK Module</h2>
<p><a href="../README.md">Please check the Developers Implementation Guide to chose the best way to implement this module in your project.</a></p>
<h2 id="gradle-additions">Gradle additions</h2>
<p>You need to add some dependencies in your build.gradle file for the SDK to work properly. You will need a tiny bit of google play services which is location.</p>
<div class="codehilite"><pre><span></span><span class="n">implementation</span> <span class="s1">&#39;com.google.android.gms:play-services-location:15.0.1&#39;</span>
<span class="n">implementation</span> <span class="s1">&#39;com.android.support:appcompat-v7:27.1.1&#39;</span>
</pre></div>


<p>The SDK module is compiled with the following dependencies :</p>
<div class="codehilite"><pre><span></span><span class="n">implementation</span> <span class="n">project</span><span class="p">(</span><span class="s1">&#39;:core&#39;</span><span class="p">)</span>
<span class="n">implementation</span> <span class="s1">&#39;com.android.support:appcompat-v7:27.1.1&#39;</span>
</pre></div>


<h2 id="android-permissions">Android permissions</h2>
<p>TagCommander requires the following permissions:</p>
<ul>
<li>android.permission.INTERNET</li>
<li>android.permission.ACCESS_NETWORK_STATE</li>
<li>android.permission.ACCESS_WIFI_STATE</li>
</ul>
<p>If not already done, you should also add the dependecy to the GMS library:</p>
<div class="codehilite"><pre><span></span><span class="nt">&lt;meta-data</span> <span class="na">android:name=</span><span class="s">&quot;com.google.android.gms.version&quot;</span>
 <span class="na">android:value=</span><span class="s">&quot;@integer/google_play_services_version&quot;</span> <span class="nt">/&gt;</span>
</pre></div>


<p>Localisation is a special case, if you want to use localisation, you will need to initialise the TCLocation class after TagCommander.</p>
<p>As of Android's SDK 23, we are only using the LOCATION group of permissions. In the case you need the latitude or longitude of the user, you will first need to ask your user for the LOCATION permission with "AppCompatActivity.requestPermissions".</p>
<div class="codehilite"><pre><span></span><span class="n">TCLocation</span><span class="o">.</span><span class="na">getInstance</span><span class="o">(</span><span class="n">context</span><span class="o">);</span>
</pre></div>


<p>And you will also need to add the following permission:</p>
<ul>
<li>android.permission.ACCESS_FINE_LOCATION</li>
<li>android.permission.ACCESS_COARSE_LOCATION</li>
</ul>
<h2 id="compatibility">Compatibility</h2>
<ul>
<li>Minimum Android version is 15.</li>
<li>Build Target version is 27.</li>
<li>Build Tools Version : 27.0.3.</li>
</ul>
<h1 id="using-the-sdk">Using the SDK</h1>
<h2 id="initialisation">Initialisation</h2>
<p>It is recommended to initialize TagCommander in your <code>onCreate(Bundle savedInstanceState)</code> in your <code>MainActivity</code> so it will be operational as soon as possible.
<code>TC_SITE_ID</code> and <code>TC_CONTAINER_ID</code> are integers provided by Commanders Act.
You need to pass your application context while instantiating TagCommander. The <code>context</code> is usually simply your activity from which we will be sure to get the application context.</p>
<p>A single line of code is required to properly initialize an instance of TagCommander:</p>
<div class="codehilite"><pre><span></span><span class="c1">//!\\ Very important while integrating TagCommander</span>
<span class="n">TCDebug</span><span class="o">.</span><span class="na">setDebugLevel</span><span class="o">(</span><span class="n">Log</span><span class="o">.</span><span class="na">VERBOSE</span><span class="o">);</span>

<span class="n">TagCommander</span> <span class="n">TCInstance</span> <span class="o">=</span> <span class="k">new</span> <span class="n">TagCommander</span><span class="o">(</span><span class="n">TC_SITE_ID</span><span class="o">,</span> <span class="n">TC_CONTAINER_ID</span><span class="o">,</span> <span class="k">this</span><span class="o">);</span>
</pre></div>


<div class="warning"></div>

<blockquote>
<p>This class is not a Singleton. If you have only needs for one pair
of <code>site ID</code> and <code>container ID</code>, you might want to use it as a Singleton
anyway for a greater ease of use.</p>
</blockquote>
<p>If you want to use localisation, you will need to initialise the TCLocation class after TagCommander.</p>
<div class="codehilite"><pre><span></span><span class="n">TCLocation</span><span class="o">.</span><span class="na">getInstance</span><span class="o">(</span><span class="n">context</span><span class="o">);</span>
</pre></div>


<p>We have set the default setInterval to 30 minutes to save battery. If you need another time precision, you can set TCLocation.GPSInterval to any value and it will be used instead of the default value.</p>
<h2 id="executing-tags">Executing tags</h2>
<p>For every element that needs tagging in your application, you need to call addData on your TagCommander instance and when you want to send all those information to the server, you will simply need to call sendData.</p>
<div class="codehilite"><pre><span></span><span class="n">TCInstance</span><span class="o">.</span><span class="na">addData</span><span class="o">(</span><span class="s">&quot;#EVENT#&quot;</span><span class="o">,</span> <span class="s">&quot;click&quot;</span><span class="o">);</span>
<span class="n">TCInstance</span><span class="o">.</span><span class="na">addData</span><span class="o">(</span><span class="s">&quot;#PAGE#&quot;</span><span class="o">,</span> <span class="s">&quot;order&quot;</span><span class="o">);</span>
<span class="n">TCInstance</span><span class="o">.</span><span class="na">addData</span><span class="o">(</span><span class="s">&quot;#AMOUNT#&quot;</span><span class="o">,</span> <span class="s">&quot;10000&quot;</span><span class="o">);</span> <span class="c1">// don&#39;t forget to put numbers as String</span>

<span class="n">TCInstance</span><span class="o">.</span><span class="na">sendData</span><span class="o">();</span>
</pre></div>


<p>For compatibility reasons, we can still use TCAppVars to pass those information to TagCommander.</p>
<div class="codehilite"><pre><span></span><span class="n">TCAppVars</span> <span class="n">appVars</span> <span class="o">=</span> <span class="k">new</span> <span class="n">TCAppVars</span><span class="o">();</span>
<span class="n">appVars</span><span class="o">.</span><span class="na">put</span><span class="o">(</span><span class="s">&quot;#EVENT#&quot;</span><span class="o">,</span> <span class="s">&quot;click&quot;</span><span class="o">);</span>
<span class="n">appVars</span><span class="o">.</span><span class="na">put</span><span class="o">(</span><span class="s">&quot;#PAGE#&quot;</span><span class="o">,</span> <span class="s">&quot;order&quot;</span><span class="o">);</span>
<span class="n">appVars</span><span class="o">.</span><span class="na">put</span><span class="o">(</span><span class="s">&quot;#AMOUNT#&quot;</span><span class="o">,</span> <span class="s">&quot;10000&quot;</span><span class="o">);</span> <span class="c1">// don&#39;t forget to put numbers as String</span>

<span class="n">TCInstance</span><span class="o">.</span><span class="na">execute</span><span class="o">(</span><span class="n">appVars</span><span class="o">);</span>
</pre></div>


<div class="warning"></div>

<blockquote>
<p>Always handle values as String !</p>
</blockquote>
<h2 id="example">Example</h2>
<p>Let's say that the URL you are using in your server-side container uses the following url:</p>
<div class="codehilite"><pre><span></span>http://engage.commander1.com/dms?tc_s=3109&amp;tc_type=dms&amp;data_sysname=#TC_SYSNAME#
&amp;data_sysversion=#TC_SYSVERSION#&amp;page=#SCREEN_NAME#&amp;event=#EVENT#
</pre></div>


<p>In order to be executed, the tag needs two values:</p>
<ul>
<li><code>#EVENT#</code></li>
<li><code>#SCREEN_NAME#</code></li>
</ul>
<p>With the code from the previous section, this tag could be fired from Commanders Act's server. The application sends two dynamic variables (<code>#EVENT#</code> and <code>#SCREEN_NAME#</code>) and the SDK adds all information available to it (like #TC_SYSVERSION# and #TC_SYSNAME# in this hit).</p>
<h2 id="product-tags">Product tags</h2>
<p>There are some tags that need to be passed a list of dictionaries, usually representing products. By passing complex information, we are able to create and send complex hits or many hits at the same time.</p>
<p>Tags that needs to be passed a list of dictionaries are easy to spot in the configuration. They have appended to the name of the dynamic variable the name of the key that is retrieved from the dictionary.</p>
<p>Most of the time the data are provided ready to use, but we provide a TCProduct class representing a product and its possible values.</p>
<div class="codehilite"><pre><span></span><span class="kd">public</span> <span class="kt">void</span> <span class="nf">SendViewCart</span><span class="o">()</span>
<span class="o">{</span>
  <span class="n">TCInstance</span><span class="o">.</span><span class="na">addData</span><span class="o">(</span><span class="s">&quot;#REGIONAL_CODE#&quot;</span><span class="o">,</span> <span class="s">&quot;eu&quot;</span><span class="o">);</span>
  <span class="n">TCInstance</span><span class="o">.</span><span class="na">addData</span><span class="o">(</span><span class="s">&quot;#EVENT#&quot;</span><span class="o">,</span> <span class="s">&quot;viewCart&quot;</span><span class="o">);</span>
  <span class="n">TCInstance</span><span class="o">.</span><span class="na">addData</span><span class="o">(</span><span class="s">&quot;#PARTNER_ID#&quot;</span><span class="o">,</span> <span class="s">&quot;868&quot;</span><span class="o">);</span>

  <span class="c1">// adding products</span>
  <span class="n">TCProduct</span> <span class="n">product1</span> <span class="o">=</span> <span class="k">new</span> <span class="n">TCProduct</span><span class="o">();</span>
  <span class="n">product1</span><span class="o">.</span><span class="na">ID</span> <span class="o">=</span> <span class="s">&quot;22561563&quot;</span><span class="o">;</span>
  <span class="n">product1</span><span class="o">.</span><span class="na">priceATI</span> <span class="o">=</span> <span class="s">&quot;253.50&quot;</span><span class="o">;</span>
  <span class="n">product1</span><span class="o">.</span><span class="na">quantity</span> <span class="o">=</span> <span class="s">&quot;1&quot;</span><span class="o">;</span>
  <span class="n">TCInstance</span><span class="o">.</span><span class="na">addProduct</span><span class="o">(</span><span class="s">&quot;#ORDER_PRODUCTS#&quot;</span><span class="o">,</span> <span class="n">product1</span><span class="o">);</span>

  <span class="n">TCProduct</span> <span class="n">product2</span> <span class="o">=</span> <span class="k">new</span> <span class="n">TCProduct</span><span class="o">();</span>
  <span class="n">product2</span><span class="o">.</span><span class="na">ID</span> <span class="o">=</span> <span class="s">&quot;21669790&quot;</span><span class="o">;</span>
  <span class="n">product2</span><span class="o">.</span><span class="na">priceATI</span> <span class="o">=</span> <span class="s">&quot;223.10&quot;</span><span class="o">;</span>
  <span class="n">product2</span><span class="o">.</span><span class="na">quantity</span> <span class="o">=</span> <span class="s">&quot;2&quot;</span><span class="o">;</span>
  <span class="n">TCInstance</span><span class="o">.</span><span class="na">addProduct</span><span class="o">(</span><span class="s">&quot;#ORDER_PRODUCTS#&quot;</span><span class="o">,</span> <span class="n">product2</span><span class="o">);</span>

  <span class="n">TCProduct</span> <span class="n">product3</span> <span class="o">=</span> <span class="k">new</span> <span class="n">TCProduct</span><span class="o">();</span>
  <span class="n">product3</span><span class="o">.</span><span class="na">ID</span> <span class="o">=</span> <span class="s">&quot;3886822&quot;</span><span class="o">;</span>
  <span class="n">product3</span><span class="o">.</span><span class="na">priceATI</span> <span class="o">=</span> <span class="s">&quot;45.99&quot;</span><span class="o">;</span>
  <span class="n">product3</span><span class="o">.</span><span class="na">quantity</span> <span class="o">=</span> <span class="s">&quot;3&quot;</span><span class="o">;</span>
  <span class="n">TCInstance</span><span class="o">.</span><span class="na">addProduct</span><span class="o">(</span><span class="s">&quot;#ORDER_PRODUCTS#&quot;</span><span class="o">,</span> <span class="n">product3</span><span class="o">);</span>

  <span class="n">TCInstance</span><span class="o">.</span><span class="na">sendData</span><span class="o">();</span>
<span class="o">}</span>
</pre></div>


<p>The following properties can be used with the TCProduct class:</p>
<ul>
<li>ID</li>
<li>name</li>
<li>quantity</li>
<li>category</li>
<li>priceATI</li>
<li>discountATI</li>
<li>priceTF</li>
<li>discountTF</li>
<li>trademark</li>
<li>URLPage</li>
<li>URLPicture</li>
<li>rating</li>
<li>inStock</li>
</ul>
<p>If you want to add more properties, please use the method on your TCProduct instance:</p>
<div class="codehilite"><pre><span></span><span class="n">product</span><span class="o">.</span><span class="na">customProperties</span><span class="o">.</span><span class="na">put</span><span class="o">(</span><span class="s">&quot;Menu&quot;</span><span class="o">,</span> <span class="s">&quot;12&quot;</span><span class="o">);</span>
<span class="n">product</span><span class="o">.</span><span class="na">customProperties</span><span class="o">.</span><span class="na">put</span><span class="o">(</span><span class="s">&quot;TakeOut&quot;</span><span class="o">,</span> <span class="s">&quot;0&quot;</span><span class="o">);</span>
</pre></div>


<div class="warning"></div>

<blockquote>
<p>If you are updating from an old version of the SDK you can still use old functions with TCAppVars and a list of products.</p>
</blockquote>
<h2 id="using-proguard">Using ProGuard</h2>
<p>If your release build uses ProGuard to strip and obfuscate your code, you will need to add some configuration for the modules to keep working.</p>
<p>Just add the following line in your proguard-rules.pro.</p>
<div class="codehilite"><pre><span></span><span class="o">-</span><span class="n">keep</span> <span class="k">class</span> <span class="n">com</span><span class="o">.</span><span class="n">tagcommander</span><span class="o">.</span><span class="n">lib</span><span class="o">.</span><span class="n n-Operator">**</span> <span class="p">{</span> <span class="o">*</span><span class="p">;</span> <span class="p">}</span>
</pre></div>


<h2 id="install-referrer">Install Referrer</h2>
<p>If you want the source channel of your application in Commanders Act, please point the INSTALL_REFERRER broadcast toward our receiver TCReferrerReceiver :</p>
<p>It's as simple as adding the following lines in the AndroidManifest.xml of your application and inside the "Application" tag.</p>
<div class="codehilite"><pre><span></span><span class="nt">&lt;receiver</span>
    <span class="na">android:name=</span><span class="s">&quot;com.tagcommander.lib.TCReferrerReceiver&quot;</span>
    <span class="na">android:exported=</span><span class="s">&quot;true&quot;</span><span class="nt">&gt;</span>
    <span class="nt">&lt;intent-filter&gt;</span>
        <span class="nt">&lt;action</span> <span class="na">android:name=</span><span class="s">&quot;com.android.vending.INSTALL_REFERRER&quot;</span> <span class="nt">/&gt;</span>
    <span class="nt">&lt;/intent-filter&gt;</span>
<span class="nt">&lt;/receiver&gt;</span>
</pre></div>


<p>Once the broadcast is received, TagCommander will store the full string into the #TC_INSTALL_REFERRER# predefined variable.</p>
<p>Example:</p>
<div class="codehilite"><pre><span></span>utm_source=adMob<span class="err">&amp;</span>utm_medium=banner<span class="err">&amp;</span>utm_term=running+shoes<span class="err">&amp;</span>utm_content=theContent
<span class="err">&amp;</span>utm_campaign=couponReduc<span class="err">&amp;</span>anid=adMob
</pre></div>


<h2 id="background-mode">Background Mode</h2>
<p>While the application is goind to background, the SDK sends all data that was already queued then stops. This is in order to preserve battery life and not use carrier data when not required.</p>
<p>But some applications need to be able to continue sending data because they have real background activities. For example listening to music.</p>
<p>For those cases, we added a way to bypass the way to SDK usually react to background. Please call:</p>
<div class="codehilite"><pre><span></span><span class="n">TC</span><span class="o">.</span><span class="na">enableRunningInBackground</span><span class="o">();</span>
</pre></div>


<p>One drawback is that we're not able to ascertain when the application will really be killed. In normal mode, we're saving all hits not sent when going in the background, which is not possible here anymore. To be sure to not loose any hits in background mode, we will save much more often the offline hits. This only applies if the SDK is offline, meaning that you don't have internet or don't have enough battery.</p>
<h2 id="deactivating-the-sdk">Deactivating the SDK</h2>
<p>If you want to show a privacy message to your users allowing them to stop the tracking, you might want to use the following function to stop it if they refuse to be tracked.</p>
<div class="codehilite"><pre><span></span><span class="n">TCInstance</span><span class="o">.</span><span class="na">disableSDK</span><span class="o">();</span>
</pre></div>


<p>What this function does is stopping all systems in the SDK that update automatically or listen to notifications like background or internet reachability. This will also ignore all calls to the SDK by your application so that nothing is treated anymore and you don't have to protect those calls manually.</p>
<div class="codehilite"><pre><span></span><span class="n">TCInstance</span><span class="o">.</span><span class="na">enableSDK</span><span class="o">();</span>
</pre></div>


<p>In the case you need to re-enable it after disabling it the first time, you can use this function.</p>
<h1 id="troubleshooting">Troubleshooting</h1>
<p>The TagCommander SDK also offers methods to help you with the Quality Assessment of the SDK implementation.</p>
<h2 id="debugging">Debugging</h2>
<p>We recommend using <code>Log.VERBOSE</code> while developing your application:</p>
<div class="codehilite"><pre><span></span><span class="cm">/*</span>
<span class="cm"> * Verbose is recommended during test as it prints information</span>
<span class="cm"> * that helps figuring what is working and what&#39;s not.</span>
<span class="cm"> */</span>
  <span class="n">TCDebug</span><span class="o">.</span><span class="na">setDebugLevel</span><span class="o">(</span><span class="n">Log</span><span class="o">.</span><span class="na">VERBOSE</span><span class="o">);</span>
</pre></div>


<ul>
<li>Verbosity</li>
</ul>
<table>
<thead>
<tr>
<th>Constant Name</th>
<th>Verbosity</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>Log.VERBOSE</code></td>
<td>Print everything.</td>
</tr>
<tr>
<td><code>Log.DEBUG</code></td>
<td>Most useful information for debugging</td>
</tr>
<tr>
<td><code>Log.INFO</code></td>
<td>Basic information about TagCommander's state</td>
</tr>
<tr>
<td><code>Log.WARN</code></td>
<td>Warnings only</td>
</tr>
<tr>
<td><code>Log.ERRORS</code></td>
<td>Errors only</td>
</tr>
<tr>
<td><code>Log.ASSERT</code></td>
<td>Assertions only (not used).</td>
</tr>
</tbody>
</table>
<ul>
<li>The internal architecture is working with internal notifications. You can ask the Logger to display all the internal notifications with TCDebug.setNotificationLog(true);.</li>
</ul>
<p>If you don't call TCDebug.setDebugLevel, not log will be printed at all.</p>
<h2 id="testing">Testing</h2>
<p>There are three ways to verify that the module executes the tags in your application:</p>
<ul>
<li>By reading the debug messages in the console.</li>
<li>By going to your vendor's platform and check that the hits are displayed and that the data is correct. Please be aware that hits may not display immediately in the vendor account. This delay differs widely between vendors and may also vary for the type of hit under the same vendor.</li>
<li>You can also use a network monitor like Wireshark or Charles to check directly what is being sent on the wire to your vendors.</li>
</ul>
<h2 id="network-monitor">Network monitor</h2>
<p>Starting Android 7 (Nougat) you will have a bit more troubles while trying to profile your applications with tools like Charles. Google introduced <a href="https://android-developers.googleblog.com/2016/07/changes-to-trusted-certificate.html">changes to trusted certificates</a>.</p>
<p>Basically what is needed in order to see https hits in Charles and alike is a bit more configuration than just adding SSL certificate to the phone. You will need to add in your manifest application:</p>
<div class="codehilite"><pre><span></span><span class="nl">android:</span><span class="n">networkSecurityConfig</span><span class="o">=</span><span class="s">&quot;@xml/network_security_config&quot;</span>
</pre></div>


<p>And create a file named network_security_config.xml under the res/xml folder which should contain:</p>
<div class="codehilite"><pre><span></span><span class="nt">&lt;network-security-config&gt;</span>    
    <span class="nt">&lt;base-config&gt;</span>  
      <span class="nt">&lt;trust-anchors&gt;</span>
          <span class="nt">&lt;certificates</span> <span class="na">src=</span><span class="s">&quot;system&quot;</span> <span class="nt">/&gt;</span>
          <span class="nt">&lt;certificates</span> <span class="na">src=</span><span class="s">&quot;user&quot;</span> <span class="nt">/&gt;</span>
      <span class="nt">&lt;/trust-anchors&gt;</span>
    <span class="nt">&lt;/base-config&gt;</span>
<span class="nt">&lt;/network-security-config&gt;</span>
</pre></div>


<p>With this, you should be set!</p>
<h2 id="common-errors">Common errors</h2>
<div class="warning"></div>

<blockquote>
<ul>
<li>Enable the debug logs if you have any doubt.</li>
<li>Check if TagCommander is called when you think it is. You should see it in the console logs.</li>
<li>Make sure you have the latest version.</li>
<li>Check if you are not stripping part of the module with ProGuard.</li>
</ul>
</blockquote>
<h2 id="common-errors-with-the-tagging-plan">Common errors with the tagging plan</h2>
<div class="warning"></div>

<blockquote>
<ul>
<li>Don't forget the # at the beginning and the end of each dynamic variable.</li>
<li>Always use the String type for the value of each dynamic variable.</li>
<li>Always use upper case dynamic variables.</li>
<li>The dynamic variables are case sensitive.</li>
<li>If you don't set the correct value for one dynamic variable, an empty string will replace it.</li>
</ul>
</blockquote>
<h1 id="helpers">Helpers</h1>
<h2 id="persisting-variables">Persisting variables</h2>
<p>The SDK module permits storing of variables that remain the same in the whole application, such as vendors ID, in a TagCommander instance, instead of passing them to the instance each time you want to send data.</p>
<p>These variables will have a lower priority to the one given by the addData method but will persist for the whole run of the application.</p>
<div class="codehilite"><pre><span></span><span class="n">TCInstance</span><span class="o">.</span><span class="na">addPermanentData</span><span class="o">(</span><span class="s">&quot;#VENDOR_ID#&quot;</span><span class="o">,</span> <span class="s">&quot;UE-55668779-01&quot;</span><span class="o">);</span>
</pre></div>


<p>They can also be removed if necessary.</p>
<div class="codehilite"><pre><span></span><span class="n">TCInstance</span><span class="o">.</span><span class="na">removePermanentData</span><span class="o">(</span><span class="s">&quot;#VENDOR_ID#&quot;</span><span class="o">);</span>
</pre></div>


<h2 id="tcpredefinedvariables">TCPredefinedVariables</h2>
<p>TagCommander collects a great deal of information to function with accuracy.
You can ask for any variables computed by TagCommander through a simple getData on TCPredefinedVariables.</p>
<p>The two following line are doing exactly the same thing, one using the constants declared in the SDK, the second using the name of the variable as defined in PredefinedVariables.xlsx. You can use either one.</p>
<div class="codehilite"><pre><span></span><span class="n">TCPredefinedVariables</span> <span class="n">predefVariables</span> <span class="o">=</span> <span class="n">TCPredefinedVariables</span><span class="o">.</span><span class="na">getInstance</span><span class="o">();</span>
<span class="n">String</span> <span class="n">curVisit</span> <span class="o">=</span> <span class="n">predefVariables</span><span class="o">.</span><span class="na">getData</span><span class="o">(</span><span class="n">TCConstants</span><span class="o">.</span><span class="na">kTCPredefinedVariable_CurrentVisitMs</span><span class="o">);</span>
<span class="n">String</span> <span class="n">curVisit</span> <span class="o">=</span> <span class="n">predefVariables</span><span class="o">.</span><span class="na">getData</span><span class="o">(</span><span class="s">&quot;#TC_CURRENT_VISIT_MS#&quot;</span><span class="o">);</span>
</pre></div>


<p>You can find a full list of variables computed by the SDK, explanations and examples here: </p>
<p><a href="PredefinedVariables.md">TCPredefinedVariables</a></p>
<h1 id="kotlin">Kotlin</h1>
<p>If you want to use Kotlin as your main language, there is absolutely nothing special to do.
Compile with the latest versions and call our SDK as usual.</p>
<h1 id="example-tcdemo">Example: TCDemo</h1>
<p>To check an example of how to use this module, please check: </p>
<p><a href="https://github.com/TagCommander/Tag-Demo/tree/master/Android">Tag Demo</a></p>
<h1 id="migration-from-v2-and-v3-to-v4">Migration from v2 and v3 to v4</h1>
<p>The way we are doing thing in v4 is quite different as we can only send data in post and to TagCommander's servers. But the code didn't change much and we can still use the old basic integration for the v4.</p>
<p>Remove all function not existing anymore like forceReload and the rest should still work.
To use new methods you can check in this document how to.</p>
<blockquote>
<p>You don't have to update your tagging plan in the application ! You should be able to keep your previous execute as is.
What needs to be changed is the container in your TagCommander interface, please check with your consultant.</p>
</blockquote>
<h1 id="support-and-contacts">Support and contacts</h1>
<p><img alt="alt tag" src="../res/ca_logo.png" /></p>
<hr />
<p><strong>Support</strong>
<em>support@commandersact.com</em></p>
<p>http://www.commandersact.com</p>
<p>Commanders Act | 3/5 rue Saint Georges - 75009 PARIS - France</p>
<hr />
<p>This documentation was generated on 25/03/2019 09:56:35</p>
</body>
</html>