---
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 14dd373b71c1be2f1a4ff157b9c631530532fc80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837939"
---
# <a name="iscsi-target-server-overview"></a>iSCSI 目標伺服器概觀

適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題提供 iSCSI 目標伺服器，可讓您透過 iSCSI 通訊協定提供儲存體的 Windows Server 中的角色服務的簡短概觀。 這是適用於您的 Windows 伺服器上提供儲存體的存取，無法透過原生的 Windows 檔案共用通訊協定，SMB 通訊的用戶端。

iSCSI 目標伺服器適合下列各項：

* **網路與無磁碟開機**   使用支援開機的網路介面卡或軟體載入器，您可以部署數百部無磁碟伺服器。 有了 iSCSI 目標伺服器可快速部署。 在 Microsoft 內部測試中，於 34 分鐘內部署了 256 部電腦。 透過使用差異虛擬硬碟，您可以將用於作業系統映像的儲存空間節省高達 90%。 這在大規模部署相同作業系統映像時非常有用，例如執行 Hyper-V 或高效能運算 (HPC) 叢集的虛擬機器。

* **伺服器應用程式儲存體**   某些應用程式需要區塊存放區。 iSCSI 目標伺服器可為這些應用程式提供持續可用的區塊儲存區。 由於存放裝置可以從遠端存取，因此也能合併中央或分公司位置的區塊存放裝置。

* **異質儲存區**   iSCSI 目標伺服器支援非 Microsoft iSCSI 啟動器，因此可輕鬆共用儲存體，在混合的軟體環境中的伺服器上。

* **開發、 測試、 示範和實驗室環境**   啟用 iSCSI 目標伺服器時，執行 Windows Server 作業系統的電腦會成為網路存取的區塊存放裝置。 這對於在存放區域網路 (SAN) 進行部署前的應用程式測試而言非常有用。

## <a name="block-storage-requirements"></a>區塊存放裝置需求

可讓 iSCSI 目標伺服器提供使用現有乙太網路的區塊存放區。 不需要其他硬體。 如果高可用性是一個很重要的條件，請考慮設定高可用性叢集。 您將會需要叢集共用存放區以獲得高可用性，可以是硬體光纖通道存放區，或是序列連結 SCSI (SAS) 存放裝置陣列。

如果您啟用客體叢集，需要提供區塊儲存區。 執行 Windows Server 軟體與 iSCSI 目標伺服器的任何伺服器都可以提供區塊存放裝置。

## <a name="see-also"></a>另請參閱

[iSCSI 目標區塊存放裝置，How To](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848268(v%3dws.11))  
[新功能 Windows Server 中 iSCSI 目標伺服器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn305893(v%3dws.11))

