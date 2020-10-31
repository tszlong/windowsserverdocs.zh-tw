---
title: 將網域控制站升級到 Windows Server 2016
description: 本檔說明如何從 Windows Server 2012 R2 升級至 Windows Server 2016
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4a00bc2c6eabd8b5419d3a0d867efba9dfc1592a
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069170"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>將網域控制站升級到 Windows Server 2016

適用於：Windows Server

本主題提供有關 Windows Server 2016 中 Active Directory Domain Services 的背景資訊，以及說明從 Windows Server 2012 或 Windows Server 2012 R2 升級網域控制站的程式。

## <a name="pre-requisites"></a>必要條件

升級網域的建議方式是升級執行較新版本 Windows Server 的網域控制站，並視需要將舊版的網域控制站降級。 該方法是升級現有網域控制站之作業系統的慣用方法。 這份清單涵蓋您升級執行較新版本 Windows Server 的網域控制站之前，要遵循的一般步驟：

1. 確認目標伺服器符合系統需求。
1. 確認應用程式相容性。
1. 探討移至 Windows Server 2016 的建議
1. 確認安全性設定。 如需詳細資訊，請參閱 [Windows Server 2016 中與 AD DS 相關的已淘汰功能和行為變更](../../../get-started/deprecated-features.md)。
1. 檢查要執行安裝的電腦與目標伺服器的連線。
1. 檢查必要操作主機角色的可用性：
   - 若要在現有網域和樹系中安裝第一個執行 Windows Server 2016 的 DC，執行安裝的電腦必須連線到 **架構主機** 才能執行 adprep/forestprep 和基礎結構主機，以便執行 adprep/domainprep。
   - 若要在已擴充樹系架構的網域中安裝第一個 DC，您只需要連接到 **基礎結構主機** 。
   - 若要安裝或移除現有樹系中的網域，您需要連線到 **網域命名主機** 。
   - 任何網域控制站安裝也需要與 **RID 主機** 的連線。
   - 如果您要在現有的樹系中安裝第一個唯讀網域控制站，您需要連線到每個應用程式目錄分割的 **基礎結構主機** ，也稱為非網域命名內容或 NDNC。

### <a name="installation-steps-and-required-administrative-levels"></a>安裝步驟和所需的系統管理層級

下表提供完成這些步驟的升級步驟和許可權需求摘要

|安裝動作|認證要求|
| ----- | ----- |
|安裝新樹系|目標伺服器上的本機 Administrator|
|在現有樹系中安裝新網域|企業系統管理員|
|在現有網域中安裝其他 DC|網域管理員|
|執行 adprep /forestprep|Schema Admins、Enterprise Admins 和 Domain Admins|
|執行 adprep /domainprep|網域管理員|
|執行 adprep /domainprep /gpprep|網域管理員|
|執行 adprep /rodcprep|企業系統管理員|

如需 Windows Server 2016 新功能的詳細資訊，請參閱 [Windows server 2016 的新](../../../get-started/whats-new-in-windows-server-2016.md)功能。

## <a name="supported-in-place-upgrade-paths"></a>支援的就地升級路徑

執行64位版 Windows Server 2012 或 Windows Server 2012 R2 的網域控制站可以升級至 Windows Server 2016。 僅支援64位版本升級，因為 Windows Server 2016 只隨附于64位版本。

|如果您執行這個版本：|您可以升級到這些版本：|
| ----- | ----- |
|Windows Server 2012 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|

如需支援的升級路徑的詳細資訊，請參閱 [支援的升級路徑](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep 和域

如果您要就地將現有的網域控制站升級至 Windows Server 2016 作業系統，您將需要手動執行 adprep/forestprep 和 adprep/domainprep。  Adprep/forestprep 只需要在樹系中執行一次。  在您要升級至 Windows Server 2016 的網域控制站所在的每個網域中，都必須執行一次 Adprep/domainprep。

如果您要升級新的 Windows Server 2016 伺服器，就不需要手動執行這些伺服器。  這些會整合到 PowerShell 和伺服器管理員體驗中。

如需執行 adprep 的詳細資訊，請參閱執行 [adprep](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10))

## <a name="functional-level-features-and-requirements"></a>功能等級功能和需求

Windows Server 2016 需要 Windows Server 2003 樹系功能等級。 也就是說，在您可以將執行 Windows Server 2016 的網域控制站新增至現有的 Active Directory 樹系之前，樹系功能等級必須是 Windows Server 2003 或更高的版本。 如果樹系包含執行 Windows Server 2003 或更新版本的網域控制站，但樹系功能等級還是 Windows 2000，也會封鎖安裝。

在將 Windows Server 2016 網域控制站新增至樹系之前，必須先移除 windows 2000 網域控制站。 在這種情況下，請考慮下列工作流程：

