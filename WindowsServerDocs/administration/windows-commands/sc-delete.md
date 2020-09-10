---
title: Sc.exe 刪除
description: 瞭解如何使用 sc.exe 公用程式取消註冊服務
ms.topic: reference
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8ce9eb203fd9db68629ce5412836eb694eb67162
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637103"
---
# <a name="scexe-delete"></a>Sc.exe 刪除

從登錄中刪除服務子機碼。 如果服務正在執行，或另一個進程有服務的開啟控制碼，則會將服務標示為要刪除。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
sc.exe [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如， \\ \\ myserver) 。 若要在本機執行 SC.exe，請省略此參數。|
|\<ServiceName>|指定 **getkeyname** 作業所傳回的服務名稱。|
|?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

不建議使用 sc.exe 刪除內建的作業系統服務，例如 DHCP、DNS 或 Internet Information Services。 若要安裝、移除或重新設定作業系統角色、服務和元件，請參閱 [安裝或卸載角色、角色服務或功能](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

## <a name="examples"></a>範例

若要從本機電腦上的登錄中刪除服務子機碼 **NewServ** ，請輸入：
```
sc.exe delete newserv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
