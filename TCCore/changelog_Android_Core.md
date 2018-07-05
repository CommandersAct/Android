Changelog Android
=================

<div class="warning"></div>
>  If you want to check the previous version's changelog, you can find it here :

[Previous changelist](../res/changelog_Android_3.md)

*4.2.0 : 06/22 2018*

	+ Privacy notifications.

*4.1.9 : 05/25 2018*

    ~ Changed list types and synchronisation methods for the DynamicStores

*4.1.8 : 05/23 2018*

    ~ Default logging to nothing. Please call setDebugLevel only if you want logs.

*4.1.7 : 03/27 2018*

	~ Better synchronisation for the DynamicStores

*4.1.6 : 12/01 2017*

	+ You can now disable the SDK by calling disableSDK() nothing will be treated by the SDK after this.

*4.1.5 : 11/27 2017*

	+ Added Background Mode, a way to force the SDK to work when the application is in background.
	- Removed the ways to directly touch the DynamicStores used by the system classes.
	~ Better synchronisation for the DynamicStores

*4.1.4 : 11/10 2017*

	+ Better synchronisation around the DynamicStore to prevent issues with late AAID.

*4.1.3 : 08/03 2017*

	+ Added TC_SDK_ID and TC_NORMALIZED_ID

*4.1.2 : 05/02 2017*

	~ Updated modules' manifests to remove useless notions like application->allowrtl

*4.1.1 : 03/29 2017*

	~ Corrected a missing log in the TCCore module.

*4.1.0 : 02/06 2017*

	+ Beacon module release.

*4.0.2 : 01/10 2017*

	~ removed the dependencies in the POM generated for the JCenter deployement.

*4.0.1 : 12/14 2016*

	~ Updated CompileSDK, BuildTools, TargetSDK, appCompat and PlayServices versions.
	~ Corrected warnings in Lint report.

*4.0.0 : 12/12 2016*

	+ Added JCenter support
    + Separated all the core functions into a separated and reusable module.
    + Simplified TCDebug class
    - Removed the log output "file"
