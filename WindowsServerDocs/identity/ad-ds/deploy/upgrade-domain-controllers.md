---
title: "升級到 Windows Server 2016 的網域控制站"
description: "本文件告訴您如何從 Windows Server 2012 R2 升級到 Windows Server 2016"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187972c2f44a4d7f91b1b3ac1c905529564cfa6d
ms.sourcegitcommit: f748c6c4ce700b0787ffdd1fca620c21c4331fd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>升級到 Windows Server 2016 的網域控制站

適用於：Windows Server 2016

本主題提供 Active Directory Domain Services Windows Server 2016 中的背景資訊與解釋從 Windows Server 2012 或 Windows Server 2012 R2 網域控制站的程序。 

## <a name="pre-requisites"></a>必要條件
升級網域的建議的方式是促銷網域控制站執行較新版本的 Windows Server，而且降級舊版網域控制站視。 升級現有的網域控制站的作業系統最好的方法。 這份清單涵蓋一般到您的網域控制站執行 Windows Server 有較新版本的升級之前，請依照下列步驟： 

1.  請確認目標伺服器符合系統需求。 
2.  請確認應用程式的相容性。 
3.  移轉到 Windows Server 2016 檢視建議 
4.  檢查安全性設定。 如需詳細資訊，請查看[在 Windows Server 2016 AD DS 相關 Deprecated 功能和變更行為](../../../get-started\deprecated-features.md)。 
5.  檢查您想要執行安裝電腦的目標伺服器連接。 
6.  檢查有可用的必要作業主機的角色： 
    - 若要安裝 Windows Server 2016 上執行的現有的網域和樹系的第一個 DC，您執行安裝所在的電腦需要連接到**架構主機**為了執行 adprep /domainprep 為了執行 adprep /forestprep 與基礎結構主機。 
    - 若要安裝的第一個 DC 樹系架構會已經延伸的網域中，您只需要連接基礎結構主機。 
    - 若要安裝或在現有的樹系移除網域，您需要連接到**網域命名主機**。 
    - 任何網域控制站安裝也必須連接到**RID 主機。** 
    - 如果您第一個唯讀網域控制站安裝現有的樹系，您會需要為每個應用程式 directory 磁碟分割，也就是非網域命名操作或 NDNC 基礎結構主機連接。 

### <a name="installation-steps-and-required-administrative-levels"></a>安裝步驟和所需的系統層級
下表提供摘要升級步驟，才能完成這些步驟的權限要求

|安裝動作|認證需求|
| ----- | ----- |
|安裝新的樹系|本機目標伺服器上的系統管理員|
|在現有的樹系安裝新的網域|企業系統管理員|
|安裝其他俠現有網域中|網域系統管理員 」|
|執行 adprep /forestprep|架構系統管理員企業系統管理員，網域系統管理員|
|執行 adprep /domainprep|網域系統管理員 」|
|執行 adprep /domainprep /gpprep|網域系統管理員 」|
|執行 adprep /rodcprep|企業系統管理員|

適用於在 Windows Server 2016 中的新功能的詳細資訊，請查看[Windows Server 2016 中的新功能](../../../get-started/what-s-new-in-windows-server-2016.md)。



## <a name="supported-in-place-upgrade-paths"></a>支援的就地升級路徑
執行 64 位元版本的 Windows Server 2012 或 Windows Server 2012 R2 網域控制站可以升級到 Windows Server 2016。 因為只是在 64 位元版本的 Windows Server 2016 支援只 64 位元版本的升級。

|如果您執行此版本：|您可以升級至這些版本：|
| ----- | ----- |   
|Windows Server 2012 標準|Windows Server 2016 Standard 或 Datacenter|   
|Windows Server 2012 資料中心|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 標準|Windows Server 2016 Standard 或 Datacenter|    
|Windows Server 2012 R2 資料中心|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 的基本資訊|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 標準|Windows Storage Server 2016 Standard| 
|Windows Storage Server 2012 群組|Windows Storage Server 2016 群組|   
|Windows Storage Server 2012 R2 標準|Windows Storage Server 2016 Standard|  
|Windows Storage Server 2012 R2 群組|Windows Storage Server 2016 群組|    

