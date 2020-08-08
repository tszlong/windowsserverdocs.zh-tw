---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: 簡化的系統管理附錄
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3cef76347bcd5a341b96a71fed58d2fe0085a46c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953404"
---
# <a name="simplified-administration-appendix"></a>簡化的系統管理附錄

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

-   [伺服器管理員新增伺服器] 對話方塊 (Active Directory) ](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)

-   [伺服器管理員遠端伺服器狀態](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)

-   [Windows PowerShell 模組載入](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)

-   [舊版作業系統的 RID 發行修補程式](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)

-   [Ntdsutil.exe 從媒體變更安裝](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)

## <a name="server-manager-add-servers-dialog-active-directory"></a><a name="BKMK_AddServers"></a>伺服器管理員新增伺服器] 對話方塊 (Active Directory) 

[**新增伺服器**] 對話方塊可讓您搜尋伺服器、作業系統、使用萬用字元和位置的 Active Directory。 此對話方塊也可讓您依完整功能變數名稱或首碼名稱使用 DNS 查詢。 這些搜尋會使用透過 .NET 執行的原生 DNS 和 LDAP 通訊協定，而非透過 SOAP 的 ad 管理閘道進行 AD Windows PowerShell，這表示伺服器管理員聯繫的網域控制站甚至可以執行 Windows Server 2003。 您也可以匯入具有伺服器名稱的檔案，以供布建之用。

Active Directory 搜尋會使用下列 LDAP 篩選準則：

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

## <a name="server-manager-remote-server-status"></a><a name="BKMK_ServerMgrStatus"></a>伺服器管理員遠端伺服器狀態
伺服器管理員使用位址路由通訊協定來測試遠端伺服器存取範圍。 未回應 ARP 要求的任何伺服器都不會列出，即使它們在集區中也一樣。

如果 ARP 回應，則會對伺服器發出 DCOM 和 WMI 連接，以傳回狀態資訊。 如果無法連線到 RPC、DCOM 和 WMI，伺服器管理員就無法完全管理伺服器。

## <a name="windows-powershell-module-loading"></a><a name="BKMK_PSLoadModule"></a>Windows PowerShell 模組載入
Windows PowerShell 3.0 會執行動態模組載入。 通常不再需要使用**import-module** Cmdlet;相反地，只要叫用 Cmdlet、alias 或 function，就會自動載入模組。

若要查看已載入的模組，請使用**get-help** Cmdlet。

```
Get-Module

```

![簡化的系統管理](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)

若要使用其匯出的函式和 Cmdlet 查看所有已安裝的模組，請使用：

```
Get-Module -ListAvailable

```

使用**import-module**命令的主要案例是當您需要存取 "AD：" Windows PowerShell 虛擬磁片磁碟機，而沒有其他任何已載入模組的情況下。 例如，使用下列命令：

```
import-module activedirectory
cd ad:
dir

```

## <a name="rid-issuance-hotfixes-for-previous-operating-systems"></a><a name="BKMK_Rid"></a>舊版作業系統的 RID 發行修補程式
查看有[更新可用來偵測並防止在執行 Windows Server 2008 R2 的網域控制站上過度使用全域 RID 集](https://support.microsoft.com/kb/2618669)區。

## <a name="ntdsutilexe-install-from-media-changes"></a><a name="BKMK_IFM"></a>Ntdsutil.exe 從媒體變更安裝
Windows Server 2012 會針對**ifm (Ifm 媒體建立) **功能表，將兩個額外的選項新增至 Ntdsutil.exe 命令列工具。 這些可讓您建立 IFM 存放區，而不需要先對匯出的 NTDS 執行離線磁碟重組。DIT 資料庫檔案。 當磁碟空間不是 premium 時，這會節省建立 IFM 的時間。

下表描述兩個新的功能表項目：

|功能表項目|說明|
|--|--|
|建立完整的 NoDefrag% s|建立沒有磁碟重組的 IFM 媒體，將完整的 AD DC 或 AD/LDS 實例重組到資料夾% s|
|建立 Sysvol 完整 NoDefrag% s|建立具有 SYSVOL 的 IFM 媒體，但不將完整 AD DC 的磁碟重組到資料夾% s|

![簡化的系統管理](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)

![簡化的系統管理](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)