1. 安裝執行 Windows Server 2003 或更新版本的網域控制站。 可以在評估版的 Windows Server 上部署這些網域控制站。 這個步驟還有一個先決條件，就是需要針對該作業系統版本執行 adprep.exe。
1. 移除 Windows 2000 網域控制站。 具體來說，就是以正常方式降級或以強制方式從網域和使用的 Active Directory 使用者和電腦移除 Windows Server 2000 網域控制站，藉此移除所有已移除之網域控制站的網域控制站帳戶。
1. 將樹系功能等級提升至 Windows Server 2003 或更高。
1. 安裝執行 Windows Server 2016 的網域控制站。
1. 移除執行舊版 Windows Server 的網域控制站。

### <a name="rolling-back-functional-levels"></a>回復功能等級

將樹系功能等級 (FFL) 設定為特定值之後，您就無法回復或降低樹系功能等級，但有下列例外狀況：

- 如果您要從 Windows Server 2012 R2 FFL 升級，可以將它降低回 Windows Server 2012 R2。
- 如果您要從 Windows Server 2008 R2 FFL 升級，可以將它降低回 Windows Server 2008 R2。

將網域功能等級設定為某個值之後，您就無法回復或降低網域功能等級，但有下列例外狀況：

- 當您將網域功能等級提高至 Windows Server 2016，而且樹系功能等級為 Windows Server 2012 或更低時，您可以選擇將網域功能等級回復至 Windows Server 2012 或 Windows Server 2012 R2。

如需較低功能等級上可用之功能的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>AD DS 與其他伺服器角色和 Windows 作業系統的互通性

下列 Windows 作業系統不支援 AD DS：

- Windows MultiPoint Server
- Windows Server 2016 Essentials

AD DS 無法安裝在同時執行下列伺服器角色或角色服務的伺服器上：

- Microsoft Hyper-V Server 2016
- 遠端桌面連線代理人

## <a name="administration-of-windows-server-2016-servers"></a>Windows Server 2016 伺服器的系統管理

使用 Windows 10 的遠端伺服器管理工具管理執行 Windows Server 2016 的網域控制站及其他伺服器。 您可以在執行 Windows 10 的電腦上執行 Windows Server 2016 遠端伺服器管理工具。

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>升級至 Windows Server 2016 的逐步解說

以下是將 Contoso 樹系從 Windows Server 2012 R2 升級至 Windows Server 2016 的簡單範例。

![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. 將新的 Windows Server 2016 加入您的樹系。 出現提示時重新開機。

   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)

1. 使用網域系統管理員帳戶登入新的 Windows Server 2016。
1. 在 **伺服器管理員** 的 [新增 **角色及功能** ] 下，在新的 Windows Server 2016 上安裝 **Active Directory Domain Services** 。 這會自動在 2012 R2 樹系和網域上執行 adprep。

   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png)

1. 在 **伺服器管理員** 中，按一下黃色三角形，然後從下拉式清單中按一下 [ **將伺服器升級為網域控制站** ]。

   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)

1. 在 [ **部署** 設定] 畫面上，選取 [ **新增現有樹系的網域控制站** ]，然後按 [下一步]。

   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)

1. 在 [ **網域控制站選項** ] 畫面上，輸入 **目錄服務還原模式 (DSRM)** 密碼，然後按 [下一步]。
1. 針對其餘的畫面，請按 **[下一步]** 。
1. 在 [必要條件 **檢查** ] 畫面上，按一下 [ **安裝** ]。 重新開機完成後，您就可以重新登入。
1. 在 Windows Server 2012 R2 伺服器的 **伺服器管理員** 中，于 [工具] 底下，選取 [ **Windows PowerShell 的 Active Directory 模組** 。

   ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)

1. 在 PowerShell 視窗中，使用 Move-ADDirectoryServerOperationMasterRole 來移動 FSMO 角色。 您可以輸入每個 OperationMasterRole 的名稱，或使用數位來指定角色。 如需詳細資訊，請參閱 [移動-ADDirectoryServerOperationMasterRole](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10))

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)

1. 移至 Windows Server 2016 伺服器，並在 **伺服器管理員** 的 [ **工具** ] 底下，選取 [工具] **Windows PowerShell 的 [Active Directory 模組** ]，以確認角色已移動。 使用 `Get-ADDomain` 和 `Get-ADForest` Cmdlet 來查看 FSMO 角色持有者。

    ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)

    ![升級](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)

1. 降級和移除 Windows Server 2012 R2 網域控制站。 如需降級 dc 的詳細資訊，請參閱 [降級網域控制站和網域](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
1. 一旦伺服器降級和移除之後，您就可以將樹系功能和網域功能等級提高至 Windows Server 2016。

## <a name="next-steps"></a>後續步驟

- [Active Directory Domain Services 安裝和移除的新功能](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)
- [安裝 Active Directory Domain Services &#40;層級 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)
- [Windows Server 2016 功能等級](../active-directory-functional-levels.md)
