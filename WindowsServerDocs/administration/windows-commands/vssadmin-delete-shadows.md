---
title: Vssadmin 刪除陰影
description: Vssadmin 刪除陰影命令的描述。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847089"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin 刪除陰影

>適用於：Windows 10，Windows 8.1，Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

刪除指定磁碟區陰影複製。

## <a name="syntax"></a>語法

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---|---|
|/for=\<ForVolumeSpec>|指定要刪除的磁碟區陰影複製。|
|/oldest|刪除最舊陰影複製。|
|/all|刪除所有指定的磁碟區陰影複製。|
|/shadow=\<ShadowID>|刪除 ShadowID 所指定的陰影複製。 若要取得的陰影複製識別碼，請使用**list shadows**命令。 當您輸入的陰影複製識別碼時，使用下列格式，其中每個*X*代表十六進位字元：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|指定此命令不會顯示在執行時的訊息。|

## <a name="remarks"></a>備註

您只能刪除陰影複製用戶端可存取的類型。

## <a name="examples"></a>範例

若要刪除舊的陰影複製磁碟區 C，輸入下列命令：

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他參考資料

* [命令列語法關鍵](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [List shadows](vssadmin-list-shadows.md)