# QuietSwich

QuietSwitch is a portable Windows privacy app for turning off selected AI features, managing what Windows shares with Microsoft, and blocking known ad and tracking domains locally. It does not install a service, require a sign-in, or send analytics.

Getting started

1.	Extract the ZIP and open QuietSwitch.exe. Use your normal Windows account; the app will ask Windows for administrator approval only when a change affects the whole PC.
2.	Pick AI controls, Files & cloud, Diagnostics & ads, or Ads & tracking. Select Apply or Enable to see what will change before confirming. Guide or ? provides instructions you can follow manually.
3.	Use Undo on a change made by QuietSwitch to restore the previous policy. Reopen the affected app or restart Windows if the change has not taken effect.

The executable is built for 64-bit x86 Windows (x64). The app targets Windows 11 and Windows 10 22H2. The ad filter also works on eligible Home systems with a standard, writable hosts file and administrator approval. Other automated controls use published policies on eligible Pro, Enterprise, Education, and IoT Enterprise builds; Windows Home gets instructions and Settings shortcuts for those settings. QuietSwitch is unsigned, so Windows may show an unknown-publisher prompt. The source is included for review and rebuilding.

If upgrading from 1.0, close the old app first. The 1.1 executable can recognize and undo the device and account policies saved by 1.0.

What you can change

Area	Apply and Undo on supported editions/builds	Manual guides
AI controls	Stop Recall snapshots; disable Cocreator, Image Creator, and Generative Fill in Paint	Uninstall the standalone Microsoft Copilot app; adjust Copilot in Edge
Files & cloud	Stop OneDrive sync; disable cross-device Windows clipboard sync; exclude OneDrive and SharePoint account results from Windows Search	Adjust OneDrive folder backup or unlink; review Microsoft 365 connected experiences
Diagnostics & ads	Limit Windows diagnostics to required data, preserving an existing stricter value; disable the advertising ID, diagnostic personalization, or Windows Error Reporting	Open the relevant Windows Settings pages
Ads & tracking	Enable or disable local ad and tracker domain filtering; hide Windows Search highlights; suppress Windows feedback prompts	Review browser content-blocking options

Before disabling OneDrive: Download any cloud-only files you need and verify a copy in a folder outside OneDrive. The policy does not move files, delete existing cloud copies, or revoke links. Steps for downloading and stopping backup are in Microsoft's OneDrive backup guide and Microsoft's OneDrive unlinking guide.

Before disabling Recall snapshots: Windows can delete existing snapshots when this policy takes effect. Undo cannot recover them. On eligible Copilot+ PCs you can also choose whether to save snapshots in Windows Settings, as described in Microsoft's Recall privacy guide.

Ad and tracker blocking

Go to Ads & tracking, select Enable, and approve the device-wide change. QuietSwitch adds a marked section to the Windows hosts file using a bundled AdAway blocklist snapshot. It does not change your DNS provider, install a browser extension, or inspect traffic. Windows loads local hosts mappings into its resolver cache; QuietSwitch asks Windows to flush that cache after a change. Reopen your browser or affected app if it still uses an old result.

This is exact-domain blocking for apps that respect Windows hosts mappings. It may miss ads served from the same domain as the content, video or in-app ads, or requests that use a separate resolver. It does not hide empty ad boxes. If a page or app stops working, select Disable and reopen the affected app; for more fine-grained filtering see the browser guide in the app.

QuietSwitch skips pre-existing mappings and removes only its own section. If another tool edits that section, the app refuses to overwrite it. If an interrupted change leaves a hosts.QuietSwitch-*.bak beside the hosts file, have an administrator review the backup and current file before restoring anything. License and attribution details are in AdAway-Attribution.md.

Status and limits

•	Policy set: QuietSwitch wrote and read back the expected registry values. This does not verify that a service is installed, that the Windows feature has picked up the policy yet, or that network traffic has stopped.
•	Already set: The expected policy was present before QuietSwitch changed anything. There is nothing for QuietSwitch to undo.
•	Review or Check policy: The value changed, could not be safely read, or the backup needs attention. QuietSwitch will not overwrite an unexpected value during Undo.
•	Manual only: Use the instructions in the app. App-, browser-, or account-level changes done manually are not backed up by QuietSwitch.
•	Filter on/off: The managed hosts section is present or absent. This does not verify how individual apps resolve names.

The app does not disable Microsoft Defender, SmartScreen, the firewall, or Windows Update. It does not block all Microsoft traffic or turn off every AI feature. Required Windows diagnostics can still be sent, and Microsoft's apps and websites have separate privacy controls. On a work or school PC, check with IT first; organizational policies can take precedence or replace local values.

Policy references

•	Windows AI: Recall and Paint policy mappings
•	Uninstalling the standalone Copilot app
•	Copilot browsing context in Microsoft Edge
•	Windows System policies: OneDrive and diagnostic level
•	Windows Privacy policies: clipboard and advertising ID
•	Windows Search policies: cloud account content
•	Windows Search highlights policy
•	Windows Experience policies: diagnostic personalization and feedback notifications
•	Windows Error Reporting policy
•	Microsoft 365 connected experiences

Source and verification

From the Source folder, with GNU Make, Node.js, and an x64 MinGW-w64 C++17 cross-compiler installed, run make to create build/QuietSwitch.exe. Run make test with a host C++17 compiler to check supported builds, policy rollback, and hosts-file conflict handling. Run make test-win to build Windows API smoke tests that use a disposable HKCU test key and a temporary hosts fixture. They do not change real Windows policies or the actual hosts file.