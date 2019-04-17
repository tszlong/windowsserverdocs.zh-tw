---
title: "升級到 Windows Server 2016 SQL server 中 AD FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 034d4f1f8d81cf105ba94bff34b180555702acde
ms.sourcegitcommit: 33c1f4965cd2eed7d384a096cfa5c883467b16a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2017
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>升級到 Windows Server 2016 SQL server 中 AD FS

>適用於：Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Windows Server 2012 R2 AD FS 發電廠從移到 Windows Server 2016 AD FS 陣列  
下列文件將描述您 AD FS Windows Server 2012 R2 發電廠升級到 Windows Server 2016 中的 AD FS，當您正在使用 AD FS 資料庫 SQL Server 的方式。  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>AD FS 升級到 Windows Server 2016 FBL  
新在適用於 Windows Server 2016 AD FS 是發電廠行為層級功能 (FBL)。   此功能發電廠寬並判斷 AD FS 發電廠可以使用的功能。   根據預設，Windows Server 2012 R2 FBL 是在 Windows Server 2012 R2 AD FS 發電廠 FBL。  

Windows Server 2016 AD FS 伺服器新增到 Windows Server 2012 R2 陣列，它會維持 FBL 相同與 Windows Server 2012 R2。  當您有 Windows Server 2016 AD FS 伺服器以這種方式運作時，您發電廠即稱為 「 混合 」。  不過，您將無法善加利用 Windows Server 2016 的新功能，直到 FBL 以 Windows Server 2016 提出。  使用混合發電廠：  

-   系統管理員可以新增新的 Windows Server 2016 聯盟現有的 Windows Server 2012 R2 發電廠伺服器。  如此一來，發電廠 「 混合模式 」 是與 Windows Server 2012 R2 發電廠行為層級的運作方式。  若要確保發電廠一致的行為，Windows Server 2016 的新功能無法設定或使用此模式。  

-   之後，已從農場混合模式下，移除所有的 Windows Server 2012 R2 聯盟伺服器，在 WID 陣列，其中一個新的 Windows 進行 2016年聯盟伺服器已升級至主要節點的角色，系統管理員可以再提高 FBL Windows Server 2012 R2 的 Windows Server 2016。  如此一來，任何新 AD FS Windows Server 2016 功能可以再設定及使用。  

-   如此一來的混合的發電廠功能，AD FS Windows Server 2012 R2 組織想要升級到 Windows Server 2016 不會部署全新發電廠，匯出與匯入設定資料。  他們可以 online 時 Windows Server 2016 節點加入現有發電廠而只會造成參與 FBL 提高相當簡短中斷。  

請注意，在混合的發電廠模式時，AD FS 發電廠不支援的新功能或 AD FS 在 Windows Server 2016 中加入的功能。  這表示公司想要試用的新功能無法執行此動作 FBL 引發直到。  如果您的組織期待測試新功能 rasing FBL 之前，您需要部署不同發電廠，若要這樣做。  

其餘部分是文件會提供適用於 Windows Server 2016 聯盟伺服器新增到 Windows Server 2012 R2 環境，然後引發 FBL Windows Server 2016 的步驟。  架構如下圖所要求的測試環境中執行這些步驟。  

> [!NOTE]  
> 您可以移至在 Windows Server 2016 FBL AD FS 之前，您必須移除所有的 Windows 2012 R2 節點。  您只是無法升級到 Windows Server 2016 的是 Windows Server 2012 R2 的作業系統，並讓它變得 2016年節點。  您必須將它移除，並更換新 2016年節點。  

下列架構圖表顯示用來驗證及錄製下列步驟進行安裝。

![架構](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png) 


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>加入 Windows 2016 AD FS 伺服器至 AD FS 陣列

1.  Windows Server 2016 上使用伺服器管理員安裝 Active Directory 同盟服務的角色  

2.  請使用 AD FS 設定精靈，加入現有的 AD FS 發電廠新的 Windows Server 2016 伺服器。  在**歡迎**畫面上按一下**下**。
 ![加入農場](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  在**連接至 Active Directory Domain Services**畫面，請**指定管理員**的權限來執行同盟服務設定，按一下 [**下一步**。
4.  在**指定發電廠**畫面中，輸入 SQL server，而且執行個體的名稱，然後按一下**下**。
![加入農場](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  在**指定 SSL 憑證**畫面中，指定的憑證，按一下 [**下**。
![加入農場](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  在**指定服務 Account**畫面中，指定服務帳號，按一下 [**下**。 
7.  在**檢視選項**畫面中選項，按**下**。 
8.  在**必要條件檢查**畫面上，確認所有必要條件檢查已經過了，按一下 [**設定**。
9.  在**結果**畫面，確定已成功設定伺服器，按**關閉**。
 
   
#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Windows Server 2012 R2 AD FS 伺服器中移除

>[!NOTE]
>您不需要設定使用 AdfsSyncProperties 設定為主要 AD FS 伺服器-時使用資料庫 SQL 角色。  這是因為所有節點視為主要此設定。

1.  在 Windows Server 2012 R2 AD FS 伺服器在伺服器管理員使用**移除角色與功能**在**管理**。 
![移除伺服器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  在**在您開始之前**畫面中，按一下 [**下**。
3.  在**伺服器選取**畫面上，按一下 [**下**。
4.  在**伺服器角色**畫面上，移除旁邊的核取**Active Directory 同盟服務**並按**下一步**。
![移除伺服器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  在**功能**畫面上，按**下**。
6.  在**確認**畫面上，按**移除**。
7.  一旦完成後，請重新開機伺服器。
     
#### <a name="raise-the-farm-behavior-level-fbl"></a>提升發電廠行為 (FBL)
這個步驟之前，您需要確保 forestprep 及準備網域有已執行 Active Directory 環境並 Active Directory 有的 Windows Server 2016 結構描述。  開始使用 Windows 2016 網域控制站這份文件，並不需要執行這些因為他們已執行安裝 AD 時。

1. 現在在 Windows Server 2016 伺服器開放 PowerShell 並執行下列命令：**$cred = 取得認證**並輸入叫用。
2. 輸入認證對 SQL Server 中的系統管理員權限。
3. 現在在 PowerShell，輸入下列項目：**叫用-AdfsFarmBehaviorLevelRaise-認證 $cred**
2. 出現提示時，輸入**Y**。這將會開始提高層級。  一旦完成後您已成功地提高 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. 現在，如果您移至 [AD FS 管理，您將會看到 AD fs Windows Server 2016 中已加入新節點  
4. 同樣地，您可以使用 PowerShell cmdlt： 取得-AdfsFarmInformation 來顯示您目前 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)
