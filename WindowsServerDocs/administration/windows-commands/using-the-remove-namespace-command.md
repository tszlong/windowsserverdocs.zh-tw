---
title: 使用移除命名空間命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115c0a90a60e18ee4b89758200773d1dfec2163f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842039"
---
# <a name="using-the-remove-namespace-command"></a>使用移除命名空間命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

移除自訂命名空間。
## <a name="syntax"></a>語法
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/ 命名空間：<Namespace name>|指定的命名空間名稱。 這不是易記的名稱，而且必須是唯一。<br /><br />-   **部署伺服器角色服務**:命名空間名稱的語法是 /Namespace:WDS:<ImageGroup>/<ImageName>/<Index>。 例如: **WDS:ImageGroup1/install.wim/1**<br />-   **傳輸伺服器角色服務**:此值必須符合伺服器上建立時指定命名空間的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|[/force]|立即移除命名空間，並終止所有用戶端。 請注意，除非您指定 **/force**，現有的用戶端可以完成傳輸，但不能加入了新的用戶端。|
## <a name="BKMK_examples"></a>範例
若要停止的命名空間 （目前的用戶端可以完成移轉但不能加入了新的用戶端），型別：
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
若要強制終止所有用戶端，請輸入：
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用 新命名空間命令](using-the-new-namespace-command.md)
 [子命令： 開始命名空間](subcommand-start-namespace.md)
