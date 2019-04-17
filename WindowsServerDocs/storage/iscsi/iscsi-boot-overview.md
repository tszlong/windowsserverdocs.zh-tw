---
ms.assetid: 134840f3-c416-4a10-ad73-ef7855b206f7
title: "iSCSI 目標開機概觀"
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: MicGray-MS
manager: dongill
ms.author: micgray
ms.date: 10/11/2016
ms.openlocfilehash: 958d8a71e6fe62ec9d256be132aef4edf00942db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="iscsi-target-boot-overview"></a>iSCSI 目標開機概觀

> 適用於：Windows Server (半年度管道)、Windows Server 2016

Windows Server 中的 iSCSI 目標伺服器可透過儲存在集中位置的單一作業系統映像，將數百部電腦開機。 這個做法可以提升效率、管理性、可用性以及安全性。  
  
## <a name="BKMK_OVER"></a>功能描述  
您可以透過差異虛擬硬碟 \(VHDs\)，使用單一作業系統映像 \(「母片映像」\) 執行多達 256 部電腦的開機。 例如，假設您使用大約為 20 GB 的作業系統映像部署了 Windows Server，而您使用兩個鏡像磁碟機做為開機磁碟區。 僅要將 256 部電腦的作業系統映像開機，您將需要大約 10 TB 的存放空間。 利用 iSCSI 目標伺服器，您會將 40 GB 供作業系統基礎映像使用，以及將 2 GB 供每個伺服器執行個體差異虛擬硬碟使用，總共 552 GB 用於作業系統映像。 相較於單獨儲存作業系統映像，這可以節省 90% 以上的儲存空間。  
  
## <a name="BKMK_APP"></a>實際應用  
使用受到控制的作業系統映像可提供下列優點：  
  
**更安全且容易管理。** 有些企業要求以實際鎖定在集中位置的儲存裝置來保護資料。 在這種情況下，伺服器會從遠端存取資料，包括作業系統映像。 有了 iSCSI 目標伺服器，系統管理員可以集中管理作業系統開機映像，並控制要在母片映像中使用的應用程式。  
  
**快速部署。** 因為母片映像是使用 Sysprep 準備，當電腦從母片映像開機時，會略過 Windows 安裝期間所進行的檔案複製和安裝階段，直接進入自訂階段。 在 Microsoft 內部測試中，於 34 分鐘內部署了 256 部電腦。  
  
**快速復原。** 因為作業系統映像是裝載在執行 iSCSI 目標伺服器的電腦上，所以如果需要更換無磁碟用戶端，新的電腦可以指向作業系統映像，然後立即開機。  
  
> [!NOTE]  
> 許多廠商提供存放區域網路 \(SAN\) 開機解決方案，可供 iSCSI 目標伺服器在採用商用硬體的 Windows Server 中使用。  
  
## <a name="BKMK_HARD"></a>硬體需求  
iSCSI 目標伺服器不需要特殊硬體即可進行功能上的驗證。 在包含大規模部署的資料中心，應該針對特定硬體驗證設計。 給您作參考，Microsoft 內部測試指出 256 電腦的部署需要在 RAID 10 設定中有 24 個 15000 RPM 的磁碟用於儲存體。 最佳的網路頻寬為 10 GB。 一般預估為每 1 GB 的網路介面卡供 60 部 iSCSI 開機伺服器使用。  
  
此案例中不需要網路介面卡，並且可以使用軟體開機載入器 \(例如 iPXE 開放來源開機韌體\)。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
iSCSI 目標伺服器可以隨著伺服器管理員中的檔案和 iSCSI 服務角色服務安裝。

> [!NOTE]
> 不支援從 iSCSI (無論是從 Windows iSCSI 目標伺服器或協力廠商的目標實作) 將 Nano 伺服器開機。

## <a name="see-also"></a>另請參閱
* [iSCSI Target Server](https://technet.microsoft.com/library/hh848272(v=ws.11).aspx)
* [iSCSI 啟動器 Cmdlet](https://technet.microsoft.com/library/hh826099(v=wps.640).aspx)
* [iSCSI 目標伺服器 Cmdlet](https://technet.microsoft.com/library/jj612803(v=wps.630).aspx)