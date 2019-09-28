---
title: 使用新的-Namespace 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1df6634bc7598701db050f3d240e41dbb6f06019
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363013"
---
# <a name="using-the-new-namespace-command"></a>使用新的-Namespace 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立並設定新的命名空間。 當您只安裝傳輸伺服器角色服務時，應該使用此選項。 如果您已安裝「部署伺服器」角色服務和「傳輸伺服器」角色服務（這是預設值），請使用[MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)。 請注意，在使用此選項之前，您必須先註冊內容提供者。
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
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/FriendlyName： <Friendly name>|指定命名空間的易記名稱。|
|/Description<Description>]|設定命名空間的描述。|
|/Namespace： <Namespace name>|指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<br /><br />-   **部署伺服器角色服務**：此選項的語法為/Namespace： WDS： <Image group> @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4。 例如: **WDS： ImageGroup1/install .wim/1**<br />-   **傳輸伺服器角色服務**：這個值應該符合在伺服器上建立命名空間時所指定的名稱。|
|/ContentProvider： <Name>]|指定將為命名空間提供內容的內容提供者名稱。|
|[/ConfigString： <Configuration string>]|指定內容提供者的設定字串。|
|/Namespacetype： {AutoCast &#124; ScheduledCast}|指定傳輸的設定。 您可以使用下列選項來指定設定：<br /><br />-[/time： <time>]-設定傳輸應使用下列格式啟動的時間：YYYY/MM/DD： hh： MM。 此選項只適用于已排程的轉換傳輸。<br />-[/Clients： <Number of clients>]-設定要在傳輸開始前等待的最小用戶端數目。 此選項只適用于已排程的轉換傳輸。|
## <a name="BKMK_examples"></a>典型
若要建立自動轉換命名空間，請輸入：
```
wdsutil /New-Namespace /FriendlyName:"Custom AutoCast Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
若要建立排程轉換命名空間，請輸入：
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:"Custom Scheduled Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:"2006/11/20:17:00" /Clients:20
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用 remove-Namespace 命令](using-the-remove-namespace-command.md)
[子命令： start-namespace](subcommand-start-namespace.md)
