
<html>
<body>
<p><img alt="alt tag" src="../res/logo.png" /></p>
<h1 id="segments-implementation-guide">Segment's Implementation Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>20/12/2016</em><br />
Release version : <em>4.0.1</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#segments-implementation-guide">Segment's Implementation Guide</a></li>
<li><a href="#introduction">Introduction</a></li>
<li><a href="#creating-segments">Creating Segments</a></li>
<li><a href="#getting-segments">Getting Segments</a><ul>
<li><a href="#fetching">Fetching</a></li>
<li><a href="#getting-the-notification">Getting the notification</a></li>
<li><a href="#asking-for-the-list">Asking for the List</a></li>
</ul>
</li>
<li><a href="#demo-application">Demo Application</a></li>
<li><a href="#support-and-contacts">Support and contacts</a></li>
</ul>
</div>
<h1 id="introduction">Introduction</h1>
<p>By using TagCommander's Engage product you will be able to store a lot of data and create segments of users. With those segments you can target precisely your offers to your customers or server personalized content in your application.</p>
<p>TCSegment is a small module especially made to get the segment of your user from within your application.</p>
<h1 id="creating-segments">Creating Segments</h1>
<p>For the creation of segments, please check the documentation of the Engage product.</p>
<h1 id="getting-segments">Getting Segments</h1>
<p>The module needs some information to be able to fetch segments. It will need your siteID, the context of your application and also your security token. Your siteID and token are provided by TagCommander, the application context need to be provided by your application.</p>
<p>For debugging purpose, we recommand the use of TCDebug which will help you seeing what's happening inside the modules.</p>
<div class="codehilite"><pre><span class="n">TCDebug</span><span class="o">.</span><span class="na">setDebugLevel</span><span class="o">(</span><span class="n">Log</span><span class="o">.</span><span class="na">VERBOSE</span><span class="o">);</span>
<span class="n">TCDebug</span><span class="o">.</span><span class="na">setNotificationLog</span><span class="o">(</span><span class="kc">true</span><span class="o">);</span>
<span class="n">TCSegment</span><span class="o">.</span><span class="na">getInstance</span><span class="o">().</span><span class="na">setSiteIDAppContextAndToken</span><span class="o">(</span><span class="mi">3311</span><span class="o">,</span> <span class="k">this</span><span class="o">.</span><span class="na">getApplicationContext</span><span class="o">(),</span> <span class="s">&quot;e2032376eca5533858b7d6616d40802be54d221db1b75e1b&quot;</span><span class="o">);</span>
</pre></div>


<div class="warning"></div>

<blockquote>
<p>We can't fetch Segments before the SDK fetched for the phone's Android Advertising ID, you might have to wait severals seconds after the initialisation of the Segment module.</p>
</blockquote>
<p>Since fetching segments needs internet and is not instantaneous, getting the segments require two steps. First you will ask the module to fetch the segmentation, then you will be able to get the list of segment once the first operation ended by either registering to a notification or by asking directly the segment list.</p>
<h2 id="fetching">Fetching</h2>
<p>To ask the module to fetch the segments, simply call the following line. Call it back each time you want to refresh the value.</p>
<div class="codehilite"><pre><span class="n">TCSegment</span><span class="o">.</span><span class="na">getInstance</span><span class="o">().</span><span class="na">fetchSegments</span><span class="o">();</span>
</pre></div>


<h2 id="getting-the-notification">Getting the notification</h2>
<p>The simplest way to have the segment list as soon as possible is by listening to the notification sent by the module. Simply use your application context to get the LocalBroadcastManager and register your class as a listener of kTCNotification_SegmentAvailable.</p>
<div class="codehilite"><pre><span class="n">LocalBroadcastManager</span> <span class="n">broadcastManager</span> <span class="o">=</span> <span class="n">LocalBroadcastManager</span><span class="o">.</span><span class="na">getInstance</span><span class="o">(</span><span class="n">main</span><span class="o">.</span><span class="na">getApplicationContext</span><span class="o">());</span>
<span class="n">broadcastManager</span><span class="o">.</span><span class="na">registerReceiver</span><span class="o">(</span><span class="k">this</span><span class="o">,</span> <span class="k">new</span> <span class="n">IntentFilter</span><span class="o">(</span><span class="n">TCSConstants</span><span class="o">.</span><span class="na">kTCNotification_SegmentAvailable</span><span class="o">));</span>
</pre></div>


<p>Then treat the notification in your onReceive method:</p>
<div class="codehilite"><pre><span class="nd">@Override</span>
<span class="kd">public</span> <span class="kt">void</span> <span class="nf">onReceive</span><span class="o">(</span><span class="n">Context</span> <span class="n">context</span><span class="o">,</span> <span class="n">Intent</span> <span class="n">intent</span><span class="o">)</span>
<span class="o">{</span>
    <span class="n">String</span> <span class="n">intentName</span> <span class="o">=</span> <span class="n">intent</span><span class="o">.</span><span class="na">getAction</span><span class="o">();</span>

    <span class="k">if</span> <span class="o">(</span><span class="n">intentName</span> <span class="o">!=</span> <span class="kc">null</span> <span class="o">&amp;&amp;</span> <span class="n">intentName</span><span class="o">.</span><span class="na">equals</span><span class="o">(</span><span class="n">TCSConstants</span><span class="o">.</span><span class="na">kTCNotification_SegmentAvailable</span><span class="o">))</span>
    <span class="o">{</span>
        <span class="c1">// Do anything you want here</span>
    <span class="o">}</span>
<span class="o">}</span>
</pre></div>


<h2 id="asking-for-the-list">Asking for the List</h2>
<p>If you don't want to or can't register to the notification, you can also simply call a method from the module that will give you the current list. Be carefull as it not synchroneous, the list may get updated after you asked for it.</p>
<div class="codehilite"><pre><span class="n">List</span><span class="o">&lt;</span><span class="n">String</span><span class="o">&gt;</span> <span class="n">segments</span> <span class="o">=</span> <span class="n">TCSegment</span><span class="o">.</span><span class="na">getInstance</span><span class="o">().</span><span class="na">getSegmentList</span><span class="o">();</span>
</pre></div>


<p>If no segment are found or they were never fetched, the list will be empty and not null.</p>
<h1 id="demo-application">Demo Application</h1>
<p>To check an example of how to use this module, please check: </p>
<p><a href="https://github.com/TagCommander/Segment-Demo/tree/master/Android">Segment Demo</a></p>
<h1 id="support-and-contacts">Support and contacts</h1>
<p><img alt="alt tag" src="../res/logo.png" /></p>
<hr />
<p><strong>Support</strong>
<em>support@tagcommander.com</em></p>
<p>http://www.tagcommander.com</p>
<p>TagCommander | 3/5 rue Saint Georges - 75009 PARIS - France</p>
<hr />
<p>This documentation was generated on 20/12/2016 14:40:37</p>
</body>
</html>