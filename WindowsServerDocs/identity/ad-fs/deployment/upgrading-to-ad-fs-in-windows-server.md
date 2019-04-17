---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: "升級到 Windows Server 2016 中 AD FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ce07398a2d624a1e9b004cd35eb9228d59dc2b5b
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/20/2018
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>升級到 Windows Server 2016 使用 WID 資料庫中 AD FS

>適用於：Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Windows Server 2012 R2 AD FS 發電廠從移到 Windows Server 2016 AD FS 陣列  
下列文件可描述您 AD FS Windows Server 2012 R2 發電廠升級到 Windows Server 2016 中的 AD FS，當您使用 WID 資料庫的方式。  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>AD FS 升級到 Windows Server 2016 FBL  
新在適用於 Windows Server 2016 AD FS 是發電廠行為層級功能 (FBL)。   此功能發電廠寬並判斷 AD FS 發電廠可以使用的功能。   根據預設，Windows Server 2012 R2 FBL 是在 Windows Server 2012 R2 AD FS 發電廠 FBL。  

Windows Server 2016 AD FS 伺服器新增到 Windows Server 2012 R2 陣列，它會維持 FBL 相同與 Windows Server 2012 R2。  當您有 Windows Server 2016 AD FS 伺服器以這種方式運作時，您發電廠即稱為 「 混合 」。  不過，您將無法善加利用 Windows Server 2016 的新功能，直到 FBL 以 Windows Server 2016 提出。  使用混合發電廠：  

-   系統管理員可以新增新的 Windows Server 2016 聯盟現有的 Windows Server 2012 R2 發電廠伺服器。  如此一來，發電廠 「 混合模式 」 是與 Windows Server 2012 R2 發電廠行為層級的運作方式。  若要確保發電廠一致的行為，Windows Server 2016 的新功能無法設定或使用此模式。  

-   之後，已從農場混合模式下，移除所有的 Windows Server 2012 R2 聯盟伺服器，在 WID 陣列，其中一個新的 Windows Server 2016 聯盟伺服器已升級至主要節點的角色，系統管理員可以再提高 FBL Windows Server 2012 R2 的 Windows Server 2016。  如此一來，任何新 AD FS Windows Server 2016 功能可以再設定及使用。  

-   如此一來的混合的發電廠功能，AD FS Windows Server 2012 R2 組織想要升級到 Windows Server 2016 不會部署全新發電廠，匯出與匯入設定資料。  他們可以 online 時 Windows Server 2016 節點加入現有發電廠而只會造成參與 FBL 提高相當簡短中斷。  

請注意，在混合的發電廠模式時，AD FS 發電廠不支援的新功能或 AD FS 在 Windows Server 2016 中加入的功能。  這表示公司想要試用的新功能無法執行此動作 FBL 引發直到。  如果您的組織期待測試新功能 rasing FBL 之前，您需要部署不同發電廠，若要這樣做。  

其餘部分是文件會提供適用於 Windows Server 2016 聯盟伺服器新增到 Windows Server 2012 R2 環境，然後引發 FBL Windows Server 2016 的步驟。  架構如下圖所要求的測試環境中執行這些步驟。  

> [!NOTE]  
> 您可以移至在 Windows Server 2016 FBL AD FS 之前，您必須移除所有的 Windows 2012 R2 節點。  您只是無法升級到 Windows Server 2016 的是 Windows Server 2012 R2 的作業系統，並讓它變得 2016年節點。  您必須將它移除，並更換新 2016年節點。
>
> 升級 FBL SQL 使用儲存 AD FS 設定時，會建立新的管理資料庫」AdfsConfigurationV3」。

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2016-farm-behavior-level"></a>AD FS 發電廠您升級到 Windows Server 2016 發電廠行為層級  

1.  Windows Server 2016 上使用伺服器管理員安裝 Active Directory 同盟服務的角色  

2.  請使用 AD FS 設定精靈，加入現有的 AD FS 發電廠新的 Windows Server 2016 伺服器。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  Windows Server 2016 聯盟伺服器上開放 AD FS 管理。    請注意，不顯示在此聯盟伺服器不主要伺服器。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  加入完成之後，在 Windows Server 2016 伺服器上，請打開 PowerShell，並執行下列 cmdlt: AdfsSyncProperties 設定-角色 PrimaryComputer  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  伺服器原始 AD FS Windows Server 2012 R2 上開放 PowerShell 並執行下列 cmdlt: AdfsSyncProperties 設定-角色 SecondaryComputer PrimaryComputerName {FQDN}  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  在您的 Web 應用程式 Proxy 開放 PowerShell 並執行 followoing cmdlt： 安裝-WebApplicationProxy CertificateThumbprint {SSLCert}-fsname fsname-TrustCred $trustcred  

7.  現在在 Windows Server 2016 聯盟伺服器開放 AD FS 管理。  請注意，現在所有節點會因為此伺服器的主要職務已經傳輸。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  與 Windows Server 2016 的安裝媒體，開放命令提示字元中，瀏覽至 support\adprep directory。  執行下列命令： adprep /forestprep。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

9. 一旦您已完成準備網域執行的 adprep 日  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

10. 現在在 Windows Server 2016 伺服器開放 PowerShell 並執行下列 cmdlt： 叫用 AdfsFarmBehaviorLevelRaise  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

11. 出現提示時，Y 的輸入。這將會開始提高層級。  一旦完成後您已成功地提高 FBL。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

> [!NOTE]  
> AD FS 伺服器使用 SQL 設定，現在已名稱」AdfsConfiguraionV3」建立新的 manamgement 資料庫。 

12. 現在，如果您移至 [AD FS 管理，您將會看到 AD fs Windows Server 2016 中已加入新節點  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. 同樣地，您可以使用 PowerShell cmdlt： 取得-AdfsFarmInformation 來顯示您目前 FBL。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
