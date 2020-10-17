---
title: vssadmin list shadows
description: Vssadmin list shadow 命令的描述，此命令會列出指定磁片區的所有現有陰影複製。
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: cb7ddaa09e2da350c1c5422c6224aabd8831f763
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156201"
---
# <a name="vssadmin-list-shadows"></a>vssadmin list shadows

> 適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

列出指定磁片區的所有現有陰影複製。 如果您在不使用參數的情況下使用此命令，它會依 **陰影複製集**的順序，顯示電腦上的所有磁片區陰影複製。

## <a name="syntax"></a>語法

```
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /for =`<ForVolumeSpec>` | 指定將列出陰影複製的磁片區。 |
| /shadow =`<ShadowID>` | 列出 ShadowID 所指定的陰影複製。 若要取得陰影複製識別碼，請使用 [vssadmin list shadows 命令](vssadmin-list-shadows.md)。 當您輸入陰影複製識別碼時，請使用下列格式，其中每個 *X* 都代表一個十六進位字元：<p>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 的 < XXXX |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [vssadmin 命令](vssadmin.md)

- [vssadmin 清單遮蔽命令](vssadmin-list-shadows.md)
