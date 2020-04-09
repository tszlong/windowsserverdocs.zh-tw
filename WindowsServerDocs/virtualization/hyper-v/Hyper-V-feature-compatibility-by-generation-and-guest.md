---
title: 由世代和來賓的 hyper-v 功能相容性
description: 列出與主要 Hyper-v 功能相容的世代和作業系統
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: kbdazure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 7212cf21858c8031db0a72efa8d79d78974b0309
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853231"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>由世代和來賓的 hyper-v 功能相容性

>適用於︰Windows Server 2016
  
本文中的表格說明與部分 Hyper-v 功能相容的世代和作業系統，並依類別分組。 一般來說，您可以透過執行最新作業系統的第2代虛擬機器，取得功能的最佳可用性。  
  
請記住，某些功能依賴硬體或其他基礎結構。 如需硬體詳細資料，請參閱[Windows Server 2016 上的 Hyper-v 系統需求](System-requirements-for-Hyper-V-on-Windows.md)。 在某些情況下，功能可以與任何支援的客體作業系統搭配使用。 如需支援哪些作業系統的詳細資訊，請參閱：  
  
* [支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [支援的 Windows 客體作業系統](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>可用性和備份  
  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
檢查點 | 1和2 | 任何支援的來賓  
來賓叢集 | 1和2 | 執行叢集感知應用程式並已安裝 iSCSI 目標軟體的來賓  
複寫 | 1和2 | 任何支援的來賓  
網域控制站 | 1和2 | 任何支援的 Windows Server 來賓僅使用生產檢查點。 請參閱[支援的 Windows Server 客體作業系統](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>計算  
  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
動態記憶體 | 1和2 | 支援來賓的特定版本。 如需比 Windows Server 2016 和 Windows 10 更舊的版本，請參閱[hyper-v 動態記憶體總覽](https://technet.microsoft.com/library/hh831766.aspx)。  
熱新增/移除記憶體 | 1和2 | Windows Server 2016、Windows 10  
虛擬 NUMA | 1和2 | 任何支援的來賓  
  
## <a name="development-and-test"></a>開發和測試  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
COM/序列通訊埠 | 1和2 <br>**注意：** 針對第2代，請使用 Windows PowerShell 進行設定。 如需詳細資訊，請參閱[新增內核偵錯工具的 COM 埠](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging)。 | 任何支援的來賓  
  
## <a name="mobility"></a>移動性  
  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
即時移轉  | 1和2 |  任何支援的來賓  
Import/export (匯入/匯出) | 1和2 |  任何支援的來賓  
  
## <a name="networking"></a>網路功能  
  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
虛擬網路介面卡的熱新增/移除 | 2 | 任何支援的來賓  
舊版虛擬網路介面卡 | 1 | 任何支援的來賓  
單一根目錄輸入/輸出虛擬化（SR-IOV） | 1和2 | 64位 Windows 來賓，從 Windows Server 2012 和 Windows 8 開始。  
虛擬機器多佇列（VMMQ） | 1和2  | 任何支援的來賓  
  
## <a name="remote-connection-experience"></a>遠端連線體驗  
  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
離散裝置指派（DDA） | 1和2 | Windows Server 2016、Windows Server 2012 R2 （僅限已安裝 Update 3133690）、Windows 10 <br> **注意：** 如需更新3133690的詳細資訊，請參閱[這](https://support.microsoft.com/kb/3133690)篇支援檔。  
增強的工作階段模式 | 1和2 | Windows Server 2016、Windows Server 2012 R2、Windows 10 和 Windows 8.1，並已啟用遠端桌面服務 <br>**注意**：您可能也需要設定主機。 如需詳細資訊，請參閱[在 hyper-v 虛擬機器上使用本機資源搭配 VMConnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)。  
RemoteFx | 1和2 | 從 Windows 8 開始，32位和64位 Windows 版本上的第1代。 <br> 64位 Windows 10 版本上的第2代  
  
## <a name="security"></a>安全性  
  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
安全開機 | 2 | **Linux**： Ubuntu 14.04 和更新版本、SUSE Linux Enterprise Server 12 和更新版本、Red Hat Enterprise Linux 7.0 和更新版本，以及 CentOS 7.0 和更新版本<br>**Windows**：可在第2代虛擬機器上執行的所有支援版本  
受防護的虛擬機器 | 2 | **Windows**：可在第2代虛擬機器上執行的所有支援版本  
  
## <a name="storage"></a>儲存體  
  
功能  | 代數 | 客體作業系統  
------------- | ------------- | -----------  
共用虛擬硬碟（僅限 VHDX） | 1和2  | Windows Server 2016、Windows Server 2012 R2、Windows Server 2012  
SMB3 | 1和2 | 所有支援 SMB3 的  
儲存空間直接存取 | 2 | Windows Server 2016  
虛擬光纖通道 | 1和2 | Windows Server 2016、Windows Server 2012 R2、Windows Server 2012  
VHDX 格式 | 1和2 | 任何支援的來賓   
  
  
  
  
    


