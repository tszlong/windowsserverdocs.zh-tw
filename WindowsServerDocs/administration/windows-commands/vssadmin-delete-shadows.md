---
title: Vssadmin 刪除陰影
description: Vssadmin 刪除陰影命令的說明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081968"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin 刪除陰影

>適用於： Windows 10、 Windows 8.1、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

會刪除指定磁碟區陰影複製。

## <a name="syntax"></a>語法

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>參數

|參數|說明|
|---|---|
|/ for = \ < ForVolumeSpec >|會指定將刪除的磁碟區陰影複製。|
|最早 /|會刪除最舊陰影複製。|
|/all|會刪除所有指定的磁碟區陰影複製。|
|/ 陰影 = \ < ShadowID >|會刪除指定 ShadowID 陰影複製。 若要取得的陰影複製識別碼，使用**vssadmin 清單陰影**命令。 當您輸入的陰影複製識別碼時，使用下列格式，其中每個*X*代表十六進位字元：<br><br>XXXXXXXX-是-是-是-XXXXXXXXXXXX|
|/ 安靜|會指定命令不會顯示訊息時執行。|

## <a name="remarks"></a>備註

您只可以刪除陰影複製與用戶端存取類型。

## <a name="examples"></a>範例

若要刪除舊的陰影複製的大量 C，輸入此命令：

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他參考

* [命令列語法索引鍵](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin 清單陰影](vssadmin-list-shadows.md)