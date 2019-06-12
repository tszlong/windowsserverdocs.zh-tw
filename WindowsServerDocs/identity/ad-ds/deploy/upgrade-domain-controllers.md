---
title: 將網域控制站升級到 Windows Server 2016
description: 本文件說明如何從 Windows Server 2012 R2 升級到 Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6f3907426fd1124c5ed0a411a155490a2a537239
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719676"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>將網域控制站升級到 Windows Server 2016

適用於：Windows Server 2016

本主題提供 Windows Server 2016 中的 Active Directory 網域服務的背景資訊，並說明從 Windows Server 2012 或 Windows Server 2012 R2 升級網域控制站的程序。 

## <a name="pre-requisites"></a>必要條件
升級網域的建議的方式是升級執行較新版本的 Windows Server，以及降級舊版網域控制站所需的網域控制站。 該方法是升級現有網域控制站之作業系統的慣用方法。 這份清單涵蓋您升級執行較新版本的 Windows Server 網域控制站前要遵循的一般步驟： 

1. 確認目標伺服器符合系統需求。 
2. 請確認應用程式相容性。 
3. 移至 Windows Server 2016 中檢閱建議 
4. 確認安全性設定。 如需詳細資訊，請參閱 <<c0> [ 到 Windows Server 2016 中的 AD DS 相關的過時功能與行為變更](https://docs.microsoft.com/en-us/windows-server/get-started/deprecated-features)。 
5. 檢查要執行安裝的電腦與目標伺服器的連線。 
6. 檢查必要操作主機角色的可用性： 
   - 若要安裝執行 Windows Server 2016 的現有網域和樹系中的第一個 DC，您用來執行安裝的電腦需要連線能力**架構主機**才能執行 adprep /forestprep，基礎結構主機若要執行 adprep /domainprep。 
   - 若要在已經延伸樹系架構的網域中安裝第一個 DC，則只需要連線到基礎結構主機即可。 
   - 若要安裝或移除現有的樹系中的網域，您需要連接到**網域命名主機**。 
   - 任何網域控制站安裝也需要連線到**RID 主機。** 
   - 若要在現有樹系中安裝第一個唯讀網域控制站，您需要連接到每一個應用程式目錄分割的基礎結構主機，也稱為非網域命名內容或 NDNC。 

### <a name="installation-steps-and-required-administrative-levels"></a>安裝步驟和所需的系統管理層級
下表提供的升級步驟和權限需求，來完成這些步驟的摘要

|安裝動作|認證要求|
| ----- | ----- |
|安裝新樹系|目標伺服器上的本機 Administrator|
|在現有樹系中安裝新網域|Enterprise Admins|
|在現有網域中安裝其他 DC|Domain Admins|
|執行 adprep /forestprep|Schema Admins、Enterprise Admins 和 Domain Admins|
|執行 adprep /domainprep|Domain Admins|
|執行 adprep /domainprep /gpprep|Domain Admins|
|執行 adprep /rodcprep|Enterprise Admins|

如需其他有關 Windows Server 2016 中的新功能的詳細資訊，請參閱[什麼是 Windows Server 2016 中新](../../../get-started/what-s-new-in-windows-server-2016.md)。



## <a name="supported-in-place-upgrade-paths"></a>支援的就地升級路徑
執行 64 位元版本的 Windows Server 2012 或 Windows Server 2012 R2 的網域控制站可以升級至 Windows Server 2016。 只有 64 位元版本進行升級，因為 Windows Server 2016 僅提供 64 位元版本。

|如果您執行這個版本：|您可以升級到這些版本：|
| ----- | ----- |   
|Windows Server 2012 Standard|Windows Server 2016 Standard 或 Datacenter|   
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard 或 Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|    

如需有關支援之升級路徑的詳細資訊，請參閱[支援的升級路徑](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep 和 Domainprep
如果您要執行的 Windows Server 2016 作業系統的現有網域控制站就地升級，您必須手動執行 adprep /forestprep，adprep /domainprep。  Adprep /forestprep，必須在樹系執行一次。  Adprep /domainprep，就必須在每個物件，其中有您要升級到 Windows Server 2016 的網域控制站的網域執行一次。

如果您正在升級您不需要手動執行這些新的 Windows Server 2016 伺服器。  這些都已整合至 PowerShell 和伺服器管理員體驗。

如需執行 adprep 的詳細資訊，請參閱[執行 Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>功能等級功能和需求
Windows Server 2016 需要 Windows Server 2003 樹系功能等級。 也就是您可以新增至現有的 Active Directory 樹系中執行 Windows Server 2016 的網域控制站之前，樹系功能等級必須是 Windows Server 2003 或更高版本。 如果樹系包含執行 Windows Server 2003 或更新版本的網域控制站，但樹系功能等級還是 Windows 2000，也會封鎖安裝。 

必須移除 Windows 2000 網域控制站，才能將 Windows Server 2016 網域控制站新增至您的樹系。 在這種情況下，請考慮下列工作流程： 


1. 安裝執行 Windows Server 2003 或更新版本的網域控制站。 可以在評估版的 Windows Server 上部署這些網域控制站。 此步驟也需要執行該作業系統版本的 adprep.exe，做為必要條件。 
2.  移除 Windows 2000 網域控制站。 具體來說，就是以正常方式降級或以強制方式從網域和使用的 Active Directory 使用者和電腦移除 Windows Server 2000 網域控制站，藉此移除所有已移除之網域控制站的網域控制站帳戶。 
3.  將樹系功能等級提升至 Windows Server 2003 或更高。 
4.  安裝執行 Windows Server 2016 的網域控制站。 
5.  移除執行舊版 Windows Server 的網域控制站。 

### <a name="rolling-back-functional-levels"></a>正在回復的功能等級

您將樹系功能等級 (FFL) 設定為某個值之後，您無法回復或降低樹系功能等級，但有下列例外： 

- 如果您從 Windows Server 2012 R2 FFL 進行升級，您可以回到 Windows Server 2012 R2 中將它降低。 
- 如果您從 Windows Server 2008 R2 FFL 進行升級，您可以回到 Windows Server 2008 R2 中將它降低。

網域功能等級設定為某個值之後，您就無法回復或降低網域功能等級，但有下列例外： 

- 當您提高至 Windows Server 2016 網域功能等級，而且如果樹系功能等級為 Windows Server 2012 或更低，則可以選擇將網域功能等級是回 Windows Server 2012 或 Windows Server 2012 R2 

如需較低功能等級上可用之功能的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>AD DS 與其他伺服器角色和 Windows 作業系統的互通性
下列 Windows 作業系統不支援 AD DS： 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

AD DS 無法安裝在同時執行下列伺服器角色或角色服務的伺服器上： 

- Microsoft Hyper-V Server 2016
- 遠端桌面連線代理人 

## <a name="administration-of-windows-server-2016-servers"></a>管理 Windows Server 2016 伺服器
您可以使用遠端伺服器管理工具適用於 Windows 10 的來管理網域控制站和其他執行 Windows Server 2016 的伺服器。 您可以在執行 Windows 10 的電腦上執行 Windows Server 2016 遠端伺服器管理工具。 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>逐步升級至 Windows Server 2016
以下是從 Windows Server 2012 R2 的 Contoso 樹系升級到 Windows Server 2016 的簡單範例。

![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. 您的樹系中加入新的 Windows Server 2016。 出現提示時重新啟動。 
   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2. 新的 Windows Server 2016 和網域系統管理員帳戶登入。
3. 在 **伺服器管理員**下方**新增角色及功能**，安裝**Active Directory 網域服務**新的 Windows Server 2016 上。 這會自動執行 adprep，2012 R2 的樹系和網域。
   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4. 在 **伺服器管理員**，按一下黃色的三角形，然後從下拉式清單按一下**提升為網域控制站伺服器**。 
   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5. 在 **部署組態**畫面上，選取**將網域控制站新增至現有的樹系**，按一下 下一步。 
   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6. 在 **網域控制站選項**畫面上，輸入**目錄服務還原模式 (DSRM)** 密碼並按 下一步。 
7. 按一下螢幕上的其餘部分**下一步**。 
8. 在 **必要條件檢查**畫面上，按一下**安裝**。 重新啟動完成後您可以重新登入。
9. 在 Windows Server 2012 R2 伺服器上，在**伺服器管理員**，在 [工具] 下選取**模組的 Windows PowerShell 的 Active Directory**。 
   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. 在 PowerShell 視窗中使用移動 ADDirectoryServerOperationMasterRole 移動 FSMO 角色。 您可以輸入的每個-OperationMasterRole 名稱，或使用指定之角色的數字。 如需詳細資訊，請參閱[移動 ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. 確認角色已在移至 Windows Server 2016 伺服器上，移動**伺服器管理員**下方**工具**，選取**Active Directory 的 Windows PowerShell 模組**。 使用`Get-ADDomain`和`Get-ADForest`cmdlet 來檢視的 FSMO 角色持有者。
    ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
    ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. 降級和移除 Windows Server 2012 R2 網域控制站。 降級的 dc 上的資訊，請參閱[降級網域控制站和網域](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. 一旦伺服器是降級和移除您可以提高至 Windows Server 2016 網域功能等級與樹系功能。


## <a name="next-steps"></a>後續步驟
-   [Active Directory Domain Services 安裝和移除的新功能](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [安裝 Active Directory 網域服務&#40;層級 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Windows Server 2016 功能等級](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
