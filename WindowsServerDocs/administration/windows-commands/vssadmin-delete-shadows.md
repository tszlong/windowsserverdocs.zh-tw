---
title: Vssadmin 刪除陰影
description: Vssadmin delete shadows 命令的描述。
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 884407cee82926b3b258afba5ab2e47dc640e10f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891807"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin 刪除陰影

> 適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

刪除指定的磁片區陰影複製。

## <a name="syntax"></a>語法

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---|---|
|/for =\<ForVolumeSpec>|指定將刪除哪一個磁片區的陰影複製。|
|/oldest|只刪除最舊的陰影複製。|
|/all|刪除指定磁片區的所有陰影複製。|
|/shadow =\<ShadowID>|刪除 ShadowID 所指定的陰影複製。 若要取得陰影複製識別碼，請使用**vssadmin list shadows**命令。 當您輸入陰影複製識別碼時，請使用下列格式，其中每個*X*代表一個十六進位字元：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX （XXXX）|
|/quiet|指定命令在執行時不會顯示訊息。|

## <a name="remarks"></a>備註

您只能使用用戶端可存取的類型來刪除陰影複製。

## <a name="examples"></a>範例

若要刪除磁片區 C 最舊的陰影複製，請輸入下列命令：

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他參考資料

* [命令列語法索引鍵](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin 清單陰影](vssadmin-list-shadows.md)
