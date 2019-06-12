---
title: 系統需求
description: 全新安裝時，每種安裝選項之儲存體、CPU 網路、記憶體、RAM 的最低需求。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8b42d7-9fe5-4efe-9ea1-ace2131fe068
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: d089af3562467aa1c222b17d9a1ad69d9c1b5008
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810591"
---
# <a name="system-requirements"></a>系統需求

>適用於：Windows Server （半年通道），Windows Server 2016 

本主題說明執行 Windows Server&reg; 2016 或 Windows Server 版本 1709 的最低系統需求。

> [!NOTE]  
> 此版本建立使用全新安裝。  

> [!NOTE]  
> 如果在安裝的時候，您選擇 [伺服器核心] 選項進行安裝，您應該要注意這不會安裝任何 GUI 元件，且您將無法使用伺服器管理員來安裝或取消安裝這些 GUI 元件。 如果您需要 GUI 功能，請務必在您安裝 Windows Server 2016 時選擇 [含桌面體驗的伺服器] 選項。 如需詳細資訊，請參閱[安裝 Nano Server](Getting-Started-with-Nano-Server.md)  


## <a name="review-system-requirements"></a>檢閱系統需求  
下列是 Windows Server 2016 的預估系統需求。 如果您的電腦低於「最低」需求，則無法正確安裝本產品。 視系統設定以及安裝的應用程式和功能而定，實際需求會有所不同。

除非另行指定，否則這些最低系統需求適用於所有的安裝選項 ([Server Core]、[含桌面體驗的伺服器] 和 [Nano 伺服器]) 以及 Standard 和 Datacenter 版本。  

> [!IMPORTANT]  
> 因為有太多可能的部署方式，所以不太可能提出一般通用的「建議」系統需求。 請參閱每一個要部署的伺服器角色相關文件，取得更多有關特定伺服器角色所需資源的詳細資料。 為了達到最佳效果，可執行測試部署，判斷特定部署案例的適當系統需求。  


## <a name="processor"></a>處理器  
處理器效能不僅取決於處理器的時脈頻率，也取決於處理器核心的數目及處理器快取的大小。 下列是本產品的處理器需求：  

**最低需求**：  
- 1.4 Ghz 64 位元處理器  
- 相容於 x64 指令集  
- 支援 NX 與 DEP  
- 支援 CMPXCHG16b、LAHF/SAHF 與 PrefetchW  
- 支援第二層地址翻譯 (EPT 或 NPT)  

