---
title: 容器網路功能概觀
description: 本主題概述適用於 Windows 容器的網路堆疊，並包含建立、 設定及管理容器網路的其他指導連結。
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
ms.date: 09/04/2018
ms.openlocfilehash: 72b1ac739d9012ac7b90e97abe22e5f321ddba63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886139"
---
# <a name="container-networking-overview"></a>容器網路功能概觀

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，我們提供的網路堆疊概觀適用於 Windows 容器和我們包括建立、 設定及管理容器網路的其他指導連結。

Windows Server 容器是輕量型作業系統虛擬化方法，將相同的容器主機上執行的其他服務應用程式或服務。 Windows 容器函式，類似於虛擬機器。 啟用時，每個容器會有作業系統、 程序、 檔案系統、 登錄和 IP 位址，您可以連接到虛擬網路的個別檢視。 

Windows 容器主機上執行的所有容器與容器主機共用核心。 由於共用核心空間，因此這些容器需要相同的核心版本和組態。 容器會提供透過程序和命名空間隔離技術的應用程式隔離。

>[!IMPORTANT]
>Windows 容器不提供惡意的安全性界限，而且不應該用來隔離不受信任的程式碼。 

與 Windows 容器，您可以部署 HYPER-V 主機，您在 VM 主機建立一或多個虛擬機器的位置。 在 VM 主機中，建立容器，並是透過虛擬機器內執行的虛擬交換器的網路存取。 您可以使用可重複使用的存放庫中儲存的映像將作業系統和服務部署到容器。 每個容器有虛擬網路介面卡連接到虛擬交換器，轉送輸入和輸出流量。 您可以將容器端點附加至本機主機網路 （例如 NAT)、 實體網路或建立透過 SDN 堆疊的重疊虛擬網路。

強制使用相同的主機上的容器之間的隔離，您會建立每個 Windows Server 和 HYPER-V 容器的網路區間。 Windows Server 容器使用主機 vNIC 連結到虛擬交換器。 Hyper-V 容器使用綜合 VM NIC (不向公用程式 VM 公開) 連結到虛擬交換器。 

## <a name="related-topics"></a>相關主題 

- [Windows 容器網路功能](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture):了解如何建立和管理容器網路非重疊/SDN 部署。

- [將容器端點連線至租用戶虛擬網路](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md):了解如何建立和管理 SDN 重疊虛擬網路的容器網路。 