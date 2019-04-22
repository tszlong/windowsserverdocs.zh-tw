---
title: 設定網路介面順序
description: 本主題是 Windows Server 2016 的網路子系統效能調整指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 18bc9a268b4e69e4b87b6b1e310f1162473adb10
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821769"
---
# <a name="configure-the-order-of-network-interfaces"></a>設定網路介面順序

>適用於：Windows Server （半年通道），Windows Server 2016

在 Windows Server 2016 和 Windows 10 中，您可以使用的介面公制，若要設定的網路介面的順序。

這是不同於在舊版的 Windows 和 Windows Server，可讓您使用的使用者介面或命令來設定網路介面卡的繫結順序**INetCfgComponentBindings::MoveBefore**並**INetCfgComponentBindings::MoveAfter**。 無法在 Windows Server 2016 和 Windows 10 中使用這兩種方法來排序網路介面。

相反地，您可以使用新的方法來設定網路介面卡的列舉的順序，藉由設定每個介面卡的介面公制。 您可以使用設定的介面公制[組 NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) Windows PowerShell 命令。

當選擇網路流量路由，且您已設定**介面公制**的參數**組 NetIPInterface**命令時，用來判斷介面的整體度量喜好設定為路由公制] 和 [介面公制的總和。 一般而言的介面公制提供喜好設定給特定的介面，例如使用有線如果兩者有線及無線可用。

下列 Windows PowerShell 命令範例示範使用此參數。

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

配接器清單中出現的順序取決於 IPv4 或 IPv6 介面度量。  如需詳細資訊，請參閱 < [gaa_flag_include_tunnel_bindingorder 平面函式](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。

本指南中的所有主題的連結，請參閱[網路子系統效能調整](net-sub-performance-top.md)。
