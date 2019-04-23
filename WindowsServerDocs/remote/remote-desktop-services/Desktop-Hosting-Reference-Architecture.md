---
title: 桌面主機參考架構
description: 建立桌面主機解決方案使用 RDS 和 Azure 架構的指引。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: 6f235fd89c34c00601c802f4ea71e440af630169
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890239"
---
# <a name="desktop-hosting-reference-architecture"></a>桌面主機參考架構

>適用於：Windows Server （半年通道），Windows Server 2016

這篇文章會定義一組的架構區塊，使用遠端桌面服務 (RDS) 和 Microsoft Azure 虛擬機器來建立多租用戶、 託管 Windows 桌面和應用程式的服務，我們稱之為 「 桌面主機 」。 您可以建立高度安全、 可擴充且可靠的桌面主機解決方案的小型和中型大小 5 到 5000 使用者的組織使用這個架構參考。    
  
此參考架構的主要對象主機服務提供者想要運用透過多個租用戶提供桌面主機服務的 Microsoft Azure 基礎結構服務及訂戶存取授權 (Sal) [Microsoft 服務提供者授權合約](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx)(SPLA) 方案。 此參考架構的第二個對象是想要建立並管理他們自己的員工使用的 Microsoft Azure 基礎結構服務中的桌面主機解決方案的客戶[延伸透過軟體的權限的 RDS 使用者 Cal保證](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)(SA)。   
  
若要提供桌面主機解決方案，主控合作夥伴和 SA 客戶利用提供 Windows 使用者熟悉的商務使用者和取用者應用程式體驗的 Windows Server。 建置在 Windows 10 的基礎上，Windows Server 2016 提供熟悉的應用程式支援和使用者體驗。    
  
本文件的範圍僅限於：   
  
* 桌面主機服務的架構設計指引。 詳細的資訊，例如部署程序、 效能和容量規劃是在個別的文件中說明。 更多一般 Azure 基礎結構服務的詳細資訊，請參閱[Microsoft Azure 虛擬機器](https://azure.microsoft.com/documentation/services/virtual-machines/)。   
  
* 工作階段為基礎的桌面、 RemoteApp 的應用程式和伺服器為基礎的個人桌面，使用 Windows Server 2016 遠端桌面工作階段主機 （RD 工作階段主機）。 未涵蓋 Windows 用戶端虛擬桌面基礎結構，因為沒有任何服務提供者授權合約 (SPLA) 的 Windows 用戶端作業系統。 允許 windows 伺服器型虛擬桌面基礎結構在 SPLA 中，並允許在某些情況下的一般客戶授權的專用硬體上的 Windows 用戶端虛擬桌面基礎結構。 不過，用戶端虛擬桌面基礎結構是範圍外的這份文件。   
  
* Microsoft 產品和功能，主要是 Windows Server 2016 和 Microsoft Azure 基礎結構服務。   
  
* 桌面主機服務的規模範圍從 5 到 5000 個使用者的租用戶。   對於較大的租用戶，您可能需要修改此架構，以提供足夠的效能。 伺服器管理員 RDS 圖形化使用者介面 (GUI) 不是建議用於部署超過 500 位使用者。 PowerShell 管理 RDS 部署，介於 500 到 5000 使用者建議。   
  
* 元件和服務的桌面主機服務所需的最小的集合。 有許多選用元件和服務，可以加入至增強的桌面主機服務，但這些範圍外的這份文件。    
  
閱讀這份文件之後, 讀取器應該了解：   
- 為了提供安全、 可靠且多租用戶的桌面主機解決方案為基礎在 Microsoft Azure 服務的建置組塊。  
- 每個建置組塊的目的和它們如何相互配合。  
  
有多個方法，可建置桌面主機解決方案，此架構為基礎。 此架構會概述整合和使用 Windows Server 2016 在 Azure 中的增強功能。 其他部署選項可用於[桌面主機參考架構指南](https://go.microsoft.com/fwlink/p/?LinkId=517389)適用於 Windows Server 2012 R2。    
  
涵蓋下列主題：  
- [桌面主機邏輯架構](Desktop-hosting-logical-architecture.md)  
- [了解的 RDS 角色](Understanding-RDS-roles.md)
- [了解桌面主機環境](Understanding-the-desktop-hosting-environment.md)  
- [Azure 服務與桌面主機的考量](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


