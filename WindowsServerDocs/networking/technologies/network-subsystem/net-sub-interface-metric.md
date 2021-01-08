---
title: 設定網路介面順序
description: 瞭解如何使用介面度量來設定網路介面的順序。
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 08/07/2020
ms.openlocfilehash: 9f75e68b04ba32421148f4f21e8a6ca5aed8eaad
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040378"
---
# <a name="configure-the-order-of-network-interfaces"></a>設定網路介面順序

>適用於：Windows Server (半年度管道)、Windows Server 2016

在 Windows Server 2016 和 Windows 10 中，您可以使用介面度量來設定網路介面的順序。

這與舊版 Windows 和 Windows Server 不同，這可讓您使用使用者介面或命令 **INetCfgComponentBindings：： MoveBefore** 和 **INetCfgComponentBindings：： MoveAfter** 來設定網路介面卡的系結順序。 這兩種排序網路介面的方法無法在 Windows Server 2016 和 Windows 10 中使用。

相反地，您可以藉由設定每個介面卡的介面度量，使用新的方法來設定網路介面卡的列舉順序。 您可以使用 Get-netipinterface Windows PowerShell 命令來 [設定](/powershell/module/nettcpip/set-netipinterface) 介面度量。

如果選擇網路流量路由，而且您已 **設定 get-netipinterface** 命令的 **InterfaceMetric** 參數，則用來判斷介面喜好設定的整體度量就是路由計量和介面度量的總和。 一般而言，介面計量會提供特定介面的喜好設定，例如，如果有線和無線都可使用，則會使用有線。

下列 Windows PowerShell 命令範例示範如何使用此參數。

```powershell
Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15
```

介面卡出現在清單中的順序取決於 IPv4 或 IPv6 介面度量。  如需詳細資訊，請參閱 [GetAdaptersAddresses 函數](/windows/win32/api/iphlpapi/nf-iphlpapi-getadaptersaddresses?f=255&MSPPError=-2147217396)。

如需本指南中所有主題的連結，請參閱 [網路子系統效能微調](net-sub-performance-top.md)。