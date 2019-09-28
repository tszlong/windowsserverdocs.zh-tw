---
title: 開始使用 Windows Server Update Services （WSUS）
description: Windows Server Update Service （WSUS）主題-伺服器角色及其實際應用程式的總覽
ms.prod: windows-server
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
ms.openlocfilehash: 89247f91f616233fc6e4967a0457ff34fac221da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361653"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server Update Services (WSUS) 能夠讓資訊技術管理員部署最新的 Microsoft 產品更新。 您可以使用 WSUS 來完全管理透過 Microsoft Update 發行到網路電腦的更新散發。 這個主題提供這個伺服器角色的概觀，以及如何部署和維護 WSUS 的詳細資訊。

## <a name="wsus-server-role-description"></a>WSUS 伺服器角色描述
WSUS 伺服器提供一些功能，可讓您透過管理主控台來管理和散發更新。 WSUS 伺服器也可以是組織內其他 WSUS 伺服器的更新來源。 做為更新來源的 WSUS 伺服器稱為「上游伺服器」。 在 WSUS 的執行中，您網路上至少有一部 WSUS 伺服器必須能夠連線到 Microsoft Update 以取得可用的更新資訊。 身為系統管理員，您可以根據網路安全性和設定來判斷--有多少其他 WSUS 伺服器直接連線到 Microsoft Update。

### <a name="practical-applications"></a>實際應用
更新管理是控制過渡版軟體發行到生產環境的部署及維護的程序。 它幫助您維護運作效率、克服安全性弱點以及維護生產環境的穩定性。 如果您的組織無法判斷和維護作業系統與應用程式軟體內已知的信任層級，就可能出現一些安全性弱點，如果利用這些弱點，就會造成利潤和智慧財產權的損失。 為了將這個威脅的影響降到最小，您必須適當設定系統、使用最新的軟體以及安裝建議的軟體更新。

WSUS 為您企業提供附加價值的核心案例如下：

-   集中管理更新

-   更新管理自動化

### <a name="new-and-changed-functionality"></a>新功能和變更的功能

> [!NOTE]
> 從支援 WSUS 3.2 的任何 Windows Server 版本升級到 Windows Server 2012 R2，都需要先卸載 WSUS 3.2。
> 
> 在 Windows Server 2012 中，如果偵測到 WSUS 3.2，則會在安裝程式期間封鎖從已安裝 WSUS 3.2 的任何 Windows Server 版本進行升級。 在這種情況下，在升級伺服器之前，系統會提示您先卸載 Windows Server Update Services。
> 
> 不過，由於本版 Windows Server 和 Windows Server 2012 R2 的變更，從任何版本的 Windows Server 和 WSUS 3.2 進行升級時，不會封鎖安裝。 在執行 Windows Server 2012 R2 升級之前無法卸載 WSUS 3.2，會導致 Windows Server 2012 R2 中 WSUS 的後續安裝工作失敗。 在此情況下，唯一已知的修正措施是將硬碟格式化，並重新安裝 Windows Server。

Windows Server Update Services 是內建的伺服器角色，包含了下列增強功能：

-   可以使用伺服器管理員進行新增和移除

-   包含用來管理 WSUS 中最重要系統管理工作的 Windows PowerShell Cmdlet

-   加入 SHA256 雜湊功能，提供額外的安全性

-   提供用戶端和伺服器分離： Windows Update 代理程式（WUA）的版本可獨立于 WSUS 之外運送

### <a name="using-windows-powershell-to-manage-wsus"></a>使用 Windows PowerShell 管理 WSUS
系統管理員為了自動化他們的作業，必須透過命令列自動化來進行。 主要目標是允許系統管理員自動化他們每天的作業有助於 WSUS 系統管理。

**這個變更增加了什麼價值？**

透過 Windows PowerShell 公開核心 WSUS 作業，系統管理員就可以提高產能、降低新工具的學習曲線，以及減少因為類似作業缺乏一致性而無法達到期望所產生的錯誤。

**有哪些不同？**

在舊版的 Windows Server 作業系統中，沒有提供 Windows PowerShell Cmdlet，而且更新管理自動化是一項挑戰。 適用於 WSUS 作業的 Windows PowerShell Cmdlet 為系統管理員加入了彈性和靈活度。

## <a name="in-this-collection"></a>在此集合中
下列規劃、部署和管理 WSUS 的指南位於此集合：

-   [部署 Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [使用 Windows Server Update Services 管理更新](../manage/update-management-with-windows-server-update-services.md)


