---
title: 建立遠端桌面的虛擬機器
description: 建立 Vm 來裝載在雲端中的遠端桌面元件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: 5c61d9f08cb799d6a63a004bedab924a6ba37fdc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446734"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>建立遠端桌面的虛擬機器

>適用於：Windows Server （半年通道），Windows Server 2019，Windows Server 2016

若要將用來執行 Windows Server 2016 角色、 服務和功能的桌面主機部署所需的租用戶的環境中建立虛擬機器中使用下列步驟。   
  
此範例的基本部署中，將會建立 3 部虛擬機器的最小值。 「 遠端桌面 (RD) 連線代理人和授權伺服器角色服務和部署檔案共用，將會裝載一部虛擬機器。 第二個虛擬機器會裝載 「 RD 閘道和 Web 存取角色服務。  第三個虛擬機器主機的 RD 工作階段主機角色服務。 對於非常小型的部署，您可以使用 AAD 應用程式 Proxy 來排除所有的公用端點，從部署，並結合到單一 VM 上的所有角色服務，以減少 VM 成本。 若是大型部署，您可以允許更佳的調整個別的虛擬機器上安裝各種角色服務。  
  
本節將概述部署每個角色中的 Windows Server 映像為基礎的虛擬機器所需的步驟[Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/)。 如果您需要建立虛擬機器從自訂映像，必須使用 PowerShell，請參閱[使用 Resource Manager 和 PowerShell 建立 Windows VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/)。 然後返回此處以附加檔案共用的 Azure 資料磁碟，並輸入外部 URL 為您的部署。  
  
1. [建立 Windows 虛擬機器](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/)來裝載 RD 連線代理人、 RD 授權伺服器，以及檔案伺服器。  
  
   我們的目的，我們會使用下列命名慣例：  
   - RD 連線代理人、 授權伺服器，以及檔案伺服器：   
       - VM:Contoso-Cb1  
       - 可用性設定組：CbAvSet    
   - RD Web 存取和 RD 閘道伺服器：   
       - VM:Contoso-WebGw1  
       - 可用性設定組：WebGwAvSet  
          
   - RD 工作階段主機：   
       - VM:Contoso-Sh1  
       - 可用性設定組：ShAvSet  
          
   每個 VM 會使用相同的資源群組。  
2. 建立 Azure 資料磁碟並將使用者設定檔磁碟 (UPD) 共用：  
   1.  在 Azure 入口網站中按一下**瀏覽 > 資源群組**，按一下 部署的資源群組，然後按一下 針對 RD 連線代理人 (例如，Contoso-Cb1) 所建立的 VM。  
   2.  按一下 **設定 > 磁碟 > 連結新**。  
   3.  接受預設的名稱和型別。  
   4.  輸入的大小足以容納租用戶的環境，包括使用者設定檔磁碟和憑證的網路共用 （以 gb 為單位）。 您可以在每個使用者計劃讓近似 5 GB  
   5.  接受位置和快取主機的預設值，然後按**確定**。  
3. 建立外部負載平衡器存取外部部署：
   1. 在 Azure 入口網站中按一下**瀏覽 > 負載平衡器**，然後按一下**新增**。
   2. 請輸入**名稱**，選取**公用**做為**型別**的負載平衡器，並選取適當**訂用帳戶**， **資源群組**，並**位置**。
   3. 選取 **選擇公用 IP 位址**，**新建**，輸入名稱，然後選取**Ok**。
   4. 選取 **建立**建立負載平衡器。
4. 設定外部負載平衡器，為您的部署
   1. 在 Azure 入口網站中按一下**瀏覽 > 資源群組**，按一下 部署的資源群組，然後按一下 負載平衡器，您建立部署。
   2. 新增傳送出流量的負載平衡器後端集區：
       1. 選取 **後端集區**並**新增**。
       2. 請輸入**名稱**，然後選取 **\+新增虛擬機器**。
       3. 選取 **可用性設定組**並**WebGwAvSet**。
       4. 選取**虛擬機器**， **Contoso WebGw1**，**選取**，**確定**，並**確定**。
   3. 新增探查，讓負載平衡器可讓您知道哪些機器正在使用：
       1. 選取 **探查**並**新增**。
       2. 請輸入**名稱**（例如 HTTPS)，選取**TCP**，輸入**連接埠**443，然後選取**確定**。
   4. 輸入負載平衡規則，以平衡的連入流量：
      1. 選取 **負載平衡規則**和**新增**
      2. 輸入**名稱**（例如 HTTPS)，選取**TCP**，和 443，同時**連接埠**而**後端連接埠**。
          - 如需 Windows 10 和 Windows Server 2016 部署中，保留**工作階段持續性**作為**無**，否則請選取**用戶端 IP**。
      3. 選取 **確定**接受 HTTPS 規則。
      4. 選取建立新的規則**新增**。
      5. 輸入**名稱**（例如 UDP) 」，選取**UDP**，並同時 3391<strong>連接埠和 * * 後端連接埠</strong>。
          - Windows 10 和 Windows Server 2016 的部署，保留**工作階段持續性**作為**無**，否則請選取**用戶端 IP**。
      6. 選取 **確定**接受 UDP 規則。
   5. 輸入以直接連線到 Contoso WebGw1 的輸入的 NAT 規則
       1. 選取 **輸入 NAT 規則**並**新增**。
       2. 輸入**名稱**（例如 RDP-Contoso-WebGw1)，選取**Customm**服務**TCP**通訊協定，並輸入如 14000**連接埠**。
       3. 選取 **選擇虛擬機器**和 Contoso WebGw1。
       4. 選取**自訂**連接埠對應，請輸入為 3389**目標連接埠**，然後選取**確定**。
5. 輸入您的部署在外部存取的外部 URL/DNS 名稱：  
   1.  在 Azure 入口網站中，按一下**瀏覽 > 資源群組**，按一下 部署的資源群組，然後按一下 建立 RD Web 存取和 RD 閘道的公用 IP 位址。  
   2.  按一下 **組態**，輸入 （例如 contoso) 的 DNS 名稱標籤，然後按一下**儲存**。 此 DNS 名稱標籤 (contoso.westus.cloudapp.azure.com) 是您要用來連線到 RD Web 存取和 RD 閘道伺服器的 DNS 名稱。  

