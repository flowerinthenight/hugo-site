---
title: "A simple zip function in Powershell"
description: "2016-04-05"
date: "2016-04-05"
paige:
  feed:
    hide_page: true
categories: ["Code"]
weight: 1
---

```powershell
function ZipFiles($ZipFileName, $SourceDir)
{
    Add-Type -Assembly System.IO.Compression.FileSystem
    $compressionLevel = [System.IO.Compression.CompressionLevel]::Optimal
    [System.IO.Compression.ZipFile]::CreateFromDirectory($SourceDir, $ZipFileName, $compressionLevel, $false)
}
```

To use the function:

```powershell
ZipFiles -ZipFileName test.zip -SourceDir .\folder\to\zip
```

<br>
