---
title: vssadmin delete shadows
description: Vssadmin 刪除 shadow 命令的描述，此命令會刪除指定的磁片區陰影複製。
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2dcd1bf8cbe946c77d0b5a100d2a29ecb36ef50f
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156220"
---
# <a name="vssadmin-delete-shadows"></a>vssadmin delete shadows

> 適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

刪除指定磁片區的陰影複製。 您只能刪除具有 *用戶端可存取* 類型的陰影複製。

## <a name="syntax"></a>語法

```
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /for =`<ForVolumeSpec>` | 指定將刪除的磁片區陰影複製。 |
| /oldest | 只刪除最舊的陰影複製。 |
| /all | 刪除所有指定磁片區的陰影複製。 |
| /shadow =`<ShadowID>` | 刪除 ShadowID 所指定的陰影複製。 若要取得陰影複製識別碼，請使用 [vssadmin list shadows 命令](vssadmin-list-shadows.md)。 當您輸入陰影複製識別碼時，請使用下列格式，其中每個 *X* 都代表一個十六進位字元：<p>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 的 < XXXX |
| /quiet | 指定在執行時，命令不會顯示訊息。 |

## <a name="examples"></a>範例

若要刪除磁片區 C 的最舊陰影複製，請輸入：

```
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [vssadmin 命令](vssadmin.md)

- [vssadmin 清單遮蔽命令](vssadmin-list-shadows.md)
