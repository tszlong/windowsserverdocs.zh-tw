---
description: 深入瞭解： iSCSI 目標伺服器總覽
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 71c2e3bb7ef125e79dadab331786b5c53fa0800d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041246"
---
# <a name="iscsi-target-server-overview"></a>iSCSI 目標伺服器總覽

適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供 iSCSI 目標伺服器的簡介，這是 Windows Server 中的角色服務，可讓您透過 iSCSI 通訊協定提供存放裝置。 對於無法透過原生 Windows 檔案共用通訊協定（SMB）進行通訊的用戶端，在 Windows server 上提供存放裝置的存取權時，這非常有用。

iSCSI 目標伺服器適合下列各項：

* **網路與無磁碟開機**   使用支援開機的網路介面卡或軟體載入器，就可以部署數百部無磁碟伺服器。 有了 iSCSI 目標伺服器可快速部署。 在 Microsoft 內部測試中，於 34 分鐘內部署了 256 部電腦。 透過使用差異虛擬硬碟，您可以將用於作業系統映像的儲存空間節省高達 90%。 這在大規模部署相同作業系統映像時非常有用，例如執行 Hyper-V 或高效能運算 (HPC) 叢集的虛擬機器。

* **伺服器應用程式存放區**   某些應用程式需要區塊存放區。 iSCSI 目標伺服器可為這些應用程式提供持續可用的區塊儲存區。 由於存放裝置可以從遠端存取，因此也能合併中央或分公司位置的區塊存放裝置。

* **異質儲存區**   iSCSI 目標伺服器支援非 Microsoft iSCSI 啟動器，因此可方便在混合的軟體環境中共用伺服器上的存放裝置。

* **開發、測試、示範以及實驗室環境**   當 iSCSI 目標伺服器啟用時，執行 Windows Server 作業系統的電腦會成為可從網路存取的區塊存放裝置。 這對於在存放區域網路 (SAN) 進行部署前的應用程式測試而言非常有用。

## <a name="block-storage-requirements"></a>區塊存放裝置需求

可讓 iSCSI 目標伺服器提供使用現有乙太網路的區塊存放區。 不需要其他硬體。 如果高可用性是一個很重要的條件，請考慮設定高可用性叢集。 您將會需要叢集共用存放區以獲得高可用性，可以是硬體光纖通道存放區，或是序列連結 SCSI (SAS) 存放裝置陣列。

如果您啟用客體叢集，需要提供區塊儲存區。 執行 Windows Server 軟體與 iSCSI 目標伺服器的任何伺服器都可以提供區塊存放裝置。

## <a name="see-also"></a>另請參閱

[ISCSI 目標區塊存放裝置，How To](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848268(v%3dws.11)) 
[Windows Server 中 ISCSI 目標伺服器的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn305893(v%3dws.11))
