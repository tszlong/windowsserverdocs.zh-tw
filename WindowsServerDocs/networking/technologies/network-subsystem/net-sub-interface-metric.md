---
title: 設定網路介面的訂單
description: 本主題是部分的 Windows Server 2016 的網路效能子系統調整節目表。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2edf9d312e50a0fd8485e0032dee8413675367cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-order-of-network-interfaces"></a>設定網路介面的訂單

>適用於：Windows Server（以每年次管道）、Windows Server 2016

在 Windows Server 2016 和 Windows 10 中，您可以使用介面度量設定的網路介面順序。

這是在舊版 Windows 和 Windows Server 允許您透過使用者介面或命令設定繫結訂單的網路介面卡的不同於**INetCfgComponentBindings::MoveBefore**和**INetCfgComponentBindings::MoveAfter**。 這兩種方法來排序網路介面不是在 Windows Server 2016 和 Windows 10 中提供。

而是，您可以使用新的方法所設定的每個介面卡介面度量設定列舉的訂單的網路介面卡。 您可以設定介面度量使用[設定為 NetIPInterface](https://docs.microsoft.com/en-us/powershell/module/nettcpip/set-netipinterface) Windows PowerShell 命令。

當網路流量路徑選擇與您已設定**介面公制**的參數**設定-NetIPInterface**命令整體用來判斷介面喜好設定的這個度量是路由度量和和介面度量。 一般而言，介面度量提供特定的介面，例如使用有線如果兩者有線並無線有可用的喜好設定。

Windows PowerShell 命令下例顯示使用此參數。

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

介面卡清單中出現的順序是由 IPv4 或 IPv6 介面度量判斷。  如需詳細資訊，請查看[GetAdaptersAddresses 函式](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。

本指南所有主題的連結，會看到[的網路效能子系統調整](net-sub-performance-top.md)。
