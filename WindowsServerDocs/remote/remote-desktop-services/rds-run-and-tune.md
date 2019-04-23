---
title: RDS-執行和微調
description: 提供遠端桌面服務的管理的資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 02/08/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 40f8dbd560da359e8764ed715e7776cc2d230a7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862179"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>執行及調整您的遠端桌面服務環境

微調您的部署需要時間，而需要檢測和監視。 使用下列程序來精簡您的遠端桌面部署，讓它保持執行並啟用相應放大 （和縮小），視需要。 

它是個不錯的做法會持續評估的度量和針對執行成本平衡。

## <a name="management-and-monitoring"></a>管理和監視

請參閱[管理您的 RDS 集合中的使用者](rds-user-management.md)如需有關如何管理您的桌面和遠端資源的存取資訊。

使用**Microsoft Operations Management Suite (OMS)** 來監視潛在的瓶頸的遠端桌面部署和管理使用下列方法之一： 

- **伺服器管理員**:使用內建在 Windows Server 來管理最多 500 個並行的遠端使用者與部署 RD 管理工具。 
- **PowerShell**:您可以使用 RD PowerShell 模組，也內建於 Windows Server 來管理最多 5000 並行的遠端使用者的部署。

## <a name="scale-bigger-better-faster"></a>小數位數：更大、 更好、 更快

部署的可見度，您可以控制更多有效位數與小數位數。 輕鬆地新增或移除擴展需求為基礎的遠端桌面主機伺服器。 

建置在 Azure 的遠端桌面部署可使用的 Azure 服務，例如 Azure SQL 來依需求自動擴充。

## <a name="automation-script-for-success"></a>自動化：成功的指令碼

維護執行、 高度調整的應用程式，以包含重複的定期執行的作業。 若要開發可以在需要時的多個部署執行的指令碼中使用遠端桌面服務 PowerShell cmdlet 與 WMI 提供者。 最佳做法分析程式 (BPA) 上執行規則 for Remote Desktop Services 部署，以調整您的部署。

## <a name="load-testing-avoid-surprises"></a>負載測試：避免意外狀況

負載測試的壓力測試和實際使用量的模擬部署。 不同的載入大小，可避免意外狀況 ！ 確認回應符合使用者需求，而且整個系統就不怕。 建立負載測試模擬工具，例如 LoginVSI，檢查您的部署能否符合使用者需求。 