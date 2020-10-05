---
title: wdsutil 新命名空間
description: Wdsutil 新命名空間的參考文章，可建立並設定新的命名空間。
ms.topic: reference
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 67f479af3644cad5c587a4c6eb17dbf43e1fbe0c
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729871"
---
# <a name="wdsutil-new-namespace"></a>wdsutil 新命名空間

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立並設定新的命名空間。 當您只安裝傳輸伺服器角色服務時，應該使用此選項。 如果您已安裝部署伺服器角色服務和傳輸伺服器角色服務 (這是預設) ，請使用 [wdsutilnew-MulticastTransmission 命令](wdsutil-new-multicasttransmission.md)。 請注意，您必須先註冊內容提供者，才能使用此選項。
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
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|友好<Friendly name>|指定命名空間的好記名稱。|
|/Description<Description>]|設定命名空間的描述。|
|命名空間<Namespace name>|指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<p>-   **部署伺服器角色服務**：此選項的語法為/NAMESPACE： WDS： <Image group> / <Image name> / <Index> 。 例如： **WDS： ImageGroup1/install .wim/1**<br />-   **傳輸伺服器角色服務**：此值應該符合在伺服器上建立命名空間時所提供的名稱。|
|/ContentProvider： <Name> ]|指定將提供命名空間內容的內容提供者名稱。|
|[/ConfigString： <Configuration string> ]|指定內容提供者的設定字串。|
|/Namespacetype： {AutoCast &#124; ScheduledCast}|指定傳輸的設定。 您可以使用下列選項來指定設定：<p>-[/time： <time> ]-使用下列格式設定傳輸應開始的時間： YYYY/MM/DD： hh： MM。 此選項只適用于已排程的轉換傳輸。<br />-[/Clients： <Number of clients> ]-設定開始傳輸之前等候的用戶端數目下限。 此選項只適用于已排程的轉換傳輸。|
## <a name="examples"></a>範例
若要建立自動轉換命名空間，請輸入：
```
wdsutil /New-Namespace /FriendlyName:Custom AutoCast Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
若要建立已排程的轉換命名空間，請輸入：
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:Custom Scheduled Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider
/Namespacetype:ScheduledCast /time:2006/11/20:17:00 /Clients:20
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allnamespaces 命令](wdsutil-get-allnamespaces.md)
- [wdsutil remove-namespace 命令](wdsutil-remove-namespace.md)
- [wdsutil 開始-命名空間命令](wdsutil-start-namespace.md)
