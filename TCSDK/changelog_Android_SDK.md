Changelog Android
=================

<div class="warning"></div>
>  If you want to check the previous version's changelog, you can find it here :

[Previous changelist](../res/changelog_Android_3.md)


*4.3.0 : 12/05 2018*

	- Moved TCUser_agent in Core
	- Moved TCNetworkManager and TCWaitingQueue which are now part of Core./Users/jeanjulien/Sources/Mobile/Docs/changelogs/changelog_iOS_SDK.md

*4.2.2 : 08/06 2018*

	~ Refactoring on consent hits to give them a proper route.

*4.2.1 : 08/01 2018*

	+ Protecting the addition of data in the datalayer when the SDK is disabled.

*4.2.0 : 06/22 2018*

	~ Cleaning on android > 15 which is now always true.

*4.1.5 : 01/15 2018*

	~ Forcing the calls to Commanders Act server-side to be https. Please read the documentation at "Network monitor" as this is impacted.

*4.1.4 : 12/01 2017*

	+ You can now disable the SDK by calling disableSDK() nothing will be treated by the SDK after this.

*4.1.3 : 11/27 2017*

	+ Added Background Mode, a way to force the SDK to work when the application is in background.
	- Removed the ways to directly touch the DynamicStores used by the system classes.
	~ Better synchronisation for the DynamicStores

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
