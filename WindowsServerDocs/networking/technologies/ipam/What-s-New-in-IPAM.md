---
title: IPAM 的新功能
description: 本主題說明 Windows Server 2016 中新增或變更的 IP 位址管理（IPAM）功能。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fc19a58482df5dfbfb4ea324f317bbe1b27bf834
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405594"
---
# <a name="whats-new-in-ipam"></a>IPAM 的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的 IP 位址管理（IPAM）功能。  
  
IPAM 為企業或雲端服務提供者（CSP）網路上的 IP 位址和 DNS 基礎結構提供高度可自訂的系統管理和監視功能。 您可以使用 IPAM 來監視、審查和管理執行動態主機設定通訊協定（DHCP）和網域名稱系統（DNS）的伺服器。  
  
## <a name="BKMK_IPAM2012R2"></a>IPAM 伺服器中的更新  
以下是 Windows Server 2016 中 IPAM 的全新和改良功能。  
  
|特色/功能|新功能或增強功能|描述|  
|--------------------------|-------------------|---------------|  
|[增強的 IP 位址管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|增強功能|IPAM 功能已針對處理 IPv4/32 和 IPv6/128 子網，以及尋找 IP 位址區塊中的可用 IP 位址子網和範圍等案例進行改良。|  
|[增強的 DNS 服務管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|新的|IPAM 可針對加入網域的 Active Directory 整合式和以檔案為基礎的 DNS 伺服器，支援 DNS 資源記錄、條件式轉寄站和 DNS 區域管理。|  
|[整合式 DNS、DHCP 和 IP 位址（DDI）管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|增強功能|已啟用數個新體驗和整合式生命週期管理作業，例如將所有與 IP 位址有關的 DNS 資源記錄視覺化、根據 DNS 資源記錄自動清查 IP 位址，以及 IP 位址生命週期管理適用于 DNS 和 DHCP 作業。|  
|[多個 Active Directory 樹系支援](#bkmk_ad)|新的|當安裝 IPAM 的樹系和每個遠端樹系之間有雙向信任關係時，您可以使用 IPAM 來管理多個 Active Directory 樹系的 DNS 和 DHCP 伺服器。|  
|[清除使用量資料](#bkmk_purge)|新的|您現在可以藉由清除早于指定日期的 IP 位址使用量資料，來減少 IPAM 資料庫大小。|  
|[以角色為基礎的存取控制的 Windows PowerShell 支援](#bkmk_ps)|新的|您可以使用 Windows PowerShell 來設定 IPAM 物件的存取範圍。|  
  
### <a name="EIP"></a>增強的 IP 位址管理  
下列功能會改善 IPAM 位址管理功能。  
>[!NOTE]
>如需 IPAM Windows PowerShell 命令參考，請參閱[Windows powershell 中的 IP 位址管理（IPAM）伺服器 Cmdlet](https://docs.microsoft.com/powershell/module/ipamserver/)。  
  
#### <a name="support-for-31-32-and-128-subnets"></a>支援/31、/32 和/128 子網  
Windows Server 2016 中的 IPAM 現在支援/31、/32 和/128 子網。 例如，交換器之間的點對點連結可能需要兩個位址子網（/31 IPv4）。 此外，某些交換器可能需要單一回送位址（/32 用於 IPv4，/128 用於 IPv6）。  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**尋找 IpamFreeSubnet 的免費子網**  
  
此命令會傳回可供配置的子網，並指定 IP 區塊、前置長度和要求的子網數目。   
  
如果可用的子網數目小於所要求的子網數目，則會傳回可用的子網，並出現警告，指出可用的數目小於所要求的數目。  
  
>[!NOTE]
>此函式不會實際配置子網，只會報告其可用性。 不過，Cmdlet 輸出可以透過管道傳送至**IpamSubnet**命令來建立子網。  
  
如需詳細資訊，請參閱[IpamFreeSubnet](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeSubnet)。  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**尋找 IpamFreeRange 的免費位址範圍**  
  
這個新命令會根據指定的 IP 子網、範圍內所需的位址數目，以及要求的範圍數，傳回可用的 IP 位址範圍。   
  
命令會搜尋連續系列的未配置 IP 位址，以符合要求的位址數目。 此程式會重複，直到找到要求的範圍數目為止，或直到沒有可用的位址範圍為止。  
  
> [!NOTE]
> 此函式不會實際配置範圍，只會報告其可用性。 不過，Cmdlet 輸出可以透過管道傳送至**IpamRange**命令來建立範圍。  
  
如需詳細資訊，請參閱[IpamFreeRange](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeRange)。  
  
### <a name="EDNS"></a>增強的 DNS 服務管理  
Windows Server 2016 中的 IPAM 現在支援在執行 IPAM 的 Active Directory 樹系中探索以檔案為基礎、已加入網域的 DNS 伺服器。  
  
此外，已新增下列 DNS 函式：  
  
-   從執行 Windows Server 2008 或更新版本的 DNS 伺服器，DNS 區域和資源記錄集合（而不是 DNSSEC 的相關）。  
  
-   設定（建立、修改和刪除）所有資源記錄類型的屬性和作業（而不是 DNSSEC 的相關資訊）。  
  
-   在所有類型的 DNS 區域（包括主要次要和存根區域）上設定（建立、修改、刪除）屬性和作業。  
  
-   在次要和存根區域上觸發的工作，不論它們是正向或反向對應區域。 例如，**從 Master 傳送**或**從 master 傳輸新的區域複本**之類的工作。  
  
-   支援的 DNS 設定（DNS 記錄和 DNS 區域）的角色型存取控制。  
  
-   條件式轉寄站集合和設定（[建立]、[刪除]、[編輯]）。  
  
### <a name="DDI"></a>整合式 DNS、DHCP 和 IP 位址（DDI）管理  
當您查看 IP 位址清查中的 IP 位址時，您可以在 [詳細資料] 視圖中選擇此選項，以查看與該 IP 位址相關聯的所有 DNS 資源記錄。  
  
做為 DNS 資源記錄集合的一部分，IPAM 會收集 DNS 反向查詢區域的 PTR 記錄。 針對對應至任何 IP 位址範圍的所有反向對應區域，IPAM 會在對應的對應 IP 位址範圍內，為屬於該區域的所有 PTR 記錄建立 IP 位址記錄。 如果 IP 位址已存在，PTR 記錄就會直接與該 IP 位址相關聯。 如果反向對應區域未對應至任何 IP 位址範圍，則不會自動建立 IP 位址。  
  
透過 IPAM 在反向對應區域中建立 PTR 記錄時，會以如上所述的相同方式更新 IP 位址清查。 在後續的集合中，由於 IP 位址已存在於系統中，因此 PTR 記錄只會與該 IP 位址對應。  
  
### <a name="bkmk_ad"></a>多個 Active Directory 樹系支援  
在 Windows Server 2012 R2 中，IPAM 能夠探索和管理屬於與 IPAM 伺服器相同 Active Directory 樹系的 DNS 和 DHCP 伺服器。 現在您可以管理屬於不同 AD 樹系的 DNS 和 DHCP 伺服器，與安裝 IPAM 伺服器的樹系具有雙向信任關係。 您可以移至 [**設定伺服器探索**] 對話方塊，並從您想要管理的其他受信任樹系新增網域。 探索到伺服器之後，管理體驗會與屬於安裝 IPAM 之相同樹系的伺服器相同。  
  
如需詳細資訊，請參閱[管理多個 Active Directory 樹系中的資源](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>清除使用量資料  
清除使用量資料可讓您藉由刪除舊的 IP 位址使用量資料，來減少 IPAM 資料庫大小。 若要執行資料刪除，您可以指定日期，而 IPAM 會刪除早于或等於您所提供日期的所有資料庫專案。   
  
如需詳細資訊，請參閱[清除使用量資料](../../technologies/ipam/Purge-Utilization-Data.md)。  
  
### <a name="bkmk_ps"></a>以角色為基礎的存取控制的 Windows PowerShell 支援  
您現在可以使用 Windows PowerShell 來設定以角色為基礎的存取控制。 您可以使用 Windows PowerShell 命令來取出 IPAM 中的 DNS 和 DHCP 物件，並變更其存取範圍。 因此，您可以撰寫 Windows PowerShell 腳本，將存取範圍指派給下列物件。  
  
-   IP 位址空間  
  
-   IP 位址區塊  
  
-   IP 位址子網  
  
-   IP 位址範圍  
  
-   DNS 伺服器  
  
-   DNS 區域  
  
-   DNS 條件轉寄站  
  
-   DNS 資源記錄  
  
-   DHCP 伺服器  
  
-   DHCP 超級領域  
  
-   DHCP 領域  
  
如需詳細資訊，請參閱[使用 Windows Powershell 管理角色型存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)和[windows Powershell 中的 IP 位址管理（IPAM）伺服器 Cmdlet](https://docs.microsoft.com/powershell/module/ipamserver/)。  

