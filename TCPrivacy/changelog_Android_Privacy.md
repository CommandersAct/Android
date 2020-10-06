Changelog Android
=================

*4.6.6 : 10/06 2020*

	~ Correcting first IAB purpose which was saved as consent and legitimate interest. The first purpose can't have legitimate interest.
	~ Correcting landscape issue.

*4.6.5 : 09/24 2020*

	~ Corrected language issue displaying only English.

*4.6.4 : 09/21 2020*

	+ Adding methods to enter the IAB Privacy center directly on the purpose screen or on the vendor screen. Please check documentation.
	+ Functions used to accept or refuse all consent without entering the Privacy Center.

*4.6.3 : 09/15 2020*

	~ Corrected an issue in the parsing of the field used for button selection in IAB.
	- /!\ Removed TCIAB as a mandatory dependencies for this module.

*4.6.2 : 09/07 2020*

	+ Added a way to choose which buttons to have on first and second layer (IAB interface). Please check TCIAB documentation for further details.
	- Removed URL-encoding for the statistic hits parameters which was not needed anymore.

*4.6.1 : 08/27 2020*

	+ Disabling back button for IAB interface
	- Removing the save of the privacy when pressing the back button (when not disabled)
	+ New method in TCPrivacyAPI : TCShouldDisplayPrivacyCenter. /!\ Beware that if you need to use this method, you might need to re-display the first time.
	~ Corrected issue with accept all and Refuse all buttons staying disabled on tablets.

*4.6.0 : 07/30 2020*

	+ Interface based on IAB TCF v2 framework can now be used alongside TCIAB's module.

*4.5.6 : 06/22 2020*

	~ /!\ IAB v1 ONLY: Modified the saving of the consent string from single entry ranges to bitfield. This should make consent string much smaller when you have a lot of vendors.


*4.5.5 : 05/11 2020*

	~ Corrected TCPrivacyAPI getAcceptedCategories and getAcceptedVendors which tested presence in sharedPref and not values.


*4.5.4 : 04/30 2020*

	~ resetSavedConsent is now a public function as some clients might want to force it. Especially usefull with for testing or when Auto Backup is on.


*4.5.3 : 04/29 2020*

	~ Added margin inside the privacy center view. (still can't be customised for now)


*4.5.2 : 03/30 2020*

	~ Modified the IAB consentScreenID to 1 during consentString creation.
	~ Corrected the consent Time_created and Time_updated which were inverted.


*4.5.1 : 01/20 2020*

	+ Added a way to customise privacy title by adding PCM.putExtra(TCPrivacyConstants.kTCIntentExtraCustomTitle, "My Title");


*4.5.0 : 12/18 2019*

	+ Added an API class to check the content or status of the consent. Please check the TCPrivacy documentation on github.


*4.4.3 : 10/29 2019*

	+ All the switch positions of the Privacy Center can now default to the off position by using : TCPrivacy.getInstance().switchDefaultState = false;


*4.4.2 : 09/25 2019*

    ~ Correction on TCConfigurationFactory initialisation when not using CMP


*4.4.1 : 09/20 2019*

    ~ /!\ Function to initialize Privacy have changed
    ~ /!\ Update Core module alongside this module.
    ~ Refactoring on file configurations
    + We now have a class to manage distant configuration and privacy versions are taken from the configuration. 


*4.3.10 : 08/02 2019*

    ~ Only passing categories ID related IAB for Consent String generation.


*4.3.9 : 06/12 2019*

	~ Correction on subCategories switch issues.


*4.3.8 : 06/10 2019*

	~ Correction on back button behavior when a consent has already been given


*4.3.7 : 06/07 2019*

	+ Adding a way to prevent back button


*4.3.6 : 05/15 2019*

	+ Generating consent string for IAB version 1.


*4.3.5 : 04/11 2019*

	~ Privacy Policy: Don't take into account the /n into the link zone.


*4.3.4 : 03/25 2019*

    ~ Refactoring and better documentation for the ID used to save the privacy.


*4.3.3 : 03/19 2019*

	+ Callback if there is an important change in configuration update.


*4.3.2 : 01/22 2019*

    + Privacy policies can be added in the descriptions.
    + Better privacy metrics


*4.3.1 : 01/04 2019*

	~ Corrected callback not called when saving consent standalone.


*4.3.0 : 12/05 2018*

	+ Privacy Center to display the privacy inside the app.
	+ Managing offline configuration and checking for updates
	+ Possibility to use Privacy as a standalone module (with core, but withtout the SDK module).
	+ Possibility to give a global consent in the Privacy Center.
	~ Updated consent saving hits to use TCPID instead of the TCID.


*4.2.2 : 10/23 2018*

	+ Added callbacks for "updatedConsent" and "outdatedConsent"


*4.2.1 : 08/01 2018*

	+ You can call the "viewConsent" method to log it.
	~ Corrected the consent update hits that sometimes would be created after the disabling of the SDK.
	+ Propagating the validated categories to the hit that where waiting for consent.


*4.2.0 : 02/01 2018*

    + Give your user's privacy settings to the SDK.
