---
title: wdsutil get-allimagegroups
description: Wdsutil allimagegroups 命令的參考文章，此命令會抓取伺服器上所有映射群組的相關資訊，以及這些映射群組中的所有映射。
ms.topic: reference
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7209b0f806b0fd86167e8fbcf36f717a0d069195
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614860"
---
# <a name="wdsutil-get-allimagegroups"></a>wdsutil get-allimagegroups

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取伺服器上所有映射群組的相關資訊，以及這些映射群組中的所有映射。

## <a name="syntax"></a>語法

```
wdsutil [options] /get-allimagegroups [/server:<servername>] [/detailed]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[/server:<servername>]` | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| [/detailed] | 傳回每個影像的影像中繼資料。 如果未使用此參數，預設行為是只傳回每個影像的映射名稱、描述和檔案名。 |

## <a name="examples"></a>範例

若要查看映射群組的相關資訊，請輸入下列其中一項：

```
wdsutil /get-allimagegroups
```

```
wdsutil /verbose /get-allimagegroups /server:MyWDSServer /detailed
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-imagegroup 命令](wdsutil-add-imagegroup.md)

- [wdsutil remove-imagegroup 命令](wdsutil-remove-imagegroup.md)

- [wdsutil 設定-imagegroup 命令](wdsutil-set-imagegroup.md)
