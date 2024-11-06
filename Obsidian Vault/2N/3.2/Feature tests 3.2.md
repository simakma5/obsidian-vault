Procedure mostly follows the general description charted-out in [[Feature testing]].
# [HIPSU-9292](https://dev.2n.cz/browse/HIPSU-9292): ACOM installation naming
## Site name in emails
 * All emails sent by ACOM excluding emails with company templates will have:
	 - site name in the header
	 - app name in sender (From: Access Commander \<from@example.com\>)
		 - Omitted as part of MVP implementation for 3.2
	 - site name in the subject
	 - a link to the installation in the footer (if the Base address for e-mail links is set)
 * Test in all major email clients:
	 - light/dark themes
	 - Outlook, Gmail, Seznam, OWA, ...
	 - mobile phone clients
 * Test at least one notification e-mail and one system e-mail (e.g. password reset)
## Site name settings
 * New card in settings/configuration settings above email settings.
 - Title: Installation name
 - Contains:
	 - Name (string)
		 - max 30 chars
		 - default value: “Access Commander”
		 - non-empty (validation)
	 - Revert button in triple-dot menu
 - Default values are the same for existing and new installations
## Found issues
- [x] Email template in companies includes installation name with a bug in the footer.
- [x] HIPSU-9624: Impossible to enter an empty/whitespace site name
      Functionality changed
# [HIPSU-9794](https://dev.2n.cz/browse/HIPSU-9794): Password-encrypted device backups
- The "Back up now" button becomes a menu button with 2 options:
	1. Back up to local storage: No dialog will appear. It will be saved directly to the card as the latest successful backup which can then be downloaded from the card.
	2. Back up to a file: Opens a modal window with an option to "Encrypt backup file" with a password prompt when checked, and a warning which encourages the use of a password to protect sensitive data in the config backup.
		- For devices with FW version lower than 2.45, the "Encrypt backup file" checkbox will not be visible and the alert will be similar to "download latest" button, but also mentioning that newer FW can encrypt backups.
- The restore button modal window on device detail page now has three options, "from a file" being the new addition. This option includes a file picker with an optional password field regardless of device FW.
	- ~~Potential errors (incorrect password, etc.) will show as validation errors in the modal window.~~