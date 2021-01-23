---
title: 容器網路功能概觀
description: 本主題概要說明 Windows 容器的網路堆疊，並包含有關建立、設定和管理容器網路的其他指引連結。
manager: grcusanz
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: anpaul
author: AnirbanPaul
ms.date: 09/04/2018
ms.openlocfilehash: 2ecc240cc1886a0355b1bf1785e2f634388474bf
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716864"
---
# <a name="container-networking-overview"></a>容器網路功能概觀

>適用於：Windows Server 2019、Windows Server 2016

在本主題中，我們將概述 Windows 容器的網路堆疊，並包含有關建立、設定和管理容器網路的其他指引連結。

Windows Server 容器是輕量的作業系統虛擬化方法，可將應用程式或服務與在相同容器主機上執行的其他服務分隔開來。 Windows 容器與虛擬機器的運作方式類似。 啟用時，每個容器都有個別的作業系統、進程、檔案系統、登錄和 IP 位址的觀點，您可以連接到虛擬網路。

Windows 容器與容器主機和主機上執行的所有容器共用核心。 由於共用核心空間，因此這些容器需要相同的核心版本和組態。 容器透過程式和命名空間隔離技術，提供應用程式隔離。

>[!IMPORTANT]
>Windows 容器不會提供惡意的安全性界限，也不能用來隔離不受信任的程式碼。

您可以使用 Windows 容器來部署 Hyper-v 主機，以在 VM 主機上建立一或多部虛擬機器。 在 VM 主機內，會建立容器，而網路存取是透過在虛擬機器中執行的虛擬交換器。 您可以使用儲存在存放庫中的可重複使用映射，將作業系統和服務部署到容器中。 每個容器都有一個連線到虛擬交換器的虛擬網路介面卡，並轉送輸入和輸出流量。 您可以將容器端點連接到本機主機網路 (例如 NAT) 、透過 SDN 堆疊建立的實體網路或重迭虛擬網路。

若要在相同主機上的容器之間強制隔離，您必須為每個 Windows Server 和 Hyper-v 容器建立網路區間。 Windows Server 容器使用主機 vNIC 連結到虛擬交換器。 Hyper-V 容器使用綜合 VM NIC (不向公用程式 VM 公開) 連結到虛擬交換器。

## <a name="related-topics"></a>相關主題

- [Windows 容器網路](/virtualization/windowscontainers/container-networking/architecture)功能：瞭解如何建立和管理非重迭/SDN 部署的容器網路。

- [將容器端點連線至租使用者虛擬網路](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md)：瞭解如何使用 SDN 來建立和管理重迭虛擬網路的容器網路。