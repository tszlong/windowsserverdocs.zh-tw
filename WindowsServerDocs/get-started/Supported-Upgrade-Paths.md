---
title: Windows Server 2016 的升級和轉換選項
description: 說明到 Windows Server 2016 的所有支援升級路徑。
ms.prod: windows-server
ms.date: 01/18/2017
ms.technology: server-general
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 05e891d4170458018577b39bc83e952bf18d420e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826501"
---
# <a name="upgrade-and-conversion-options-for-windows-server-2016"></a>Windows Server 2016 的升級和轉換選項

>適用於：Windows Server 2019、Windows Server 2016

本主題包含有關使用各種方式從舊版作業系統升級到 Windows Server&reg; 2016 的資訊。

視您開始使用的作業系統和採取的方式而定，移至 Windows Server 2016 的程序可能有極大的差異。 我們使用下列詞彙來區別不同的動作，任何一項都可能涉及新的 Windows Server 2016 部署。

- **安裝** 是讓您的硬體有新作業系統的基本概念。 特別是， **全新安裝** 需要刪除之前的作業系統。 如需安裝 Windows Server 2016 的相關資訊，請參閱 [System Requirements and Installation Information for Windows Server 2016](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements--and-installation) (Windows Server 2016 的系統需求與安裝資訊)。 如需安裝其他 Windows Server 版本的相關資訊，請參閱 [Windows Server Installation and Upgrade](https://technet.microsoft.com//windowsserver/dn527667) (Windows Server 安裝與升級)。

- **移轉**表示透過轉移到一組不同的硬體或虛擬機器，從現有的作業系統移至 Windows Server 2016。 視您安裝的伺服器角色而定，移轉可能有極大的差異，這會在 [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Windows Server 安裝、升級和移轉) 中深入討論。

- **叢集作業系統輪流升級**是 Windows Server 2016 中的新功能，可讓系統管理員將叢集節點的作業系統從 Windows Server 2012 R2 升級至 Windows Server 2016，而不需停止 Hyper-V 或「向外延展檔案伺服器」工作負載。 這項功能可讓您避免可能影響服務等級協定的停機時間。 [Cluster operating system rolling upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 會更詳細地討論這項新功能。

- **授權轉換**：在某些作業系統版本中，您可使用簡單的命令與適當的授權金鑰執行一個步驟，就能將特定的版本轉換成另一種版本。 我們將此方式稱為授權轉換。 例如，如果您的伺服器執行 Windows Server 2016 Standard，您可以將它轉換為 Windows Server 2016 Datacenter。

- **升級**表示從現有作業系統版本移至更新的版本，但仍使用相同硬體。 (這有時稱為就地用戶端。)例如，如果您的伺服器執行 Windows Server 2012 或 Windows Server 2012 R2，則可以將它升級到 Windows Server 2016。 您可以從評估版的作業系統升級到零售版，或從較舊的零售版升級到較新的版本，而在某些情況下，您甚至可以從大量授權版本的作業系統升級到一般的零售版。

> [!IMPORTANT]  
> 虛擬機器中的升級效果最佳，因為虛擬機器不需要特定 OEM 硬體驅動程式，就能成功升級。  

> [!IMPORTANT]  
> 對於 14393.0.161119-1705.RS1_REFRESH 之前的 Windows Server 2016 版本，**您只能執行從已使用 [桌面體驗] 選項 (非 [Server Core] 選項) 安裝之 Windows Server 2016 的評估版轉換為零售版的這項轉換**。 從 14393.0.161119-1705.RS1_REFRESH 版本開始與未來的版本，您可以將評估版本轉換零售，無論使用何種安裝選項。

> [!IMPORTANT]  
> 如果您的伺服器使用 NIC 小組，請在升級之前停用 NIC 小組，然後在完成升級之後重新予以啟用。 如需詳細資訊，請參閱 [NIC 小組概觀](https://technet.microsoft.com/library/hh831648(v=ws.11).aspx)。

## <a name="upgrading-previous-retail-versions-of-windows-server-to-windows-server-2016"></a>將舊的 Windows Server 零售版升級到 Windows Server 2016

下表簡單地摘要說明哪些**已經授權**的 (也就是非評估版) Windows 作業系統可以升級到哪些版本的 Windows Server 2016。

請記住所支援路徑的下列一般指導方針：

- 不支援從 32 位元到 64 位元架構的升級。 所有 Windows Server 2016 版本都只支援 64 位元。
- 不支援從某種語言到另一種語言的升級。
- 如果伺服器是網域控制站，請參閱[將網域控制站升級為 Windows Server 2012 R2 與 Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx) 以取得重要資訊。
- 不支援從 Windows Server 2016 的發行前版本 (預覽) 升級。 執行 Windows Server 2016 的全新安裝。
- 不支援將 Server Core 安裝切換到含桌面的伺服器安裝，以及將含桌面的伺服器安裝切換到 Server Core 安裝的升級。
- 不支援從舊版的 Windows Server 安裝升級至評估版的 Windows Server。 評估版應該安裝為全新安裝。

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


## <a name="per-server-role-considerations-for-upgrading"></a>升級時每個伺服器角色的考量

即使在從舊的零售版到 Windows Server 2016 支援的升級路徑中，已經安裝的特定伺服器角色可能需要額外的準備或動作，以便角色在升級之後繼續運作。 對於您要升級的每個伺服器角色，請參閱特定 TechNet Library 主題，以取得可能需要之額外步驟的詳細資料。

## <a name="converting-a-current-evaluation-version-to-a-current-retail-version"></a>將目前的評估版轉換為目前的零售版

您可以將 Windows Server 2016 Standard 評估版轉換為 Windows Server 2016 Standard (零售) 或 Datacenter (零售)。 同樣地，您也可以將 Windows Server 2016 Datacenter 評估版轉換為零售版。

> [!IMPORTANT]  
> 對於 14393.0.161119-1705.RS1_REFRESH 之前的 Windows Server 2016 版本，您只能執行從已使用 [桌面體驗] 選項 (非 [Server Core] 選項) 安裝之 Windows Server 2016 的評估版轉換為零售版的這項轉換。 從 14393.0.161119-1705.RS1_REFRESH 版本開始與未來的版本，您可以將評估版本轉換零售，無論使用何種安裝選項。

嘗試從評估版轉換為零售版之前，請確認您的伺服器的確是執行評估版。 若要這樣做，請執行下列其中一項：

- 從提升權限的命令提示字元，執行 **slmgr.vbs /dlv**；評估版的輸出中將包含 EVAL。

- 從 [開始] 畫面中，開啟 [控制台]  。 依序開啟 [系統及安全性]  、[系統]  。 檢視 [系統]  頁面之 [Windows 啟用] 區域中的 Windows 啟用狀態。 按一下 [Windows 啟用] 中的 [檢視詳細資料]  ，了解 Windows 啟用狀態的詳細資訊。

如果已經啟用 Windows，桌面會顯示評估期的剩餘時間。

如果伺服器是執行零售版而不是評估版，請參閱本主題的＜將舊的 Windows Server 零售版升級到 Windows Server 2016＞一節，了解升級到 Windows Server 2016 的指示。

對於 **Windows Server 2016 Essentials**：只要在命令 **slmgr.vbs** 中輸入零售、大量授權或 OEM 金鑰，即可轉換成完整的零售版。

如果伺服器執行 Windows Server 2016 Standard 或 Windows Server 2016 Datacenter 的評估版，您可以按照下列方式將它轉換成零售版：

1.    如果伺服器是**網域控制站**，則不能轉換成零售版。 在這種情況下，請在執行零售版的伺服器上安裝另一個網域控制站，然後從評估版上執行的網域控制站中移除 AD DS。 如需詳細資訊，請參閱[將網域控制站升級為 Windows Server 2012 R2 與 Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)。
2.    閱讀授權條款。
3.    從提升權限的命令提示字元，使用命令 **DISM /online /Get-CurrentEdition**判斷目前的版本名稱。 記下版本識別碼，它是版本名稱的簡短形式。 然後，提供版本識別碼和零售版產品金鑰，來執行 **DISM /online /Set-Edition:\<版本識別碼\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**。 伺服器將重新啟動兩次。

對於 Windows Server 2016 Standard 的評估版，您也可以使用這個相同的命令及適當的產品金鑰，以一個步驟轉換成 Windows Server 2016 Datacenter 的零售版。

> [!TIP] 
> 如需 Dism.exe 的詳細資訊，請參閱 [DISM 命令列選項](https://go.microsoft.com/fwlink/?LinkId=192466) (英文)。

## <a name="converting-a-current-retail-edition-to-a-different-current-retail-edition"></a>將目前的零售版轉換為不同的目前零售版

安裝 Windows Server 2016 之後，您可以隨時執行安裝程式來修復安裝 (有時也稱為就地修復)，或者，在特定情況下，將其轉換為不同的版本。
您可以在任何版本的 Windows Server 2016 上執行安裝程式來執行就地修復；結果會是您一開始使用的相同版本。

以 Windows Server 2016 Standard 為例，您可以將系統轉換為 Windows Server 2016 Datacenter，如下所示：從提升權限的命令提示字元，使用命令 **DISM /online /Get-CurrentEdition**判斷目前的版本名稱。 就 Windows Server 2016 Standard 而言，這會是 `ServerStandard`。 執行命令 **DISM /online /Get-TargetEditions**，以取得可行升級目標版本的識別碼。 記下此版本識別碼，這是版本名稱的簡短形式。 然後，提供目標的版本識別碼及其零售版產品金鑰，以執行 **DISM /online /Set-Edition:\<版本識別碼\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**。 伺服器將重新啟動兩次。

## <a name="converting-a-current-retail-version-to-a-current-volume-licensed-version"></a>將目前的零售版轉換為目前的大量授權版

安裝 Windows Server 2016 之後，您可以隨時在零售版、大量授權版或 OEM 版本之間自由地轉換。 在此轉換期間，會維持相同版本。 如果您是從試用版開始，先將它轉換成零售版本，然後您就可以如此處所述互相轉換。

若要這樣做，請從提升權限的命令提示字元執行：**slmgr /ipk \<金鑰\>**

其中 \<金鑰\> 是適當的大量授權、零售或 OEM 產品金鑰。
