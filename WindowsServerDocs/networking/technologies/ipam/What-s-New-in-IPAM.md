---
title: IPAM 的新功能
description: 本主題說明 Windows Server 2016 中新增或變更的 IP 位址管理 (IPAM) 功能。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04838cba63805d20ba31629ed9c8e95290046320
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624682"
---
# <a name="whats-new-in-ipam"></a>IPAM 的新功能

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的 IP 位址管理 (IPAM) 功能。  
  
IPAM 企業網路或雲端服務提供者 (CSP) 網路上提供高度可自訂的管理與監控功能的 IP 位址和 DNS 基礎結構。 您可以監視、 稽核，以及管理執行動態主機設定通訊協定 (DHCP) 和網域名稱系統 (DNS) 使用 IPAM 伺服器。  
  
## <a name="BKMK_IPAM2012R2"></a>在 IPAM 伺服器的更新  
以下是 IPAM 的 Windows Server 2016 的全新和改進功能。  
  
|特色/功能|新功能或增強功能|描述|  
|--------------------------|-------------------|---------------|  
|[增強的 IP 位址管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|增強功能|IPAM 功能會改進處理 IPv4/32 和 IPv6/128 子網路和 IP 位址區塊中尋找可用的 IP 位址子網路和範圍等案例。|  
|[增強的 DNS 服務管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|新的|IPAM 支援這兩個已加入網域的 Active Directory 整合和支援檔案的 DNS 伺服器的 DNS 資源記錄、 條件式轉寄站和 DNS 區域管理。|  
|[整合式 DNS、 DHCP 和 IP 位址 (DDI) 管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|增強功能|有多項新體驗並啟用整合式生命週期管理作業，例如視覺化所有的 DNS 資源記錄 IP 的相關位址，根據 DNS 資源記錄，以及 IP 位址週期管理的 IP 位址的自動化的清查DNS 和 DHCP 的作業。|  
|[多個 Active Directory 樹系支援](#bkmk_ad)|新的|您可以使用 IPAM 來管理多個 Active Directory 樹系的 DNS 和 DHCP 伺服器，IPAM 安裝所在的樹系和每個遠端樹系之間的雙向信任關係時。|  
|[清除使用資料](#bkmk_purge)|新的|您現在可以清除早於您所指定的日期的 IP 位址使用率資料，以減少 IPAM 資料庫的大小。|  
|[Windows PowerShell 支援的角色型存取控制](#bkmk_ps)|新的|您可以使用 Windows PowerShell IPAM 物件上設定存取領域。|  
  
### <a name="EIP"></a>增強的 IP 位址管理  
下列功能改善 IPAM 位址的管理功能。  
>[!NOTE]
>如需 IPAM 的 Windows PowerShell 命令參考，請參閱[Windows PowerShell 中的 IP 位址管理 (IPAM) 伺服器 Cmdlet](https://docs.microsoft.com/en-us/powershell/module/ipamserver/)。  
  
#### <a name="support-for-31-32-and-128-subnets"></a>支援/31/32，，/128 用於子網路  
Windows Server 2016 現在支援/31/32，，/128 子網路中的 IPAM。 例如，兩個位址的子網路 (/ 31 IPv4) 可能需要交換器之間的點對點連結。 此外，某些參數可能需要單一的回送位址 (/ 32 用於 IPv4，/128 用於 IPv6)。  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**尋找與尋找 IpamFreeSubnet 可用子網路**  
  
此命令會傳回可供配置的子網路指定 IP 區塊、 前置長度及要求的子網路數目。   
  
可用的子網路數目小於所要求的子網路數目，如果可用的子網路會傳回警告，指出可用的數目少於所要求的數目。  
  
>[!NOTE]
>此函式不會實際配置的子網路，它只會報告其可用性。 不過，cmdlet 輸出可以輸送到**新增 IpamSubnet**命令來建立子網路。  
  
如需詳細資訊，請參閱 <<c0> [ 尋找 IpamFreeSubnet](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeSubnet)。  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**尋找與尋找 IpamFreeRange 可用的位址範圍**  
  
此新命令傳回提供指定 IP 位址範圍的 IP 子網路、 在範圍內，所需的位址數目和所要求的範圍數目。   
  
此命令會搜尋符合要求的位址數目的未配置 IP 位址的連續序列。 程序會重複，直到找到要求的範圍數目，或直到沒有其他可用位址範圍可用。  
  
> [!NOTE]
> 此函式不會實際配置的範圍，它只會報告其可用性。 不過，cmdlet 輸出可以輸送到**新增 IpamRange**命令來建立範圍。  
  
如需詳細資訊，請參閱 <<c0> [ 尋找 IpamFreeRange](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeRange)。  
  
### <a name="EDNS"></a>增強的 DNS 服務管理  
Windows Server 2016 中的 IPAM 現在支援以檔案為基礎，且已加入網域的 DNS 伺服器的探索，IPAM 執行所在的 Active Directory 樹系中。  
  
此外，已新增下列 DNS 功能：  
  
-   DNS 區域和資源記錄 （非 DNSSEC 相關） 的集合，從執行 Windows Server 2008 或更新版本的 DNS 伺服器。  
  
-   設定 （建立、 修改及刪除） 的屬性和作業在所有類型的資源記錄 （非 DNSSEC 相關）。  
  
-   設定 （建立、 修改及刪除） 的屬性和作業，包括主要次要和虛設常式區域的 DNS 區域的所有型別上)。  
  
-   如果他們正向或反向對應區域時，觸發在次要複本上的工作和虛設常式區域，無論。 比方說，這類工作**從主機傳送**或是**從主要傳輸的區域的新複本**。  
  
-   支援的 DNS 設定 （DNS 記錄和 DNS 區域） 的角色型存取控制。  
  
-   條件式轉寄站集合及組態 （建立、 刪除、 編輯）。  
  
### <a name="DDI"></a>整合式 DNS、 DHCP 和 IP 位址 (DDI) 管理  
當您在 IP 位址清查檢視 IP 位址時，您可以選擇在詳細資料檢視中看到 IP 位址相關聯的所有 DNS 資源記錄。  
  
DNS 資源記錄集合的一部分，IPAM 會收集 DNS 反向查閱區域的 PTR 記錄。 所有的反向對應區域是對應至任何 IP 位址範圍，IPAM 會建立屬於該區域中對應的對應 IP 位址範圍中的 PTR 記錄的 IP 位址記錄。 如果已經存在的 IP 位址，PTR 記錄是直接相關聯的 IP 位址。 如果反向對應區域未對應到任何 IP 位址範圍，不會自動建立的 IP 位址。  
  
透過 IPAM 反向對應區域中建立 PTR 記錄時，IP 位址清查會更新相同的方式，如上面所述。 後續在收集期間，因為 IP 位址會存在於系統中，則將會與該 IP 位址直接對應的 PTR 記錄。  
  
### <a name="bkmk_ad"></a>多個 Active Directory 樹系支援  
Windows Server 2012 r2 中 IPAM 無法探索及管理屬於與 IPAM 伺服器相同的 Active Directory 樹系的 DNS 和 DHCP 伺服器。 您現在可以管理 DNS 和 DHCP 伺服器屬於不同的 AD 樹系具有雙向信任關係與 IPAM 伺服器安裝所在之樹系時。 您可以前往**設定伺服器探索**對話方塊方塊中，並新增來自其他網域信任您想要管理的樹系。 會在探索到的伺服器之後，管理經驗都是屬於相同樹系安裝 IPAM 伺服器相同。  
  
如需詳細資訊，請參閱[多個 Active Directory 樹系中的 管理資源](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>清除使用資料  
清除使用率資料可讓您刪除舊的 IP 位址使用率資料來減少 IPAM 資料庫的大小。 若要執行資料刪除，您可以指定日期，和 IPAM 刪除早於的所有資料庫項目，或您提供的日期。   
  
如需詳細資訊，請參閱 <<c0> [ 清除使用率資料](../../technologies/ipam/Purge-Utilization-Data.md)。  
  
### <a name="bkmk_ps"></a>Windows PowerShell 支援的角色型存取控制  
您現在可以使用 Windows PowerShell 來設定角色型存取控制。 您可以使用 Windows PowerShell 命令來擷取在 IPAM 中的 DNS 和 DHCP 物件，並變更其存取範圍。 基於這個原因，您可以撰寫 Windows PowerShell 指令碼，以指派至下列物件的存取範圍。  
  
-   IP 位址空間  
  
-   IP 位址區塊  
  
-   IP 位址子網路  
  
-   IP 位址範圍  
  
-   DNS 伺服器  
  
-   DNS 區域  
  
-   DNS 條件轉寄站  
  
-   DNS 資源記錄  
  
-   DHCP 伺服器  
  
-   DHCP 超級領域  
  
-   DHCP 領域  
  
如需詳細資訊，請參閱 <<c0> [ 管理角色型存取控制使用 Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)並[Windows PowerShell 中的 IP 位址管理 (IPAM) 伺服器 Cmdlet](https://docs.microsoft.com/en-us/powershell/module/ipamserver/)。  

