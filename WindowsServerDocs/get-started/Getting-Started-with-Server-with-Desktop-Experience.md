---
title: 安裝含有桌面體驗的伺服器
description: 說明如何取得並安裝含有桌面體驗的伺服器安裝
ms.prod: windows-server
ms.date: 01/18/2017
ms.technology: server-general
ms.topic: article
ms.assetid: 5b38b8a0-4dfc-4130-be00-fc58bba99595
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 18c454d9ef4deb8c9ea681a486e85f356ad5c9cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826941"
---
# <a name="install-server-with-desktop-experience"></a>安裝含有桌面體驗的伺服器
> 適用於：Windows Server 2016
  

當您使用安裝精靈安裝 Windows Server 2016 時，您可以在 **Windows Server 2016**和 **Windows Server (含有桌面體驗的伺服器)** 之間進行選擇。 Windows Server 2016 的 [含有桌面體驗的伺服器] 選項，相當於已安裝桌面體驗功能之 Windows Server 2012 R2 中的 [完整] 安裝選項。 如果您不在安裝精靈中做出選擇，系統將會安裝 **Windows Server 2016**，這是 [Server Core]  安裝選項。

此 [含有桌面體驗的伺服器] 選項會安裝標準使用者介面和所有工具，包括需要在 Windows Server 2012 R2 中另外安裝的用戶體驗功能。 伺服器角色及功能可由伺服器管理員或其他方法安裝。 與 Server Core 選項相較下，它需要更多的磁碟空間，而且具有較高的維護需求。因此，除非您特別需要 [含有桌面體驗的伺服器] 選項所包含的額外使用者介面元素與圖形化管理工具，否則我們建議您選擇 Server Core 安裝。 如果您認為不需要額外元素即可進行，請參閱[安裝 Server Core](Getting-Started-with-Server-Core.md)。 如需更輕量型的選項，請參閱[安裝 Nano Server](Getting-Started-with-Nano-Server.md)。

> [!NOTE]
>
> 與 Windows Server 某些更早的版本不同，您無法在安裝完成後在 Server Core 與桌面體驗伺服器之間進行轉換。 如果您安裝桌面體驗伺服器，並稍後決定使用 Server Core，您應該執行全新安裝。

**使用者介面：** 標準圖形化使用者介面 (伺服器圖形化介面)。 伺服器圖形化介面包含新的 Windows 10 殼層。 此選項預設安裝的特定 Windows 功能為User-Interfaces-Infra、Server-GUI-Shell、Server-GUI-Mgmt-Infra、InkAndHandwritingServices、ServerMediaFoundation 及桌面體驗。 雖然這些功能會顯示於此版本的「伺服器管理員」中，系統並不支援將它們解除安裝，也不會在未來版本中提供此功能。

**在本機安裝、設定、解除安裝伺服器角色：** 使用「伺服器管理員」或使用 Windows PowerShell

**在遠端安裝、設定、解除安裝伺服器角色：** 使用「伺服器管理員」、「遠端伺服器」、RSAT 或 Windows PowerShell

**Microsoft Management Console：已安裝**

## <a name="installation-scenarios"></a>安裝案例

### <a name="evaluation"></a>評估
您可以從 [Windows Server 評估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)取得 Windows Server 的 180 天授權評估版。 選擇 [Windows Server 2016] | [64 位元 ISO] 選項  進行下載，您也可以瀏覽 [Windows Server 2016] | [虛擬實驗室]  。

> [!IMPORTANT]  
> 對於 14393.0.161119-1705.RS1_REFRESH 之前的 Windows Server 2016 版本，您只能執行從已使用 [桌面體驗] 選項 (非 [Server Core] 選項) 安裝之 Windows Server 2016 的評估版轉換為零售版的這項轉換。 從 14393.0.161119-1705.RS1_REFRESH 版本開始與未來的版本，您可以將評估版本轉換零售，無論使用何種安裝選項。


### <a name="clean-installation"></a>全新安裝

若要從媒體安裝 [含有桌面體驗的伺服器] 安裝選項，請將媒體插入磁碟機、重新啟動電腦，然後執行 Setup.exe。 在開啟的精靈中，選取 [Windows Server (含有桌面體驗的伺服器)]  (Standard 或 Datacenter)，然後完成精靈。

### <a name="upgrade"></a>升級
**升級**表示從現有作業系統版本移至更新的版本，但仍使用相同硬體。

如果您已經有適當 Windows Server 產品的完整安裝，您可以將它升級至適當 Windows Server 2016 版本的 [含有桌面體驗的伺服器] 安裝，如下所示。

