---
title: Vssadmin 清單陰影
description: Vssadmin 清單的描述會遮蔽命令。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082059"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin 清單陰影

>適用於： Windows 10、 Windows 8.1、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

列出所有現有的指定大量的陰影複製。 如果您使用此命令不含參數，它會顯示在使用**陰影複製設定**指定的順序的電腦上所有的磁碟區陰影複製。

## <a name="syntax"></a>語法

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>參數

|參數|說明|
|---|---|
|/ for = \ < ForVolumeSpec >|會指定將列的陰影複製的磁碟區。|
|/ 陰影 = \ < ShadowID >|列出 ShadowID 所指定的陰影複製。 若要取得的陰影複製識別碼，使用**vssadmin 清單陰影**命令。 當您輸入的陰影複製識別碼時，使用下列格式，其中每個*X*代表十六進位字元：<br><br>XXXXXXXX-是-是-是-XXXXXXXXXXXX|

## <a name="additional-references"></a>其他參考

* [命令列語法索引鍵](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)