---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: 簡化的系統管理附錄
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 36cdacec27e64586c359146b858a9d68750e5026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858259"
---
# <a name="simplified-administration-appendix"></a>簡化的系統管理附錄

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

  
-   [伺服器管理員加入伺服器 對話方塊中 (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [伺服器管理員遠端伺服器狀態](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Windows PowerShell 模組載入](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [舊版作業系統的 RID 發行的 Hotfix](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe Install from Media Changes](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>伺服器管理員加入伺服器 對話方塊中 (Active Directory)  

**新增伺服器**對話方塊可讓搜尋 Active Directory 伺服器作業系統，使用萬用字元，以及位置。 此對話方塊也可讓使用完整的網域名稱或前置詞名稱的 DNS 查詢。 這些搜尋會使用原生的 DNS 和 LDAP 通訊協定透過.NET，不針對透過 SOAP-這表示網域控制站連絡伺服器管理員甚至可以執行 Windows Server 2003 AD Management Gateway AD Windows PowerShell 實作。 您也可以匯入伺服器名稱的佈建用途的檔案。  
  
Active Directory 搜尋會使用下列的 LDAP 篩選器：  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
Active Directory 搜尋會傳回下列屬性：  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>伺服器管理員遠端伺服器狀態  
伺服器管理員測試使用位址路由通訊協定的遠端伺服器存取範圍。 未列出任何伺服器沒有回應 ARP 要求，即使它們是集區中。  
  
如果 ARP 回應，然後 DCOM 和 WMI 連線會對伺服器傳回狀態資訊。 如果無法連線到 RPC、 DCOM 和 WMI，伺服器管理員無法完整管理的伺服器。  
  
## <a name="BKMK_PSLoadModule"></a>Windows PowerShell 模組載入  
Windows PowerShell 3.0 會實作動態載入的模組。 使用**Import-module** cmdlet 通常是不再需要; 相反地，只需叫用 cmdlet、 別名或函式會自動載入的模組。  
  
若要查看已載入的模組，請使用**Get-module** cmdlet。  
  
```  
Get-Module  
  
```  
  
![簡化的管理](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
若要查看所有已安裝的模組匯出的函式與指令程式，請使用：  
  
```  
Get-Module -ListAvailable  
  
```  
  
主要使用案例**匯入模組**命令是當您需要存取 「 AD: 」Windows PowerShell 虛擬磁碟機，而無其他內容已載入模組。 例如，使用下列命令：  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>舊版作業系統的 RID 發行的 Hotfix  
請參閱[有可用來偵測並防止過度消耗全域的 RID 集區，在執行 Windows Server 2008 R2 的網域控制站上的更新](https://support.microsoft.com/kb/2618669)。  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe Install from Media Changes  
Windows Server 2012 的 Ntdsutil.exe 命令列工具來新增兩個其他選項**IFM （IFM 媒體建立）** 功能表。 這些可讓您建立 IFM 存放區，而不需要先執行匯出的 NTDS 離線磁碟重組。DIT 資料庫檔案。 當磁碟空間不是進階時，這樣可以節省建立 IFM 的時間。  
  
下表描述兩個新的功能表項目：  
  
|||  
|-|-|  
|功能表項目|說明|  
|建立完整 NoDefrag %s|建立不含 AD 的完整 DC 或 AD LDS/執行個體到資料夾 %s 重組的 IFM 媒體|  
|建立 Sysvol 全文 NoDefrag %s|建立 IFM 媒體包含 SYSVOL 和不含資料夾 %s 重組針對完整的 AD 網域控制站|  
  
![簡化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![簡化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


