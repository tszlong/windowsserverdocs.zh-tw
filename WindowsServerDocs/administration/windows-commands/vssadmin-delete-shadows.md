---
title: Vssadmin 刪除陰影
description: Vssadmin 刪除陰影命令的描述。
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 667aaa7477666c6128aaed4ddb10a9f3695e571a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89022862"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin 刪除陰影

> 適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

刪除指定磁片區的陰影複製。

## <a name="syntax"></a>語法

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---|---|
|/for =\<ForVolumeSpec>|指定將刪除的磁片區陰影複製。|
|/oldest|只刪除最舊的陰影複製。|
|/all|刪除所有指定磁片區的陰影複製。|
|/shadow =\<ShadowID>|刪除 ShadowID 所指定的陰影複製。 若要取得陰影複製識別碼，請使用 **vssadmin list shadows** 命令。 當您輸入陰影複製識別碼時，請使用下列格式，其中每個 *X* 都代表一個十六進位字元：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 的 < XXXX|
|/quiet|指定在執行時，命令不會顯示訊息。|

## <a name="remarks"></a>備註

您只能刪除具有用戶端可存取類型的陰影複製。

## <a name="examples"></a>範例

若要刪除磁片區 C 的最舊陰影複製，請輸入下列命令：

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他參考資料

* [命令列語法索引鍵](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin 清單陰影](vssadmin-list-shadows.md)
