---
title: 開始使用 Windows Server Update Services (WSUS)
description: Windows Server Update Service (WSUS) 主題-伺服器角色和其實際的應用程式的概觀
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 7a6c64e0a4321553162b426e3d6857ff6ac3581c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830259"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Windows Server Update Services (WSUS) 能夠讓資訊技術管理員部署最新的 Microsoft 產品更新。 您可以使用 WSUS 來完全管理的 Microsoft Update 發行到您網路上電腦的更新之散佈。 這個主題提供這個伺服器角色的概觀，以及如何部署和維護 WSUS 的詳細資訊。

## <a name="wsus-server-role-description"></a>WSUS 伺服器角色的描述
WSUS 伺服器會提供您可用來管理和散佈更新，透過管理主控台的功能。 WSUS 伺服器也可以在組織內其他 WSUS 伺服器的更新來源。 做為更新來源的 WSUS 伺服器稱為「上游伺服器」。 在 WSUS 實作中，您的網路上的至少一部 WSUS 伺服器必須能夠連線到 Microsoft Update，以取得可用的更新資訊。 身為管理員，您可以判斷-根據網路安全性和設定-多少其他 WSUS 伺服器直接連線到 Microsoft Update。

### <a name="practical-applications"></a>實際應用
更新管理是控制過渡版軟體發行到生產環境的部署及維護的程序。 它幫助您維護運作效率、克服安全性弱點以及維護生產環境的穩定性。 如果您的組織無法判斷和維護作業系統與應用程式軟體內已知的信任層級，就可能出現一些安全性弱點，如果利用這些弱點，就會造成利潤和智慧財產權的損失。 為了將這個威脅的影響降到最小，您必須適當設定系統、使用最新的軟體以及安裝建議的軟體更新。

WSUS 為您企業提供附加價值的核心案例如下：

-   集中管理更新

-   更新管理自動化

### <a name="new-and-changed-functionality"></a>新功能和變更的功能

> [!NOTE]
> 從支援 WSUS 3.2，到 Windows Server 2012 R2 的 Windows Server 任何版本的升級需要您先解除安裝 WSUS 3.2。
> 
> 在 Windows Server 2012 中，從任何版本的 Windows Server 安裝了 WSUS 3.2 升級會封鎖安裝程序期間如果偵測到 WSUS 3.2。 在此情況下，系統會提示您升級您的伺服器前，先解除安裝 Windows Server Update Services。
> 
> 不過，由於在這一版的 Windows Server 和 Windows Server 2012 R2 中，從任何版本的 Windows Server 和 WSUS 3.2 升級時變更，並不會封鎖安裝。 若要在執行 Windows Server 2012 R2 升級之前先解除安裝 WSUS 3.2 的失敗會導致貼文的 WSUS 安裝工作失敗的 Windows Server 2012 R2。 在此情況下，唯一已知的修正方法是格式化硬碟並重新安裝 Windows Server。

Windows Server Update Services 是內建的伺服器角色，包含了下列增強功能：

-   可以使用伺服器管理員進行新增和移除

-   包含 Windows PowerShell cmdlet 來管理 WSUS 中最重要的系統管理工作

-   加入 SHA256 雜湊功能，提供額外的安全性

-   提供用戶端與伺服器個別版本： 分開 WSUS 提供版本的 Windows Update Agent (WUA)

### <a name="using-windows-powershell-to-manage-wsus"></a>使用 Windows PowerShell 管理 WSUS
系統管理員為了自動化他們的作業，必須透過命令列自動化來進行。 主要目標是允許系統管理員自動化他們每天的作業有助於 WSUS 系統管理。

**這個變更增加了什麼價值？**

透過 Windows PowerShell 公開核心 WSUS 作業，系統管理員就可以提高產能、降低新工具的學習曲線，以及減少因為類似作業缺乏一致性而無法達到期望所產生的錯誤。

**有哪些不同？**

在舊版的 Windows Server 作業系統中，沒有提供 Windows PowerShell Cmdlet，而且更新管理自動化是一項挑戰。 適用於 WSUS 作業的 Windows PowerShell Cmdlet 為系統管理員加入了彈性和靈活度。

## <a name="in-this-collection"></a>在此集合中
規劃的下列指南、 部署和管理 WSUS 位於此集合中：

-   [部署 Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [使用 Windows Server Update Services 管理更新](../manage/update-management-with-windows-server-update-services.md)


