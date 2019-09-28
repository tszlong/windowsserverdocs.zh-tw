---
title: 容器網路功能概觀
description: 本主題概述適用于 Windows 容器的網路堆疊，並包含有關建立、設定和管理容器網路的其他指引連結。
manager: ravirao
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.date: 09/04/2018
ms.openlocfilehash: 352b4303b7cf08a0c53712e46a309b8365c10d08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355683"
---
# <a name="container-networking-overview"></a>容器網路功能概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，我們將概述 Windows 容器的網路堆疊，並提供有關建立、設定和管理容器網路的其他指引連結。

Windows Server 容器是輕量的作業系統虛擬化方法，用來分隔應用程式或服務與在相同容器主機上執行的其他服務。 Windows 容器的運作方式類似于虛擬機器。 啟用時，每個容器都有個別的作業系統、處理常式、檔案系統、登錄和 IP 位址的觀點，您可以連接到虛擬網路。 

Windows 容器會與容器主機和所有在主機上執行的容器共用核心。 由於共用核心空間，因此這些容器需要相同的核心版本和組態。 容器可透過程式和命名空間隔離技術，提供應用程式隔離。

>[!IMPORTANT]
>Windows 容器不提供惡意的安全性界限，而且不應該用來隔離不受信任的程式碼。 

使用 Windows 容器時，您可以部署 Hyper-v 主機，在其中于 VM 主機上建立一或多個虛擬機器。 在 VM 主機內，會建立容器，而網路存取是透過在虛擬機器中執行的虛擬交換器。 您可以使用儲存在存放庫中的可重複使用映射，將作業系統和服務部署到容器中。 每個容器都有一個連線到虛擬交換器的虛擬網路介面卡，可轉送輸入和輸出流量。 您可以將容器端點連接至本機主機網路（例如 NAT）、實體網路或透過 SDN 堆疊建立的重迭虛擬網路。

若要在相同主機上的容器之間強制隔離，您可以為每個 Windows Server 和 Hyper-v 容器建立網路區間。 Windows Server 容器使用主機 vNIC 連結到虛擬交換器。 Hyper-V 容器使用綜合 VM NIC (不向公用程式 VM 公開) 連結到虛擬交換器。 

## <a name="related-topics"></a>相關主題 

- [Windows 容器網路](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture)功能：瞭解如何建立及管理非重迭/SDN 部署的容器網路。

- [將容器端點連線至租使用者虛擬網路](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md)：瞭解如何使用 SDN 來建立和管理用於重迭虛擬網路的容器網路。 