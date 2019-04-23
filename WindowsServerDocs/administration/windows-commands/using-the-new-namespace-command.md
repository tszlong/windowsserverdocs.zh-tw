---
title: 使用新命名空間命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50d51101afe95c99b7034fc50b3d30b799ee02ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871149"
---
# <a name="using-the-new-namespace-command"></a>使用新命名空間命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立並設定新的命名空間。 當您有只傳輸伺服器角色服務安裝時，您應該使用此選項。 如果您有部署伺服器角色服務和傳輸伺服器角色服務安裝 （這是預設值），請使用[使用新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)。 請注意，您必須先註冊內容提供者，才能使用此選項。
## <a name="syntax"></a>語法
```
wdsutil [Options] /New-Namespace [/Server:<Server name>]
     /FriendlyName:<Friendly name>
     [/Description:<Description>]
     /Namespace:<Namespace name>
     /ContentProvider:<Name>
     [/ConfigString:<Configuration string>]
     /Namespacetype: {AutoCast | ScheduledCast}
         [/time:<YYYY/MM/DD:hh:mm>]
         [/Clients:<Number of clients>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|/Friendlyname:<Friendly name>|指定的命名空間的易記名稱。|
|[/ 描述：<Description>]|設定命名空間的描述。|
|/ 命名空間：<Namespace name>|指定的命名空間名稱。 請注意，這不是易記的名稱，它必須是唯一的。<br /><br />-   **部署伺服器角色服務**:此選項的語法是 /Namespace:WDS:<Image group>/<Image name>/<Index>。 例如: **WDS:ImageGroup1/install.wim/1**<br />-   **傳輸伺服器角色服務**:這個值應該符合伺服器上建立命名空間時提供的名稱。|
|/ContentProvider:<Name>]|指定的命名空間會提供內容的內容提供者的名稱。|
|[/ConfigString:<Configuration string>]|指定內容的提供者的組態字串。|
|/Namespacetype: {AutoCast &#124; ScheduledCast}|指定傳輸設定。 您指定的設定，使用下列選項：<br /><br />-[/time: <time>]-設定傳輸應使用下列格式啟動的時間：YYYY/MM/y。 此選項僅適用於排程傳送 」 傳輸。<br />-[/ 用戶端： <Number of clients>]-設定用戶端傳輸啟動之前要等待的最小數目。 此選項僅適用於排程傳送 」 傳輸。|
## <a name="BKMK_examples"></a>範例
若要建立自動傳送命名空間，請輸入：
```
wdsutil /New-Namespace /FriendlyName:"Custom AutoCast Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
若要建立排程傳送命名空間，請輸入：
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:"Custom Scheduled Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:"2006/11/20:17:00" /Clients:20
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用 移除命名空間命令](using-the-remove-namespace-command.md)
 [子命令： 開始命名空間](subcommand-start-namespace.md)