[Coreinfo](https://technet.microsoft.com/sysinternals/cc835722.aspx)是的工具，您可以用來確認您 CPU 具有這些功能。

## <a name="ram"></a>RAM  
下列是本產品的預估 RAM 需求：  

**最低需求**：  
- 512 MB ([含桌面體驗的伺服器] 安裝選項為 2 GB)
- ECC (錯誤修正程式碼) 類型或類似的技術  

> [!IMPORTANT]  
> 如果您用最低支援的硬體參數 (一個處理器核心和 512 MB RAM) 建立虛擬機器，然後嘗試在虛擬機器安裝此版本，會造成安裝失敗。  
>   
> 若要避免此狀況，請執行下列其中一項動作：  
>   
> -   在您嘗試安裝此版本的虛擬機器上配置 800 MB 以上的 RAM。 安裝完成之後，您可以根據實際的伺服器設定來變更配置，儘量減少為 512 MB RAM。  
> -   用 SHIFT+F10 中斷此版本在虛擬機器上的開機程序。 在開啟的命令提示字元中，使用 Diskpart.exe 建立並格式化安裝磁碟分割。 執行 **Wpeutil createpagefile /path=C:\pf.sys** (假設您建立的安裝磁碟分割為 C:)。 關閉命令提示字元，繼續進行安裝。  

## <a name="storage-controller-and-disk-space-requirements"></a>儲存體控制器和磁碟空間需求  
執行 Windows Server 2016 的電腦必須包含符合 PCI Express 架構規格的儲存裝置介面卡。 伺服器上歸類為硬碟機的永續性存放裝置不能為 PATA。 針對開機、分頁或資料磁碟機，Windows Server 2016 不允許使用 ATA/PATA/IDE/EIDE。  

下列是系統磁碟分割的預估 **最小** 磁碟空間需求。  

**最低需求**：32 GB  

> [!NOTE]
> 請注意，32 GB 應視為成功安裝的*絕對最小值*。 此最小值應該可讓您以 Web 服務 (IIS) 伺服器角色在 Server Core 模式中安裝 Windows Server 2016。 使用「Server Core」模式的伺服器比使用「Server 含 GUI」模式的相同伺服器大約少 4 GB。 
> 
> 如果是下列任何情況，系統磁碟分割將需要額外的空間：  
> 
> -   如果您透過網路安裝系統。  
> -   具有 16 GB RAM 以上的電腦將需要更多的磁碟空間，以供分頁、休眠及傾印檔案使用。  

## <a name="network-adapter-requirements"></a>網路介面卡需求  

此版本中使用的網路介面卡應該包含這些功能：  

**最低需求**：  
- 可支援 Gigabit 輸送量的乙太網路介面卡  
- 符合 PCI Express 架構規格。  
- 支援開機前執行環境 (PXE)。  

支援網路偵錯 (KDNet) 的網路介面卡會很有用，但不是最低需求。   

## <a name="other-requirements"></a>其他需求  
執行此版本的電腦也必須具備下列各項：  


-   DVD 光碟機 (如果您要從 DVD 媒體安裝作業系統)  

以下項目並非必要，但是對於某些特定功能來說不可或缺：  

- 支援安全開機的 UEFI 2.3.1c 型系統與韌體  
- 信賴平台模組  

-   可支援 Super VGA (1024 x 768) 或更高解析度的圖形裝置和監視器  

-   鍵盤及 Microsoft&reg; 滑鼠 (或其他相容指標裝置)  

-   網際網路存取 (可能需要付費)  

> [!NOTE]  
> 信賴平台模組 (TPM) 晶片於安裝此版本時並非必要，但是使用特定功能 (例如 BitLocker 磁碟機加密) 時則為必要。 如果您的電腦使用 TPM，它必須符合下列需求︰  
>  
> - 硬體架構的 TPM 必須實作 TPM 規格的 2.0 版。  
> - 實作 2.0 版的 TPM 必須有一個 EK 憑證，這個憑證是由硬體廠商預先佈建至 TPM，或是可由裝置在第一次開機期間擷取。  
> - 實作 2.0 版的 TPM 必須隨附 SHA-256 PCR 記憶體組，並實作 SHA-256 的 PCR 0 至 23。 可接受 TPM 隨附可同時用於 SHA-1 與 SHA-256 度量的單一可切換 PCR 記憶體組。  
> - 關閉 TPM 的 UEFI 選項並非必要。  

## <a name="installation-of-nano-server"></a>安裝 Nano 伺服器  
如需安裝 Windows Server 2016 作為 Nano 伺服器的詳細步驟，請參閱[安裝 Nano Server](Getting-Started-with-Nano-Server.md)。

## <a name="additional-resources"></a>其他資源
- [Windows 處理器需求](https://docs.microsoft.com/windows-hardware/design/minimum/windows-processor-requirements)
- [Windows Server 2016 Standard 和 Datacenter 版本的比較](https://docs.microsoft.com/windows-server/get-started/2016-edition-comparison)
- [Windows 10 系統需求](https://www.microsoft.com/windows/windows-10-specifications#system-specifications)
- [下載 Windows Server 2016 授權資料工作表](http://download.microsoft.com/download/7/2/9/7290EA05-DC56-4BED-9400-138C5701F174/WS2016LicensingDatasheet.pdf)
