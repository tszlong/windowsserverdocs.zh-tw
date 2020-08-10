---
title: 開始使用 Windows Server Update Services (WSUS)
description: Windows Server Update Service (WSUS) 主題 - 伺服器角色及其實際應用的概觀
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 45f88b9295bfc2d48d8e1a599b33bea05717ef0f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881049"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server Update Services (WSUS) 能夠讓資訊技術管理員部署最新的 Microsoft 產品更新。 您可以使用 WSUS 充分地管理如何將 Microsoft Update 所發行的更新，散佈到您網路的電腦上。 這個主題提供這個伺服器角色的概觀，以及如何部署和維護 WSUS 的詳細資訊。

## <a name="wsus-server-role-description"></a>WSUS 伺服器角色描述
WSUS 伺服器提供您透過管理主控台來管理和散發更新時所需的功能。 WSUS 伺服器也可以是組織內其他 WSUS 伺服器的更新來源。 做為更新來源的 WSUS 伺服器稱為「上游伺服器」。 在 WSUS 實作中，網路中至少要有一部 WSUS 伺服器必須能夠連線到 Microsoft Update，以取得可用的更新資訊。 身為系統管理員，您可以根據網路安全性和設定，決定有多少其他 WSUS 伺服器可以與 Microsoft Update 直接連線。

### <a name="practical-applications"></a>實際應用
更新管理是控制過渡版軟體發行到生產環境的部署及維護的程序。 它幫助您維護運作效率、克服安全性弱點以及維護生產環境的穩定性。 如果您的組織無法判斷和維護作業系統與應用程式軟體內已知的信任層級，就可能出現一些安全性弱點，如果利用這些弱點，就會造成利潤和智慧財產權的損失。 為了將這個威脅的影響降到最小，您必須適當設定系統、使用最新的軟體以及安裝建議的軟體更新。

WSUS 為您企業提供附加價值的核心案例如下：

-   集中管理更新

-   更新管理自動化

### <a name="new-and-changed-functionality"></a>新功能和變更的功能

> [!NOTE]
> 從支援 WSUS 3.2 的任何 Windows Server 版本升級到 Windows Server 2012 R2 時，需要先解除安裝 WSUS 3.2。
>
> 在 Windows Server 2012 中，如果在安裝程序中偵測到 WSUS 3.2，就會封鎖安裝了 WSUS 3.2 的任何 Windows Server 版本進行升級。 在這種情況下，系統會提示您在升級伺服器之前，必須先解除安裝 Windows Server Update Services。
>
> 不過，因為此版本 Windows Server 和 Windows Server 2012 R2 中的變更，所以從任何 Windows Server 版本與 WSUS 3.2 升級時，並不會封鎖安裝。 在執行 Windows Server 2012 R2 升級前沒有先解除安裝 WSUS 3.2，會造成 Windows Server 2012 R2 中 WSUS 安裝後工作失敗。 在此情況下，唯一已知的修正方法是格式化硬碟並重新安裝 Windows Server。

Windows Server Update Services 是內建的伺服器角色，包含了下列增強功能：

-   可以使用伺服器管理員進行新增和移除

-   包含用來管理 WSUS 中最重要系統管理工作的 Windows PowerShell Cmdlet

-   加入 SHA256 雜湊功能，提供額外的安全性

-   提供用戶端與伺服器個別版本：分開提供 Windows Update 代理程式 (WUA) 版本與 WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>使用 Windows PowerShell 管理 WSUS
系統管理員為了自動化他們的作業，必須透過命令列自動化來進行。 主要目標是允許系統管理員自動化他們每天的作業有助於 WSUS 系統管理。

**這個變更增加了什麼價值？**

透過 Windows PowerShell 公開核心 WSUS 作業，系統管理員就可以提高產能、降低新工具的學習曲線，以及減少因為類似作業缺乏一致性而無法達到期望所產生的錯誤。

**有哪些不同？**

在舊版的 Windows Server 作業系統中，沒有提供 Windows PowerShell Cmdlet，而且更新管理自動化是一項挑戰。 適用於 WSUS 作業的 Windows PowerShell Cmdlet 為系統管理員加入了彈性和靈活度。

## <a name="in-this-collection"></a>在本集合中
本集合中包含規劃、部署和管理 WSUS 的下列指南：

-   [部署 Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [使用 Windows Server Update Services來管理更新](../manage/update-management-with-windows-server-update-services.md)


