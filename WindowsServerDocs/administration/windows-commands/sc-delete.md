---
title: sc.exe 刪除
description: sc.exe delete 命令的參考文章，此命令會從登錄中刪除服務子機碼。
ms.topic: reference
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 12c09bd72259a8e4b58f134ca19f1fc825f2f1bb
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388459"
---
# <a name="scexe-delete"></a>sc.exe 刪除

從登錄中刪除服務子機碼。 如果服務正在執行，或另一個進程有服務的開啟控制碼，則會將服務標示為要刪除。

> [!NOTE]
> 我們不建議您使用此命令來刪除內建的作業系統服務，例如 DHCP、DNS 或 Internet Information Services。 若要安裝、移除或重新設定作業系統角色、服務和元件，請參閱 [安裝或卸載角色、角色服務或功能](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

## <a name="syntax"></a>語法

```
sc.exe [<servername>] delete [<servicename>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<servername>` | 指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如， \\ myserver) 。 若要在本機執行 SC.exe，請不要使用此參數。 |
| `<servicename>` | 指定 **getkeyname** 作業所傳回的服務名稱。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要從本機電腦上的登錄中刪除服務子機碼 **NewServ** ，請輸入：

```
sc.exe delete NewServ
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
