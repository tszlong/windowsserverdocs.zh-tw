---
title: wdsutil get-alldrivergroups
description: Wdsutil alldrivergroups 命令的參考文章，此命令會顯示伺服器上所有驅動程式群組的相關資訊。
ms.topic: reference
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b6d407e91af9132d6d2bc87ccb86320e3fe42b76
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614940"
---
# <a name="wdsutil-get-alldrivergroups"></a>wdsutil get-alldrivergroups

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有驅動程式群組的相關資訊。

## <a name="syntax"></a>語法

```
wdsutil /get-alldrivergroups [/server:<servername>] [/show:{packagemetadata | filters | all}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[/server:<servername>]` | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| `/show:{packagemetadata | filters | all}]` | 顯示指定群組中所有驅動程式套件的中繼資料。 **PackageMetaData** 會顯示驅動程式群組所有篩選器的相關資訊。 **篩選器** 會顯示群組之所有驅動程式套件和篩選器的中繼資料。 |

## <a name="examples"></a>範例

若要查看驅動程式檔案的相關資訊，請輸入下列其中一項：

```
wdsutil /get-alldrivergroups /server:MyWdsServer /show:All
```

```
wdsutil /get-alldrivergroups [/show:packagemetadata]
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-
- [wdsutil get-drivergroup 命令](wdsutil-get-drivergroup.md)