> [!IMPORTANT]  
> 在此版本中，虛擬機器中的升級效果最佳，因為虛擬機器不需要特定 OEM 硬體驅動程式，就能成功升級。 否則，建議使用移轉選項。  

- 不支援從 32 位元到 64 位元架構的就地升級。 所有 Windows Server 2016 版本只支援 64 位元。
- 不支援從某種語言到另一種語言的就地升級。
- 如果伺服器是網域控制站，請參閱[將網域控制站升級為 Windows Server 2012 R2 與 Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx) 以取得重要資訊。
- 不支援從 Windows Server 2016 的發行前版本 (預覽) 升級。 執行 Windows Server 2016 的全新安裝。
- 不支援將 Server Core 安裝切換到含桌面的伺服器安裝，以及將含桌面的伺服器安裝切換到 Server Core 安裝的升級。

如果您在左欄看不到目前的版本，則不支援升級到這個 Windows Server 2016 版本。

如果您在右欄看到多個版本，則支援從相同版本開始升級到列出的**任一**版本。

|如果您執行這個版本：|您可以升級到這些版本：|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|

如需移至 Windows Server 2016 的許多其他選項，例如在大量授權版本、評估版及其他版本之間的授權轉換，請參閱[升級選項](Supported-Upgrade-Paths.md)中的詳細資訊。

### <a name="migration"></a>移轉
**移轉**表示藉由在一組不同的硬體或虛擬機器上執行全新安裝，然後將舊版伺服器的工作負載轉移到新的伺服器，來從現有作業系統移至 Windows Server 2016。 視您安裝的伺服器角色而定，移轉可能有極大的差異，這會在 [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Windows Server 安裝、升級和移轉) 中深入討論。

移轉的功能會因不同的伺服器角色而異。 下表說明移至 Windows Server 2016 的特定伺服器角色升級和移轉選項。 如需個別角色移轉指南，請瀏覽[在 Windows Server 中移轉角色與功能](https://technet.microsoft.com/windowsserver/jj554790.aspx)。 如需安裝和升級的詳細資訊，請參閱 [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Windows Server 安裝、升級和移轉)。

|伺服器角色|是否可從 Windows Server 2012 R2 升級？|是否可從 Windows Server 2012 升級？|是否支援移轉？|是否可以在不停機的情況下完成移轉？|  
|-------------------|----------|--------------|--------------|----------|  
|Active Directory 憑證服務|    是|    是|    是|    否|
|Active Directory 網域服務|    是|    是|    是|    是|
|Active Directory Federation Services|    否|    否|    是|    否 (不需要新增節點至伺服器陣列)|
|Active Directory 輕量型目錄服務|    是|    是|    是|    是|
|Active Directory Rights Management Services|    是|    是|    是|    否|
|容錯移轉叢集|使用包含節點 [暫停-清空]、[收回] 的 [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 程序，升級至 Windows Server 2016 並重新加入原始叢集時，則為是。 當伺服器遭升級用叢集移除後再新增至其他叢集時，則為是。|當伺服器不是叢集的一部分時，則為否。 當伺服器遭升級用叢集移除後再新增至其他叢集時，則為是。    |是|若是 Windows Server 2012 容錯移轉叢集，則為否。 若是含 Hyper-V VM 的 Windows Server 2012 R2 容錯移轉叢集，或執行向外延展檔案伺服器角色的 Windows Server 2012 R2 容錯移轉叢集，則為是。 請參閱 [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級)。|
|檔案和存放服務|    是|    是|    因子功能而異|    否|
|列印和傳真服務|    否|    否|    是 (Printbrm.exe)|    否|
|遠端桌面服務|    是，適用於所有子角色，但不支援混合模式的伺服器陣列|    是，適用於所有子角色，但不支援混合模式的伺服器陣列|    是|    否|
|Web 伺服器 (IIS)|    是|    是|    是|    否|
|Windows Server Essentials 體驗|    是|    不適用 - 新功能|    是|    否|
|Windows Server Update Services|    是|    是|    是|    否|
|工作資料夾|    是|    是|    是|    是，使用[叢集作業系統輪流升級](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)時從 WS 2012 R2 叢集進行。|

> [!IMPORTANT]  
> 一旦安裝程式完成，而且您已安裝所有必要的伺服器角色和功能之後，請立即使用 Windows Update 或其他更新方法來檢查並安裝 Windows Server 2016 的可用更新。

---------------------------------------
如果您需要其他安裝選項，或者如果您已經完成安裝，並準備好要部署特定工作負載，您可以[回到 Windows Server 2016 主頁面](Windows-Server-2016.md)。