---
title: 使用 get 命名空間命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd11880b6e733850b522c3a06152ac7ce7a28841
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889659"
---
# <a name="using-the-get-namespace-command"></a>使用 get 命名空間命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示自訂命名空間的相關資訊。
## <a name="syntax"></a>語法
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/ 命名空間：<Namespace name>|指定的命名空間名稱。 請注意，這不是易記的名稱，它必須是唯一的。<br /><br />部署伺服器：命名空間名稱的語法是 /Namspace:WDS:<ImageGroup>/<ImageName>/<Index>。 例如: **WDS:ImageGroup1/install.wim/1**<br />-傳輸伺服器：這個值應該符合伺服器上建立時指定命名空間的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|[/ Show： 用戶端] 或 [/ 詳細資料： 用戶端]|顯示用戶端電腦連線到指定的命名空間的相關資訊。|
## <a name="BKMK_examples"></a>範例
若要檢視命名空間的相關資訊，請輸入：
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
若要檢視的命名空間和連接的用戶端的相關資訊，請輸入下列其中一項：
-   Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
-   Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用 新命名空間命令](using-the-new-namespace-command.md)
[使用移除命名空間命令](using-the-remove-namespace-command.md)
[子命令： 開始命名空間](subcommand-start-namespace.md)
