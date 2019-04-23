---
title: List shadows
description: Vssadmin 清單的描述會遮蔽命令。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861149"
---
# <a name="vssadmin-list-shadows"></a>List shadows

>適用於：Windows 10，Windows 8.1，Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

列出所有現有的指定磁碟區的陰影複本。 如果您使用此命令不加任何參數，它會依照順序在電腦上顯示所有磁碟區陰影複製**陰影複製設定**。

## <a name="syntax"></a>語法

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---|---|
|/for=\<ForVolumeSpec>|指定將針對列出的陰影複製的磁碟區。|
|/shadow=\<ShadowID>|列出 ShadowID 所指定的陰影複製。 若要取得的陰影複製識別碼，請使用**list shadows**命令。 當您輸入陰影複製識別碼，使用下列格式，其中每個*X*代表十六進位字元：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>其他參考資料

* [命令列語法關鍵](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)