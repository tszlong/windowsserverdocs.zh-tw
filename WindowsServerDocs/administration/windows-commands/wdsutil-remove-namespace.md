---
title: wdsutil 移除-命名空間
description: Wdsutil remove 命名空間的參考文章，移除自訂命名空間。
ms.topic: reference
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bc5d44b727bf93bf6630f7289833640397e69dae
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730536"
---
# <a name="wdsutil-remove-namespace"></a>wdsutil 移除-命名空間

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

移除自訂命名空間。

## <a name="syntax"></a>語法
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|命名空間<Namespace name>|指定命名空間的名稱。 這不是易記的名稱，而且必須是唯一的。<p>-   **部署伺服器角色服務**：命名空間名稱的語法為/NAMESPACE： WDS： <ImageGroup> / <ImageName> / <Index> 。 例如： **WDS： ImageGroup1/install .wim/1**<br />-   **傳輸伺服器角色服務**：此值必須符合在伺服器上建立命名空間時所提供的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/force|立即移除命名空間，並終止所有用戶端。 請注意，除非您指定 **/force**，否則現有的用戶端可以完成傳送，但新的用戶端無法加入。|
## <a name="examples"></a>範例
若要停止命名空間 (目前的用戶端可以完成傳送，但新的用戶端無法加入) ，請輸入：
```
wdsutil /remove-Namespace /Namespace:Custom Auto 1
```
若要強制終止所有用戶端，請輸入：
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /force
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allnamespaces 命令](wdsutil-get-allnamespaces.md)
- [wdsutil 新命名空間命令](wdsutil-new-namespace.md)
- [wdsutil 開始-命名空間命令](wdsutil-start-namespace.md)
