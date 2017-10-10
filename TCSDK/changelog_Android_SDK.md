Changelog Android
=================

<div class="warning"></div>
>  If you want to check the previous version's changelog, you can find it here :

[Previous changelist](../res/changelog_Android_3.md)

*4.1.2 : 10/10 2017*

	- removed Location permissions in the SDK manifest since it's not useful if you're not using TCLocation. You will need to add it in your application yourself if it's not already present.

*4.1.1 : 05/02 2017*

	~ Updated modules' manifests to remove useless notions like application->allowrtl

*4.1.0 : 02/06 2017*

	+ Beacon module release.

*4.0.2 : 01/10 2017*

	~ removed the dependencies in the POM generated for the JCenter deployement.

*4.0.1 : 12/14 2016*

	~ Updated CompileSDK, BuildTools, TargetSDK, appCompat and PlayServices versions.
	~ Corrected warnings in Lint report.

*4.0.0 : 12/12 2016*

    + Added JCenter support
    ~ Transformed the SDK from a light SDK to a "Server-side" only SDK. We will now only send information to our server to manage the reste of the treatment.
    - Removed functionalities for managing tags (Conditions and Exernal configuration)
