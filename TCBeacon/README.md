
<html>
<body>
<p><img alt="alt tag" src="../res/logo.png" /></p>
<h1 id="beacons-implementation-guide">Beacon's Implementation Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>09/02/2017</em><br />
Release version : <em>4.1.0</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#beacons-implementation-guide">Beacon's Implementation Guide</a></li>
<li><a href="#introduction">Introduction</a><ul>
<li><a href="#beacon-101">Beacon 101</a></li>
<li><a href="#types-of-beacons">Types of beacons</a></li>
<li><a href="#good-practice">Good practice</a></li>
<li><a href="#limitations">Limitations</a></li>
</ul>
</li>
<li><a href="#implementation">Implementation</a><ul>
<li><a href="#dependencies">Dependencies</a></li>
</ul>
</li>
<li><a href="#scanning-for-beacons">Scanning for beacons</a><ul>
<li><a href="#scan-settings">Scan settings</a></li>
<li><a href="#background-scanning">Background scanning</a></li>
<li><a href="#tcdemo">TCDemo</a></li>
</ul>
</li>
<li><a href="#support-and-contacts">Support and contacts</a></li>
</ul>
</div>
<h1 id="introduction">Introduction</h1>
<p>The Beacon module helps you scan your surrounding and parse the data from the beacons found.</p>
<h2 id="beacon-101">Beacon 101</h2>
<p>Beacons are devices that use the Bluetooth Low Energy (BLE) protocol to repeatedly send short transmissions. Those short transmissions depends on the type of Beacon, basic iBeacons and EddyStone UUID beacons only sends a simple UUID, while Eddystone's URL type sends a complete URL as it's name states.</p>
<p>It allows us to discover when a user is physically close to one of your beacon.</p>
<h2 id="types-of-beacons">Types of beacons</h2>
<p>Here is the list of the types of beacons currently supported by Commanders Act's module:</p>
<ul>
<li>iBeacons : Which broadcast an UUID with a Major and a Minor.</li>
<li>Eddystone : Exist with 4 types of frames, UUID</li>
</ul>
<p>When you use a beacon, it is recommended that you only access information from your network of beacons. To do that the scan are filtered by an ID (beside with Eddystone's URL beacons), on iBeacon we use the full UUID of the beacon, on Eddystone we use the first part of it (called the namespace).</p>
<p>As an exemple for our demo application we filter on "397180b5-be24-4090-8af5-8237f4e17248" when we are dealing with our iBeacons and on "017180B5-BE24-4090-8AF5" for our Eddystone ones.</p>
<p><a href="https://github.com/google/eddystone/blob/master/protocol-specification.md">If you want specific technical documentation on Eddystone's protocol, please check this link</a></p>
<h2 id="good-practice">Good practice</h2>
<p>Scanning in itself doesn't use too much of your battery (but still use some of course). It is <em>recommended</em> that you ask the user to opt-in for beacon scanning and also explain how it will be beneficial for him. As bluetooth must be enabled, explaining everything to him will also push him to activate the bluetooth more often.</p>
<h2 id="limitations">Limitations</h2>
<p>Right now, we only allow to scan and filter on one UUID. It is the classic implementation to rely on one UUID and have major/minors depending on your shops and where your beacons are placed. (or one namespace and several instances).
If you need a specific implementation or information, please contact support.</p>
<h1 id="implementation">Implementation</h1>
<p><a href="../README.md">Please check the Developers Implementation Guide to choose the best way to implement this module in your project.</a></p>
<h2 id="dependencies">Dependencies</h2>
<p><em>The Beacon module can only be used on phones using android Lollipop or higher.</em></p>
<p>To use the Beacon module, you need some permissions. If you are using JCenter or our .aar files they are defined in our manifests already, but if you use the jar files, you will need to add them yourself in your manifest.</p>
<div class="codehilite"><pre><span class="o">&lt;</span><span class="n">uses</span><span class="o">-</span><span class="n">permission</span> <span class="ss">android</span><span class="p">:</span><span class="nb">name</span><span class="o">=</span><span class="s2">&quot;android.permission.BLUETOOTH_ADMIN&quot;</span><span class="o">/&gt;</span>
<span class="o">&lt;</span><span class="n">uses</span><span class="o">-</span><span class="n">permission</span> <span class="ss">android</span><span class="p">:</span><span class="nb">name</span><span class="o">=</span><span class="s2">&quot;android.permission.BLUETOOTH&quot;</span><span class="o">/&gt;</span>
<span class="o">&lt;</span><span class="n">uses</span><span class="o">-</span><span class="n">permission</span> <span class="ss">android</span><span class="p">:</span><span class="nb">name</span><span class="o">=</span><span class="s2">&quot;android.permission.ACCESS_COARSE_LOCATION&quot;</span> <span class="sr">/&gt;</span>
<span class="sr">&lt;uses-permission android:name=&quot;android.permission.ACCESS_FINE_LOCATION&quot; /</span><span class="o">&gt;</span>
</pre></div>


<p>As say in the introduction, we recommend that you ask the user for the permission to enable the bluetooth on her device, our Beacon module won't start manually otherwise. The best way is to way for the bluetooth to be activated before you instanciate our module.</p>
<h1 id="scanning-for-beacons">Scanning for beacons</h1>
<p>Scanning for beacon is pretty easy, tell us what we need to scan for, and we'll give you updates.</p>
<p>The first thing you want to do is to listen to the events we will send you when we found, update or lost a beacon.</p>
<p>You can use an helper class we created for notifications or do it all by yourself. The following lines show you how to do it and give you the names of the notifications.</p>
<div class="codehilite"><pre><span class="n">LocalBroadcastManager</span> <span class="n">lbm</span> <span class="o">=</span> <span class="n">LocalBroadcastManager</span><span class="o">.</span><span class="na">getInstance</span><span class="o">(</span><span class="n">getApplicationContext</span><span class="o">());</span>
<span class="n">lbm</span><span class="o">.</span><span class="na">registerReceiver</span><span class="o">(</span><span class="n">mReceiver</span><span class="o">,</span> <span class="k">new</span> <span class="n">IntentFilter</span><span class="o">(</span><span class="n">TCBeaconConstant</span><span class="o">.</span><span class="na">kTCNotification_BeaconFound</span><span class="o">));</span>
<span class="n">lbm</span><span class="o">.</span><span class="na">registerReceiver</span><span class="o">(</span><span class="n">mReceiver</span><span class="o">,</span> <span class="k">new</span> <span class="n">IntentFilter</span><span class="o">(</span><span class="n">TCBeaconConstant</span><span class="o">.</span><span class="na">kTCNotification_BeaconUpdate</span><span class="o">));</span>
<span class="n">lbm</span><span class="o">.</span><span class="na">registerReceiver</span><span class="o">(</span><span class="n">mReceiver</span><span class="o">,</span> <span class="k">new</span> <span class="n">IntentFilter</span><span class="o">(</span><span class="n">TCBeaconConstant</span><span class="o">.</span><span class="na">kTCNotification_BeaconLost</span><span class="o">));</span>
</pre></div>


<p>When you have your user consent to scan for beacons, you can call the following code to scan for an Eddystone beacon:</p>
<div class="codehilite"><pre><span class="n">TCDebug</span><span class="o">.</span><span class="na">setDebugLevel</span><span class="o">(</span><span class="n">Log</span><span class="o">.</span><span class="na">VERBOSE</span><span class="o">);</span>
<span class="n">TCBeacon</span> <span class="n">tcBeacon</span> <span class="o">=</span> <span class="n">TCBeacon</span><span class="o">.</span><span class="na">getInstance</span><span class="o">(</span><span class="kc">null</span><span class="o">,</span> <span class="s">&quot;017180B5-BE24-4090-8AF5&quot;</span><span class="o">,</span> <span class="kc">null</span><span class="o">,</span> <span class="k">this</span><span class="o">);</span>
</pre></div>


<p>Or if you want to scann for an iBeacon:</p>
<div class="codehilite"><pre><span class="n">TCBeacon</span> <span class="n">tcBeacon</span> <span class="o">=</span> <span class="n">TCBeacon</span><span class="o">.</span><span class="na">getInstance</span><span class="o">(</span><span class="s">&quot;397180B5-BE24-4090-8AF5-8237F4E17248&quot;</span><span class="o">,</span> <span class="kc">null</span><span class="o">,</span> <span class="kc">null</span><span class="o">,</span> <span class="k">this</span><span class="o">);</span>
</pre></div>


<p>If you only want to scan URL type beacons, no need to pass the module any UUID at all.</p>
<p>And then create the delegate method for the notifications:</p>
<div class="codehilite"><pre><span class="n">BroadcastReceiver</span> <span class="n">mReceiver</span> <span class="o">=</span> <span class="k">new</span> <span class="n">BroadcastReceiver</span><span class="o">()</span>
<span class="o">{</span>
    <span class="nd">@Override</span>
    <span class="kd">public</span> <span class="kt">void</span> <span class="nf">onReceive</span><span class="o">(</span><span class="n">Context</span> <span class="n">context</span><span class="o">,</span> <span class="n">Intent</span> <span class="n">intent</span><span class="o">)</span>
    <span class="o">{</span>
        <span class="n">String</span> <span class="n">action</span> <span class="o">=</span> <span class="n">intent</span><span class="o">.</span><span class="na">getAction</span><span class="o">();</span>
        <span class="n">TCBaseBeacon</span> <span class="n">beacon</span><span class="o">;</span>

        <span class="c1">// we get the beacon (beacon is extends to Parcelable)</span>
        <span class="n">beacon</span> <span class="o">=</span> <span class="n">intent</span><span class="o">.</span><span class="na">getParcelableExtra</span><span class="o">(</span><span class="n">TCBeaconConstant</span><span class="o">.</span><span class="na">kTCUserInfo_BeaconObject</span><span class="o">);</span>

        <span class="k">if</span> <span class="o">(</span><span class="n">action</span><span class="o">.</span><span class="na">equals</span><span class="o">(</span><span class="n">TCBeaconConstant</span><span class="o">.</span><span class="na">kTCNotification_BeaconFound</span><span class="o">))</span>
        <span class="o">{</span>
            <span class="o">...</span>
        <span class="o">}</span>
        <span class="k">if</span> <span class="o">(</span><span class="n">action</span><span class="o">.</span><span class="na">equals</span><span class="o">(</span><span class="n">TCBeaconConstant</span><span class="o">.</span><span class="na">kTCNotification_BeaconUpdate</span><span class="o">))</span>
        <span class="o">{</span>
            <span class="o">...</span>
        <span class="o">}</span>
        <span class="k">if</span> <span class="o">(</span><span class="n">action</span><span class="o">.</span><span class="na">equals</span><span class="o">(</span><span class="n">TCBeaconConstant</span><span class="o">.</span><span class="na">kTCNotification_BeaconLost</span><span class="o">))</span>
        <span class="o">{</span>
            <span class="o">...</span>
        <span class="o">}</span>
    <span class="o">}</span>
<span class="o">}</span>
</pre></div>


<h2 id="scan-settings">Scan settings</h2>
<p>The third parameter of TCBeacon.getInstance is a custom scan settings, if passed to us, we will use those 3 parameters to override ours :</p>
<div class="codehilite"><pre>- CallbackType
- ScanMode
- ReportDelayMillis
</pre></div>


<h2 id="background-scanning">Background scanning</h2>
<p>To be able to use the Beacon while the application is in background we created a service. You will thus need to add those lines in your manifest if you are using directly the jar files instead of our JCenter release or aar release.</p>
<div class="codehilite"><pre><span class="o">&lt;</span><span class="n">service</span>
    <span class="ss">android</span><span class="p">:</span><span class="nb">name</span><span class="o">=</span><span class="s2">&quot;.TCBeaconScanService&quot;</span>
    <span class="ss">android</span><span class="p">:</span><span class="n">label</span><span class="o">=</span><span class="s2">&quot;TCBeaconScanService&quot;</span> <span class="o">&gt;</span>
<span class="o">&lt;</span><span class="sr">/service&gt;</span>
</pre></div>


<h2 id="tcdemo">TCDemo</h2>
<p>You can of course check our demo project for a simple implementation example.</p>
<p><a href="https://github.com/TagCommander/Beacon-Demo/tree/master/Android">Beacon Demo</a></p>
<h1 id="support-and-contacts">Support and contacts</h1>
<p><img alt="alt tag" src="../res/logo.png" /></p>
<hr />
<p><strong>Support</strong>
<em>support@tagcommander.com</em></p>
<p>http://www.tagcommander.com</p>
<p>TagCommander | 3/5 rue Saint Georges - 75009 PARIS - France</p>
<hr />
<p>This documentation was generated on 09/02/2017 16:25:43</p>
</body>
</html>