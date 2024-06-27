---
title: "A simple unzip function in Powershell"
description: "2016-04-06"
date: "2016-04-06"
paige:
  feed:
    hide_page: true
categories: ["Code"]
weight: 1
---

```powershell
function UnzipFiles($ZipFileName, $DestDir)
{
    Add-Type -Assembly System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory($ZipFileName, $DestDir)
}
```

To use the function:

```powershell
UnzipFiles -ZipFileName .\folder\file.zip -DestDir .\destination
```

<br>
