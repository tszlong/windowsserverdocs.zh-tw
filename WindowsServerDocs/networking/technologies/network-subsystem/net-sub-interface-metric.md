---
title: 設定網路介面順序
description: 本主題是 Windows Server 2016 的網路子系統效能微調指南的一部分。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d82009a4b6e9bcf20ae9cccb4259956358b53148
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316615"
---
# <a name="configure-the-order-of-network-interfaces"></a>設定網路介面順序

>適用於：Windows Server (半年通道)、Windows Server 2016

在 Windows Server 2016 和 Windows 10 中，您可以使用介面計量來設定網路介面的順序。

這與舊版的 Windows 和 Windows Server 不同，後者可讓您使用使用者介面或命令**INetCfgComponentBindings：： MoveBefore**和**INetCfgComponentBindings：： MoveAfter**來設定網路介面卡的系結順序。 這兩種用於排序網路介面的方法，在 Windows Server 2016 和 Windows 10 中都無法使用。

相反地，您可以藉由設定每張介面卡的介面計量，使用新的方法來設定網路介面卡的列舉順序。 您可以使用 Get-netipinterface Windows PowerShell 命令來[設定](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface)介面度量。

當選擇網路流量路由，而且您已**設定 get-netipinterface**命令的**InterfaceMetric**參數時，用來判斷介面喜好設定的整體計量是路由計量和介面計量的總和。 一般而言，介面計量會將喜好設定提供給特定的介面，例如，如果有線和無線都可以使用，就可以使用有線。

下列 Windows PowerShell 命令範例示範如何使用此參數。

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

介面卡出現在清單中的順序取決於 IPv4 或 IPv6 介面計量。  如需詳細資訊，請參閱[GetAdaptersAddresses 函數](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。

如需本指南中所有主題的連結，請參閱[網路子系統效能調整](net-sub-performance-top.md)。
