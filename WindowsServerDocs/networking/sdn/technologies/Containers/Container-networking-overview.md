---
title: 容器的網路概觀
description: 本主題適用於 Windows 容器網路堆疊概觀且包含了連結來建立、設定及管理容器網路的相關的其他指導方針。
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.openlocfilehash: fd2f022948208d4aacce2994ff053e77384b28fc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="container-networking-overview"></a>容器的網路概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 Windows 容器網路堆疊概觀且包含了連結來建立、設定及管理容器網路的相關的其他指導方針。

Windows Server 容器是用來與其他服務的容器主機上執行分開應用程式或服務的輕量型作業系統模擬方法。 若要於此，每個容器會有自己的作業系統，程序，檔案系統、登錄和 IP 位址的檢視。

Windows 容器功能類似虛擬電腦中的網路。 每個容器有 virtual 網路介面卡，已連接到 virtual 切換，輸入 / 輸出流量轉送哪些。 若要執行的容器主機上之間隔離，網路區間是每個 Windows Server 和建立 HYPER-V 容器容器的網路介面卡已安裝的。 Windows Server 容器使用主機但 vNIC 附加至 virtual 切換。 HYPER-V 容器使用（不的公用程式 vm 公開）合成 VM NIC 附加至 virtual 切換。 

容器端點可以附加到本機主機網路 (例如 NAT)、實體網路或透過 Microsoft 軟體定義網路 (SDN) 堆疊建立覆疊 virtual 網路。 

建立及管理容器網路非覆疊日 SDN 部署的詳細資訊，請參考[Windows 容器網路](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking)在 MSDN 指南。

建立和管理容器網路 SDN 覆疊 virtual 網路的相關詳細資訊，請參考[連接承租人 virtual 網路容器端點](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md)。 