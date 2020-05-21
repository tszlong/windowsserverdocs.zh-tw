---
title: Vssadmin 清單陰影
description: Vssadmin list shadows 命令的描述。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 38eda178b6c9e34fec1d63ed6c59f01023b859c4
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436668"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin 清單陰影

> 適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

列出指定磁片區的所有現有陰影複製。 如果您在沒有參數的情況下使用此命令，它會依**陰影複製組所規定**的順序，顯示電腦上的所有磁片區陰影複製。

## <a name="syntax"></a>語法

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---|---|
|/for = \< ForVolumeSpec>|指定將為其列出陰影複製的磁片區。|
|/shadow = \< ShadowID>|列出 ShadowID 所指定的陰影複製。 若要取得陰影複製識別碼，請使用**vssadmin list shadows**命令。 當您輸入陰影複製識別碼時，請使用下列格式，其中每個*X*代表一個十六進位字元：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX （XXXX）|

## <a name="additional-references"></a>其他參考

* [命令列語法索引鍵](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)