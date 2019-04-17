---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: "簡化的管理附錄"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5de7431d0f3fe9a078432b11a63ce996d3abe447
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="simplified-administration-appendix"></a>簡化的管理附錄

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

  
-   [伺服器管理員中新增伺服器對話方塊 (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [伺服器管理員遠端伺服器狀態](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Windows PowerShell 模組載入](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [移除 Hotfix 發行的舊版作業系統](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe 安裝媒體的變更](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>伺服器管理員中新增伺服器對話方塊 (Active Directory)  

**新增伺服器]**對話方塊，讓搜尋 Active Directory 伺服器、 作業系統、 使用萬用字元，以及位置。 對話方塊也可使用 DNS 查詢完整的網域名稱或前置詞的名稱。 這些搜尋使用原生的 DNS 和 LDAP 通訊協定實作透過不 AD Windows PowerShell 針對 AD 管理閘道透過繽紛-表示您的網域控制站連絡由伺服器管理員中，甚至可執行 Windows Server 2003、.NET。 您也可以匯入檔案的伺服器來提供方便的名稱。  
  
下列 LDAP 篩選器會使用 Active Directory 搜尋：  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
Active Directory 搜尋傳回屬性下列動作：  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>伺服器管理員遠端伺服器狀態  
伺服器管理員會使用位址路由通訊協定遠端伺服器的協助工具測試。 未列出任何伺服器無法回應 ARP 要求，即使它們集區中。  
  
如果 ARP 看，然後 DCOM 和 WMI 是連接到伺服器返回狀態的資訊。 如果無法取得 RPC、 DCOM 和 WMI，伺服器管理員無法完全管理伺服器。  
  
## <a name="BKMK_PSLoadModule"></a>Windows PowerShell 模組載入  
Windows PowerShell 3.0 實作載入動態模組。 使用**模組匯入**cmdlet 不再通常需要;改為，只要叫用 cmdlet、 別名或函式自動載入模組。  
  
若要查看已載入的模組，使用**取得模組**cmdlet。  
  
```  
Get-Module  
  
```  
  
![簡化的管理](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
若要查看已安裝所有模組匯出函式與 cmdlet，使用：  
  
```  
Get-Module -ListAvailable  
  
```  
  
主要案例使用**模組匯入**命令是當您需要存取 「 AD: 「 Windows PowerShell virtual 磁碟機而已已經載入模組。 例如，使用下列命令：  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>移除 Hotfix 發行的舊版作業系統  
查看[更新已可偵測與避免太多消耗上執行的 Windows Server 2008 R2 網域控制站的全域 RID 集區的](https://support.microsoft.com/kb/2618669)。  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe 安裝媒體的變更  
Windows Server 2012 新增兩個額外選項 Ntdsutil.exe 命令列工具適用於**（IFM 媒體建立） IFM**功能表。 這些可讓您不需要先進行的匯出 NTDS 離線重組建立 IFM 存放區。DIT 資料庫檔案。 當磁碟空間不收費時，這可以節省時間建立 IFM。  
  
下表描述新的兩個功能表項目：  
  
|||  
|-|-|  
|功能表項目|解釋|  
|建立完整 NoDefrag %s|建立媒體 IFM 不需要重組完整 AD DC 或到資料夾 %s AD 日 LDS 執行個體|  
|建立 Sysvol 完整 NoDefrag %s|使用 SYSVOL 以及不完整 AD dc 重組到資料夾 %s 建立 IFM 媒體|  
  
![簡化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![簡化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


