---
title: 桌面主機參考架構
description: 使用 RDS 和 Azure 建立桌面主機解決方案架構的指導。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: c2bc0c2ba3d12ea1caf8737369ba882f69b111e4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80818451"
---
# <a name="desktop-hosting-reference-architecture"></a>桌面主機參考架構

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

本文定義一組使用遠端桌面服務 (RDS) 和 Microsoft Azure 虛擬機器來建立多租用戶、託管 Windows 桌面及應用程式服務的架構區塊，我們稱為「桌面主機」。 您可以使用此架構參考，為具有 5 到 5000 位使用者的中小型組織建立高度安全、可調整且可靠的桌面主機解決方案。    
  
此參考架構的主要對象為要透過 [Microsoft 服務提供者授權合約](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) (SPLA) 方案，運用 Microsoft Azure 基礎結構服務將桌面主機服務及訂戶存取授權 (SAL) 遞送給多個租用戶的主機提供者。 此參考架構的次要對象為終端客戶，這些客戶希望[透過軟體保證 (SA) 利用 RDS 使用者 CAL 擴充權限](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)，在 Microsoft Azure 基礎結構服務中為自己的員工建立並管理桌面主機解決方案。   
  
為了提供桌面主機解決方案，主機合作夥伴和 SA 客戶會運用 Windows Server 向 Windows 使用者提供與企業使用者及客戶相似的應用程式體驗。 Windows Server 2016 建置於 Windows 10 的基礎上，可提供熟悉的應用程式支援和使用者體驗。    
  
本文件的範圍僅限於：   
  
* 桌面主機服務的架構設計指導。 詳細資訊 (例如部署程序、效能和容量規劃) 將在個別的文件中說明。 如需更多 Azure 基礎結構服務的一般資訊，請參閱 [Microsoft Azure 虛擬機器](https://azure.microsoft.com/documentation/services/virtual-machines/)。   
  
* 使用 Windows Server 2016 遠端桌面工作階段主機 (RD 工作階段主機) 的工作階段型桌面、RemoteApp 應用程式和伺服器型個人桌面。 由於沒有適用於 Windows 用戶端作業系統的提供者授權合約 (SPLA)，因此未涵蓋 Windows 用戶端的虛擬桌面基礎結構。 SPLA 允許 Windows Server 型虛擬桌面基礎結構，在某些情況下，Windows 用戶端型虛擬桌面基礎結構允許用於具有終端客戶授權的專用硬體。 不過，用戶端型虛擬桌面基礎結構不在本文件的涵蓋範圍內。   
  
* Microsoft 產品和功能，主要為 Windows Server 2016 和 Microsoft Azure 基礎結構服務。   
  
* 租用戶的桌面主機服務規模範圍為 5 到 5000 位使用者。   對於較大的租用戶，您可能需要修改此架構以提供足夠的效能。 不建議將伺服器管理員 RDS 圖形化使用者介面 (GUI) 用於超過 500 位使用者的部署。 建議將 PowerShell 用於管理介於 500 到 5000 位使用者的 RDS 部署。   
  
* 桌面主機服務所需的最小元件和服務集合。 有許多可新增的選用元件和服務可用於增強桌面主機服務，但這些元件和服務不在本文件的涵蓋範圍內。    
  
閱讀本文件之後，讀者應該了解：   
- 為了在 Microsoft Azure 服務中提供安全可靠的多租用戶桌面主機解決方案，建置組塊是必須的。  
- 每個建置組塊的用途以及如何相互配合。  
  
有多種方法可以根據此架構建置桌面主機解決方案。 此架構概述 Azure 與 Windows Server 2016 的整合與改善。 Windows Server 2012 R2 的[桌面主機參考架構指南](https://go.microsoft.com/fwlink/p/?LinkId=517389)提供其他部署選項。    
  
涵蓋下列主題：  
- [桌面主機邏輯架構](Desktop-hosting-logical-architecture.md)  
- [了解 RDS 角色](Understanding-RDS-roles.md)
- [了解桌面主機環境](Understanding-the-desktop-hosting-environment.md)  
- [桌面代管的 Azure 服務與考量](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


