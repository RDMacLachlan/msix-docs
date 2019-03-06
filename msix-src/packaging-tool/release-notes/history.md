---
title: MSIX Packaging Tool release notes
description: Full history of MSIX Packaging Tool release notes
author: c-don
ms.author: cdon
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10, uwp, msix, msix packaging tool, insider program
ms.localizationpriority: medium
ms.custom: Vibranium
---

# MSIX Packaging Tool Release Notes 

#### Ver 1.2019.304.0

New Features:

- User can specify valid expected exit codes for CLI conversions
- Empty folders generated during a conversion will persist through packaging
- Updated [AppID generation logic](#appid-generation-logic), and added additional validation for package name and app 

##### AppID generation logic
The new procedure to derive the App ID is as follows: 
1. Find the exe/msi name, and strip the extension
2. Convert to uppercase
3. Remove all non alpha-numeric characters
4. Translate the numerals to corresponding English words
5. If the resulting string contains less than 3 characters, use the string “APP” instead
6. If the resulting string contains more than 64 characters, truncate it.
7. For collisions, truncate the string if it contains more than 62 characters, and append a two-digit value, starting with 00.

#### Ver 1.2019.226.0
New Features:

- Ability to convert on a remote machine - more info
- Improved management experience in package editor
- Auto versioning recommendations when saving in package editor

Additional updates

- Added the ability to use “.” to progress the version field
- Fixed validation for installation location
- Fixed manifest translation issues for file type association handlers and com server entries
- Added the ability to get the status of your command line conversions
- Improved COM warning logging with human readable errors
