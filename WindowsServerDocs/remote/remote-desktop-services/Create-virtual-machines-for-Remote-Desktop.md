---
title: 建立遠端桌面的虛擬機器
description: 建立 VM 來裝載雲端中的遠端桌面元件。
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: 864b1a0fda39570a94007108e816c7a9b2cddd51
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939689"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>建立遠端桌面的虛擬機器

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

使用下列步驟，在租用戶的環境中建立虛擬機器，以用來執行桌面裝載部署所需的 Windows Server 2016 角色、服務和功能。

針對這個基本部署範例，將建立 3 部虛擬機器 (最小值)。 一部虛擬機器將裝載遠端桌面 (RD) 連線代理人和授權伺服器角色服務及檔案共用以進行部署。 第二部虛擬機器將裝載 RD 閘道和 Web 存取角色服務。  第三部虛擬機器將裝載 RD 工作階段主機角色服務。 對於非常小型的部署，您可以使用 AAD 應用程式 Proxy，從部署中排除所有公用端點，並將所有角色服務結合到單一 VM 上，藉以降低 VM 成本。 針對較大型部署，您可以在個別虛擬機器上安裝各種角色服務，以允許進行更好的調整。

本節將概述根據 [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/) 中的 Windows Server 映像，針對每個角色部署虛擬機器所需的步驟。 如果您需要從自訂映像 (需要 PowerShell) 建立虛擬機器，請參閱[使用 Resource Manager 和 PowerShell 建立 Windows VM](/azure/virtual-machines/windows/quick-create-powershell)。 然後返回此處來連結用於檔案共用的 Azure 資料磁碟，並為您的部署輸入外部 URL 。

1. [建立 Windows 虛擬機器](/azure/virtual-machines/windows/quick-create-portal)來裝載 RD 連線代理人、RD 授權伺服器及檔案伺服器。

   針對我們的目的，我們將使用下列命名慣例：
   - RD 連線代理人、授權伺服器及檔案伺服器：
       - VM：Contoso-Cb1
       - 可用性設定組：CbAvSet
   - RD Web 存取和 RD 閘道伺服器：
       - VM：Contoso-WebGw1
       - 可用性設定組：WebGwAvSet

   - RD 工作階段主機：
       - VM：Contoso-Sh1
       - 可用性設定組：ShAvSet

   每部 VM 都會使用相同的資源群組。
2. 針對使用者設定檔磁碟 (UPD) 共用建立並連結 Azure 資料磁碟：
   1.  在 Azure 入口網站中，按一下 [瀏覽] > [資源群組]  、按一下用於部署的資源群組，然後按一下針對 RD 連線代理人 (例如 Contoso-Cb1) 建立的 VM。
   2.  按一下 [設定] > [磁碟] > [連結新裝置]  。
   3.  接受名稱和類型的預設值。
   4.  針對租用戶的環境輸入足以容納網路共用的大小 (以 GB 為單位)，其中包括使用者設定檔磁碟和憑證。 您計畫讓每位使用者大約能夠使用 5 GB
   5.  接受位置和快取主機的預設值，然後按一下 [確定]  。
3. 建立外部負載平衡器以從外部存取部署：
   1. 在 Azure 入口網站中，按一下 [瀏覽] > [負載平衡器]  ，然後按一下 [新增]  。
   2. 輸入**名稱**、選取 [公用]  作為負載平衡器的**類型**，然後選取適當的**訂用帳戶**、**資源群組**和**位置**。
   3. 選取 [選擇公用 IP 位址]  、[新建]  、輸入名稱，然後選取 [確定]  。
   4. 選取 [建立]  以建立負載平衡器。
4. 為您的部署設定外部負載平衡器
   1. 在 Azure 入口網站中，按一下 [瀏覽] > [資源群組]  、按一下用於部署的資源群組，然後按一下針對部署建立的負載平衡器。
   2. 新增負載平衡器的後端集區，以將流量傳送至其中：
       1. 依序選取 [後端集區]  和 [新增]  。
       2. 輸入**名稱**，然後選取 [\+ 新增虛擬機器]  。
       3. 依序選取 [可用性設定組]  和 [WebGwAvSet]  。
       4. 依序選取 [虛擬機器]  、[Contoso-WebGw1]  、[選取]  、[確定]  及 [確定]  。
   3. 新增探查，讓負載平衡器知道哪些機器處於使用中狀態：
       1. 依序選取 [探查]  和 [新增]  。
       2. 輸入**名稱** (例如 HTTPS)、選取 [TCP]  、輸入**連接埠** 443，然後選取 [確定]  。
   4. 輸入負載平衡規則以平衡連入流量：
      1. 依序選取 [負載平衡規則]  和 [新增]  。
      2. 輸入**名稱** (例如 HTTPS)、選取 [TCP]  ，然後針對**連接埠**和**後端連接埠**輸入 443。
          - 針對 Windows 10 和 Windows Server 2016 部署，將 [工作階段持續性]  保留為 [無]  ，否則選取 [用戶端 IP]  。
      3. 選取 [確定]  以接受 HTTPS 規則。
      4. 選取 [新增]  以建立新的規則。
      5. 輸入**名稱** (例如 UDP)、選取 [UDP]  ，然後針對<strong>連接埠和**後端連接埠</strong>輸入 3391。
          - 針對 Windows 10 和 Windows Server 2016 部署，將 [工作階段持續性]  保留為 [無]  ，否則選取 [用戶端 IP]  。
      6. 選取 [確定]  以接受 UDP 規則。
   5. 輸入一個輸入 NAT 規則以直接連線到 Contoso-WebGw1
       1. 依序選取 [輸入 NAT 規則]  和 [新增]  。
       2. 輸入**名稱** (例如 RDP-Contoso-WebGw1)、針對服務選取 [自訂]  、針對通訊協定選取 [TCP]  ，然後針對**連接埠**輸入 14000。
       3. 依序選取 [選擇虛擬機器]  和 [Contoso-WebGw1]。
       4. 針對連接埠對應選取 [自訂]  、針對**目標連接埠**輸入 3389，然後選取 [確定]  。
5. 針對部署輸入外部 URL/DNS 名稱，以便在外部存取它：
   1.  在 Azure 入口網站中，按一下 [瀏覽] > [資源群組]  、按一下用於部署的資源群組，然後按一下針對 RD Web 存取和 RD 閘道建立的公用 IP 位址。
   2.  按一下 [設定]  、輸入 DNS 名稱標籤 (例如 contoso)，然後按一下 [儲存]  。 此 DNS 名稱標籤 (contoso.westus.cloudapp.azure.com) 是您要用來連線到 RD Web 存取和 RD 閘道伺服器的 DNS 名稱。
