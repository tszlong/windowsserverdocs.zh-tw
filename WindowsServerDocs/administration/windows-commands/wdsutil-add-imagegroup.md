---
title: wdsutil add-imagegroup
description: Wdsutil add-imagegroup 命令的參考文章，可將映射群組新增至 Windows 部署服務伺服器。
ms.topic: reference
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3bffb562ba019bb55c783541b78c906dd4a08dc7
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524463"
---
# <a name="wdsutil-add-imagegroup"></a>wdsutil add-imagegroup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將映射群組新增至 Windows 部署服務伺服器。

## <a name="syntax"></a>語法

```
wdsutil [Options] /add-ImageGroup imageGroup:<Imagegroupname> [/Server:<Server name>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| imageGroup： `<Imagegroupname>` ] | 指定要新增之映射的名稱。 |
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |

## <a name="examples"></a>範例

若要新增映射群組，請輸入下列其中一項：

```
wdsutil /add-ImageGroup imageGroup:ImageGroup2
```

```
wdsutil /verbose /add-Imagegroup imageGroup:My Image Group /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil get-allimagegroups 命令](wdsutil-get-allimagegroups.md)

- [wdsutil get-imagegroup 命令](wdsutil-get-imagegroup.md)

- [wdsutil remove-imagegroup 命令](wdsutil-remove-imagegroup.md)

- [wdsutil 設定-imagegroup 命令](wdsutil-set-imagegroup.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
