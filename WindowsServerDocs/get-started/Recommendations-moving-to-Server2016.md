---
title: 移至 Windows Server 2016 的建議
description: 移至 Windows Server 2016 的建議。
ms.prod: windows-server
ms.date: 10/18/2016
ms.technology: server-general
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 478c967e9ce769f184d72d3534ed99c84924daed
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963940"
---
# <a name="recommendations-for-moving-to-windows-server-2016"></a>移至 Windows Server 2016 的建議

>適用於：Windows Server 2016


|如果您執行：|Windows Server 2012 R2 或 Windows Server 2012|Windows Server 2008 R2 或 Windows Server 2008|  
|-------------------|----------|--------------|--------------|---------------------------------------|  
|**Windows Server 角色基礎結構**|取決於[特定角色指引](./migrate-roles-and-features.md)來選擇升級或移轉。|- 若要善用 Windows Server 2016 的新功能，請部署新的硬體，或在現有主機的虛擬機器中安裝 Windows Server 2016。 部分新功能最適用於執行 Hyper-V 的 Windows Server 2016 實體主機。 <br>- 遵循[特定角色指引](./migrate-roles-and-features.md)。|
|**Microsoft 伺服器管理和應用程式工作負載**|- 應用程式升級應包含*移轉* 至 Windows Server 2016。 請參閱[相容性清單](Server-Application-Compatibility.md)。 <br>- 僅升級至 Windows Server 2016 (意即不升級應用程式) 應使用應用程式特定的指引。|- 若要善用 Windows Server 2016 的新功能，請部署新的硬體，或在現有主機的虛擬機器中安裝 Windows Server 2016。 部分新功能最適用於執行 Hyper-V 的 Windows Server 2016 實體主機。 請遵循適用的移轉指引。 <br>- 或保留在目前的 OS 上，並在 Windows Server 2016 主機或 Microsoft Azure 所執行的虛擬機器中執行。 請透過[軟體保證](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx)連絡您的 EA 轉銷商、TAM 或 Microsoft 取得延伸支援選項。|
|**ISV 應用程式工作負載**|- 升級至 Windows Server 2016 應使用應用程式特定的指引。 <br>如需 Windows Server 與非 Microsoft 應用程式的相容性相關資訊，請瀏覽 [Windows Server Logo Certification 入口網站](https://azure.microsoft.com/publish-your-app/)。|- 若要善用 Windows Server 2016 的新功能，請部署新的硬體，或在現有主機的虛擬機器中安裝 Windows Server 2016。 部分新功能最適用於執行 Hyper-V 的 Windows Server 2016 實體主機。 請遵循適用的移轉指引。 <br>- 或保留在目前的 OS 上，並在 Windows Server 2016 主機或 Microsoft Azure 所執行的虛擬機器中執行。 請透過[軟體保證](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx)連絡您的 EA 轉銷商、TAM 或 Microsoft 取得延伸支援選項。|
|**自訂應用程式工作負載**|- 請洽詢應用程式開發人員取得 Windows Server 2016 相容性及升級指南。 <br>- 善用 Microsoft Azure 在切換前於 Windows Server 2016 測試應用程式。 <br>- 請參閱下一節中的完整選項。|- 請洽詢應用程式開發人員取得 Windows Server 2016 相容性及升級指南。 <br>- 善用 Microsoft Azure 在切換前於 Windows Server 2016 測試您的應用程式。 <br>- 若要善用 Windows Server 2016 的新功能，請部署新的硬體，或在現有主機的虛擬機器中安裝 Windows Server 2016。 部分新功能最適用於執行 Hyper-V 的 Windows Server 2016 實體主機。 <br>- 請參閱下一節中的完整選項。|

## <a name="complete-options-for-moving-servers-running-custom-or-in-house-applications-on-older-versions-of-windows-server-to-windows-server-2016"></a>完成將在 Windows Server 較舊版本上執行自訂或內部應用程式的伺服器移至 Windows Server 2016 的選項

現在有比以往更多的選項供您選擇，可協助您和客戶善用 Windows Server 2016 的功能，且會將對您目前服務及工作負載的影響降至最低。

- 搭配您的應用程式試試最新的作業系統，方法是下載評估版 [Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) 進行內部測試。 完成測試並確認品質後，您可使用零售授權金鑰執行簡易的授權轉換 (需要重新啟動)。

- [Microsoft Azure](https://azure.microsoft.com) 也能以試用為基礎進行測試，以確保您的自訂應用程式能夠在最新的伺服器作業系統上運作。 完成測試並確認品質後，於內部[移轉至最新版本的 Windows Server](./installation-and-upgrade.md#upgrade)。 

- 或者，完成測試並確認品質後，[Microsoft Azure](https://azure.microsoft.com) 可用來作為您自訂應用程式或服務的永久位置。 這可讓舊的伺服器保持可用，直到您準備好要切換至 Azure 的新伺服器為止。

    - 如果您已有 Windows Server 的軟體保證，即可透過使用 [Azure 混合式使用權益](https://azure.microsoft.com/pricing/hybrid-use-benefit/) 進行部署來節省成本。 

- 在大部分情況下，[Microsoft Azure](https://azure.microsoft.com) 可用於裝載現今所執行與 Windows Server 較舊版本所執行相同的應用程式。 使用 [Azure Marketplace](https://azure.microsoft.com/marketplace/) 映像將應用程式和工作負載移轉至具有您所選作業系統的虛擬機器。

    - 如果您已有 Windows Server 的軟體保證，即可透過使用 [Azure 混合式使用權益](https://azure.microsoft.com/pricing/hybrid-use-benefit/) 進行部署來節省成本。 

- Windows Server 的[軟體保證](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx)計畫提供新版本權限的權益。 除了一系列其他權益之外，附有軟體保證的伺服器還可以在可用時升級至最新版的 Windows Server，無需購買新的授權。 

## <a name="additional-resources"></a>其他資源

- [Windows Server 2016 已移除或取代的功能](deprecated-features.md)
- 如需一般伺服器升級及移轉選項，請瀏覽 [Windows Server 2016 的升級及轉換選項](Supported-Upgrade-Paths.md)。
- 如需產品生命週期及支援層級的詳細資訊，請參閱[支援週期原則常見問題集](https://support.microsoft.com/help/17140/support-lifecycle-policy-faq)。
