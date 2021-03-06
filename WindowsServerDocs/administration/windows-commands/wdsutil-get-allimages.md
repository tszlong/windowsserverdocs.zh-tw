---
title: wdsutil get-allimages
description: Wdsutil allimages 命令的參考文章，可取得伺服器上所有影像的相關資訊。
ms.topic: reference
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2fe93036c5493baed122fede4b25b71349996536
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614930"
---
# <a name="wdsutil-get-allimages"></a>wdsutil get-allimages

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取伺服器上所有影像的相關資訊。

## <a name="syntax"></a>語法

```
wdsutil /get-allimages [/server:<servername>] /show:{boot | install | legacyris | all} [/detailed]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[/server:<servername>]` | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| `/show:{boot | install | legacyris | all}` | 當 **開機** 只傳回開機映射時， **install** 會傳回安裝映射以及包含這些映射的映射群組的相關資訊， **LEGACYRIS** 只會傳回 (RIS) 映射的遠端安裝服務，並傳回開機映射資訊， (包括映射群組 **) 和 RIS** 映射資訊的相關資訊。 |
| [/detailed] | 表示應該傳回每個影像的所有影像中繼資料。 如果未使用此選項，預設行為是只傳回映射名稱、描述和檔案名。 |

## <a name="examples"></a>範例

若要查看影像的相關資訊，請輸入下列其中一項：

```
wdsutil /get-allimages /show:install
```

```
wdsutil /verbose /get-allimages /server:MyWDSServer /show:all /detailed
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-image 命令](wdsutil-add-image.md)

- [wdsutil 複製映射命令](wdsutil-copy-image.md)

- [wdsutil 匯出-image 命令](wdsutil-export-image.md)

- [wdsutil remove-image 命令](wdsutil-remove-image.md)

- [wdsutil 取代-image 命令](wdsutil-replace-image.md)

- [wdsutil 設定-image 命令](wdsutil-set-image.md)
