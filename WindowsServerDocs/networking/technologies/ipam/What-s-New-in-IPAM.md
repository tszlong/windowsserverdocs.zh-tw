---
title: IPAM 中的新功能
description: 本主題描述新增或變更 Windows Server 2016 中的 IP 位址管理 (IPAM) 功能。
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
ms.openlocfilehash: b90cd1ab223e38cbf5933b58a594b32d5e3d4858
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ipam"></a>IPAM 中的新功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題描述新增或變更 Windows Server 2016 中的 IP 位址管理 (IPAM) 功能。  
  
IPAM 提供企業版或雲端服務提供者 (CSP) 的網路的 IP 位址和 DNS 基礎結構高度自訂管理及監視功能。 您可以監視、 稽核，及管理執行動態主機設定通訊協定 」 (DHCP) 和網域名稱系統 」 (DNS) 使用 IPAM 伺服器。  
  
## <a name="BKMK_IPAM2012R2"></a>更新 IPAM 伺服器  
以下是在 Windows Server 2016 IPAM 新的和已改進功能。  
  
|功能日功能|新的或改進|描述|  
|--------------------------|-------------------|---------------|  
|[美化的 IP 位址管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|已改進|IPAM 功能的改良處理 IPv4/32 和 IPv6 /128 子網路和 IP 位址封鎖中尋找免費 IP 位址子網路和範圍案例。|  
|[美化的 DNS 服務管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|新|IPAM 支援 DNS 資源記錄、 條件轉寄，以及 DNS 區域管理這兩個加入網域的 Active Directory 整合和檔案備份 DNS 伺服器。|  
|[整合 DNS、 DHCP 及 IP 位址管理 (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|已改進|幾個新體驗與整合式開發週期管理支援作業，例如視覺化所有 DNS 資源記錄屬於 IP 位址，自動清單的基礎 DNS 資源記錄及 IP 位址週期管理 DNS] 和 [DHCP 作業的 IP 位址。|  
|[多個 Active Directory 樹系支援](#bkmk_ad)|新|您可以使用 IPAM 雙向信任關係樹系安裝 IPAM，與每個遠端森林之間時管理多個 Active Directory 樹系的 DNS 及 DHCP 伺服器。|  
|[清除資料使用量](#bkmk_purge)|新|您現在可以減少 IPAM 資料庫大小清除指定日期，比舊的 IP 位址使用量資料。|  
|[Windows PowerShell 角色根據存取控制支援](#bkmk_ps)|新|您可以使用 Windows PowerShell 來設定 IPAM 物件存取範圍。|  
  
### <a name="EIP"></a>美化的 IP 位址管理  
下列功能改善 IPAM 位址管理功能。  
>[!NOTE]
>針對 IPAM Windows PowerShell 命令參考資料，請查看[Windows PowerShell 中的 IP 位址管理 (IPAM) 伺服器 Cmdlet](https://technet.microsoft.com/library/jj553807.aspx)。  
  
#### <a name="support-for-31-32-and-128-subnets"></a>支援 /31、 / 32，以及 /128 子網路  
Windows Server 2016 現在支援 /31、 / 32，以及 /128 子網路中 IPAM。 例如，有兩個位址子網路 (月 31 IPv4) 可能需要點對點連結之間切換。 此外，部分參數可能需要單一回送地址 (/ 32 ipv4，/128 ipv6)。  
  
#### **<a name="find-free-subnets-with-find-ipamfreesubnet"></a>尋找與尋找-IpamFreeSubnet 免費子網路**  
  
這個命令傳回子網路所使用的配置，指定 IP 封鎖、 首碼長度] 和要求子網路中的數字。   
  
如果提供子網路的數目要求子網路，提供子網路的傳回警告，表示可以使用數字小於要求的號碼。  
  
>[!NOTE]
>這項功能會確實配置子網路，只會報告他們的可用性。 不過，可 cmdlet 輸出傳送到**新增-IpamSubnet**來建立子網路的命令。  
  
如需詳細資訊，請查看[尋找-IpamFreeSubnet](https://technet.microsoft.com/library/mt712782.aspx)。  
  
#### **<a name="find-free-address-ranges-with-find-ipamfreerange"></a>尋找與尋找-IpamFreeRange 免費地址範圍**  
  
這個新命令退貨可用的 IP 位址範圍提供的 IP 子網路的範圍中所需的地址，範圍要求的數目。   
  
命令搜尋連續一連串尚未配置的 IP 位址符合要求位址數目。 重複此程序直到找到所要求的範圍數目，或有更多可用之前範圍提供地址。  
  
> [!NOTE]
> 這項功能會確實配置範圍，只會報告他們的可用性。 不過，可 cmdlet 輸出傳送到**新增-IpamRange**命令來建立範圍。  
  
如需詳細資訊，請查看[尋找-IpamFreeRange](https://technet.microsoft.com/library/mt712772.aspx)。  
  
### <a name="EDNS"></a>美化的 DNS 服務管理  
IPAM 在 Windows Server 2016 現在支援檔案、 加入網域的 DNS 伺服器的探索 IPAM 正在 Active Directory 森林中。  
  
此外，已新增下列的 DNS 功能：  
  
-   DNS 區域和資源記錄 （以外這些屬於 DNSSEC） 集合從執行 Windows Server 2008，或更新版本的 DNS 伺服器。  
  
-   設定 （建立、 修改和 delete） 屬性和所有類型的資源記錄 （以外這些屬於 DNSSEC） 作業。  
  
-   設定 （建立修改、 delete） 屬性和作業所有類型的 DNS 區域包括主要次要，以及 Stub 區域)。  
  
-   如果會觸發在次要工作和 stub 區域，不論是往前或反向對應區域。 例如，例如工作**從主機傳送]**或**從主機傳送全新區域的**。  
  
-   角色為基礎存取控制 （DNS 記錄和 DNS 區域） 的支援 DNS 設定。  
  
-   條件轉送程式集合和設定建立、 delete (編輯）。  
  
### <a name="DDI"></a>整合 DNS、 DHCP 及 IP 位址管理 (DDI)  
當您在 [IP 位址庫存檢視 IP 位址時，您可以選擇在詳細資料檢視查看所有 DNS 資源關聯的 IP 位址。  
  
部分 DNS 資源記錄收集為 IPAM 所收集 DNS 反向查詢 PTR 的記錄。 所有反向對應的對應至任何 ip，IPAM 建立 IP 位址記錄所有 PTR 記錄屬於對應對應 IP 位址有時候您附近的區域。 如果已經的 IP 位址，PTR 記錄是只要關聯的 IP 位址。 如果不到任何 ip 對應反向對應區域，不會自動建立的 IP 位址。  
  
透過 IPAM 反向對應區域中，會建立 PTR 筆資料時, IP 位址會在更新清單相同的方式如上文所述。 在後續的收藏，因為您的 IP 位址存在將會在系統中，PTR 記錄會只要對應的 IP 位址。  
  
### <a name="bkmk_ad"></a>多個 Active Directory 樹系支援  
Windows Server 2012 R2 IPAM 是無法探索及管理 DNS 及 DHCP 伺服器屬於相同 Active Directory 樹系 IPAM 伺服器。 現在您可以管理 DNS 及 DHCP 伺服器不同的廣告森林屬於雙向信任關係的樹系安裝 IPAM 伺服器的位置時。 您可以移至**設定伺服器探索**對話方塊中，並新增從其他信任的樹系您想要管理。 伺服器會發現之後，管理經驗之後提供相同的樹系會安裝 IPAM 位置屬於伺服器一樣。  
  
如需詳細資訊，請查看[多 Active Directory 森林中的 [管理資源](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>清除資料使用量  
清除使用量資料可讓您以減少 IPAM 資料庫大小低於舊 IP 位址使用量。 若要執行刪除資料，您可以指定日期，以及 IPAM 刪除所有資料庫項目，比舊或您提供的日期。   
  
如需詳細資訊，請查看[清除使用量資料](../../technologies/ipam/Purge-Utilization-Data.md)。  
  
### <a name="bkmk_ps"></a>Windows PowerShell 角色根據存取控制支援  
您現在可以使用 Windows PowerShell 來設定為存取控制角色。 您可以使用 Windows PowerShell 命令擷取中 IPAM DNS 和 DHCP 物件並變更其存取權的範圍。 因此，您就可以撰寫存取領域給下列物件的 Windows PowerShell 指令碼。  
  
-   IP 位址空間  
  
-   IP 位址封鎖  
  
-   IP 位址子網路  
  
-   IP 位址範圍  
  
-   DNS 伺服器  
  
-   DNS 區域  
  
-   DNS 條件轉送程式  
  
-   DNS 資源記錄  
  
-   DHCP 伺服器  
  
-   超級 DHCP 領域  
  
-   DHCP 領域  
  
如需詳細資訊，請查看[管理角色根據存取控制使用 Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)和[Windows PowerShell 中的 IP 位址管理 (IPAM) 伺服器 Cmdlet](https://technet.microsoft.com/library/jj553807.aspx)。  

