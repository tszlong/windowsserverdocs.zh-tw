---
title: RDS - 執行並調整您的遠端桌面服務環境
description: 提供遠端桌面服務的管理資訊。
ms.author: spatnaik
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 5a68569943ac540204fd3538719c2a0ef6c4c979
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330430"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>執行並調整您的遠端桌面服務環境

調整部署需要時間，而需要檢測和監視。 使用下列程序來精簡您的遠端桌面部署，讓它保持運作中，並視需要，啟用相應放大 (和縮小)。

持續評估指標並平衡營運成本是一種不錯的做法。

## <a name="management-and-monitoring"></a>管理及監視

如需有關如何管理桌面和遠端資源存取的資訊，請參閱[管理 RDS 集合中的使用者](rds-user-management.md)。

使用 **Microsoft Operations Management Suite (OMS)** 監視遠端桌面部署中是否有潛在的瓶頸，並使用下列其中一種方法加以管理：

- **伺服器管理員**：使用內建在 Windows Server 中的 RD 管理工具來管理最多 500 個並行遠端使用者的部署。
- **PowerShell**：使用內建在 Windows Server 中的 RD PowerShell 模組來管理最多 5000 個並行遠端使用者的部署。

## <a name="scale-bigger-better-faster"></a>擴充：更大、更好、更快

透過對部署的可見度，您可以更精確地控制比例。 根據擴充需求，輕鬆地新增或移除遠端桌面主機伺服器。

在 Azure 上建置的遠端桌面部署可以使用 Azure 服務 (例如 Azure SQL)，依需求自動擴充。

## <a name="automation-script-for-success"></a>自動化：成功的指令碼

維護執行中且高度擴充的應用程式涉及定期重複操作。 使用遠端桌面服務 PowerShell Cmdlet 與 WMI 提供者開發可以在需要時，於多個部署上執行的指令碼。 在部署上，針對遠端桌面服務執行最佳做法分析程式 (BPA)，以調整您的部署。

## <a name="load-testing-avoid-surprises"></a>負載測試：避免意外狀況

同時以壓力測試和實際使用量模擬對部署進行負載測試。 請改變負載大小以避免意外狀況！ 請確定回應性符合使用者需求，且整個系統具復原能力。 請使用模擬工具 (如 LoginVSI) 建立負載測試，以確認部署的能力符合使用者需求。
