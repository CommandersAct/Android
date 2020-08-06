Changelog iOS
=============

*4.5.1 : 08/07 2020*

    ~ Corrected IABTCF_gdprApplies which wasn't saved correctly.
    ~ Corrected TCData which didn't record properly legitimate interests

*4.5.0 : 07/30 2020*

	/!\ Requires Privacy 4.6.0+, Core 4.5.1+
	+ IAB's consentString is now in version 2.
	+ Saving information in TCData.

*4.3.2 : 10/31 2019*

    ~ Corrected the key IABConsent_SubjectToGDPR which was not saved as a string.

*4.3.1 : 10/24 2019*

	+ Added several new keys to the user defaults.
	+ IABConsent_SubjectToGDPR which defaults to "1"
	+ IABConsent_ParsedVendorConsents String of “0”s and “1”s, where the character at position N indicates the consent status to vendorID N as defined in the Global Vendor List. 
	+ IABConsent_ParsedPurposeConsents String of “0”s and “1”s, where the character at position N indicates the consent status to purposeID N as defined in the Global Vendor List.

*4.3.0 : 05/15 2019*

	+ Release of the first version of IAB consent string framework