如需支援的升級路徑，請查看[支援的升級路徑](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep，並且準備網域
如果您正在進行就地升級現有的網域控制站到 Windows Server 2016 的作業系統，您必須手動執行 adprep /forestprep adprep /domainprep。  Adprep /forestprep 需要森林中執行一次。  Adprep /domainprep 需要在每個網域中，您有您要升級到 Windows Server 2016 的網域控制站執行一次。

如果您不需要手動執行這些新的 Windows Server 2016 伺服器升級。  這些整合 PowerShell 和伺服器管理員體驗。

如需有關執行 adprep 查看[執行 Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>層級的功能和需求
Windows Server 2016 需要 Windows Server 2003 森林功能層級。 是的您可以加入現有的 Active Directory 樹系執行 Windows Server 2016 的網域控制站之前的樹系功能層級必須 Windows Server 2003 或更高版本。 如果樹系包含執行 Windows Server 2003 網域控制站或更新版本正常運作的樹系但層級仍是 Windows 2000，也會封鎖安裝。 

Windows Server 2016 網域控制站新增到您的樹系前必須移除 Windows 2000 的網域控制站。 若是如此，請考慮將下列工作流程： 


1. 安裝網域控制站執行 Windows Server 2003 或更新版本。 這些網域控制站可以在 Windows Server 的試用版部署。 這個步驟也需要執行該作業系統版本 adprep.exe 成必要條件。 
2.  Windows 2000 的網域控制站中移除。 尤其是、 適當降級或強制移除 Windows Server 2000 網域控制站網域使用 Active Directory 使用者及移除所有已移除的網域控制站的網域控制站帳號電腦。 
3.  提高或更高到 Windows Server 2003 森林功能等級。 
4.  安裝網域控制站執行 Windows Server 2016。 
5.  移除網域控制站執行較舊的 Windows Server 版本。 

### <a name="rolling-back-functional-levels"></a>正在復原功能層級

設定為某個值的樹系功能層級 (FFL) 之後，您無法復原或降低森林功能等級，使用下列例外： 

- 如果您從 Windows Server 2012 R2 FFL 進行升級，您可以回復到 Windows Server 2012 R2 降低它。 
- 如果您從 Windows Server 2008 R2 FFL 進行升級，您可以回復到 Windows Server 2008 R2 降低它。

網域功能層級設定為某個值之後，您無法復原或降低網域功能等級，使用下列例外： 

- Windows Server 2012 或 Windows Server 2012 R2 時您網域功能提高到 Windows Server 2016 如果的樹系功能層級是 Windows Server 2012 或較低，您可以備份循環網域功能等級的選項 

如需較低的功能層級的功能，請查看[Active Directory Domain Services 了解 (AD DS) 功能的層級](../active-directory-functional-levels.md)。 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Windows 作業系統其他伺服器角色與 AD DS 交互操作
在下列 Windows 作業系統 AD DS 不支援： 


- Windows 單多點 Server 
- Windows Server 2016 Essentials 

AD DS 無法也會執行下列伺服器角色或角色服務的伺服器上安裝： 

- Microsoft HYPER-V Server 2016
- 遠端桌面連接代理人 

## <a name="administration-of-windows-server-2016-servers"></a>管理 Windows Server 2016 伺服器
使用遠端伺服器管理工具適用於 Windows 10 管理網域控制站與其他執行 Windows Server 2016 的伺服器。 您可以在執行 Windows 10 的電腦上執行 Windows Server 2016 遠端伺服器管理工具。 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>升級到 Windows Server 2016 的逐步
以下是從 Windows Server 2012 R2 的 Contoso 樹系升級到 Windows Server 2016 的簡單範例。

![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1.  到您的樹系加入新的 Windows Server 2016。 出現提示時重新開機。 
![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2.  登入新的 Windows Server 2016 核對系統管理員使用。
3.  在**伺服器管理員**的**新增角色與功能**，安裝**Active Directory Domain Services**上新的 Windows Server 2016。 這會自動執行 adprep 2012 R2 的樹系網域上。
![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4.  在**伺服器管理員**，按一下黃色三角形，，從下拉式清單中，按**為網域控制站伺服器升級**。 
![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5.  在**部署組態**畫面上，選取**現有的樹系加入網域控制站**和 [下一步。 
![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6.  在**網域控制站選項**畫面中，輸入**Directory 服務還原模式 (DSRM)**的密碼並按一下 [下一步。 
7.  按一下螢幕上的其他部分的**下一步**。 
8.  在**必要條件檢查**畫面中，按**安裝**。 完成重新您之後可以重新登入。
9.  在 Windows Server 2012 R2 伺服器上，在**伺服器管理員**，選取 [工具] 底下， **Active Directory 模組適用於 Windows PowerShell**。 
![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. PowerShell windows 中使用移動 ADDirectoryServerOperationMasterRole 移動故障。 您可以輸入每個-OperationMasterRole 的名稱，或指定的角色使用數字。 數字。 如需詳細資訊請查看[移動-ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

``` powershell
Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
```

![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. 確認已角色移動，請移到 Windows Server 2016 伺服器，在**伺服器管理員**，在**工具**、 選取**Active Directory 模組適用於 Windows PowerShell**。 使用`Get-ADDomain`與`Get-ADForest`cmdlet 檢視 FSMO 角色位置。
![升級] (media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
！[升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. 降級與 Windows Server 2012 R2 網域控制站移除。 如降級 dc 資訊，請查看[降級網域控制站和網域](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. 一旦伺服器會降級並移除您可以提高的樹系的功能和 Windows Server 2016 網域正常運作的層級。


## <a name="next-steps"></a>後續步驟
-   [安裝和移除新功能 Active Directory Domain 服務](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [安裝 Active Directory Domain Services 和 #40;層級 100 和 #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Windows Server 2016 功能層級](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
