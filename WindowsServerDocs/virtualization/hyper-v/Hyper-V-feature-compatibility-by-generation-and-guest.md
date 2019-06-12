---
title: 世代與客體各自的 HYPER-V 功能相容性
description: 列出的層代和 HYPER-V 的主要功能與相容的作業系統
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 5950a75da4569979794a5848bd41ab349dc34676
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812661"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>世代與客體各自的 HYPER-V 功能相容性

>適用於：Windows Server 2016
  
這篇文章中的資料表會顯示與部分 HYPER-V 功能，依類別分組相容的作業系統與層代。 一般情況下，您會收到與第 2 代虛擬機器執行最新的作業系統功能的最佳可用性。  
  
請記住，某些功能依賴硬體或其他基礎結構。 硬體的詳細資訊，請參閱 <<c0> [ 適用於 Windows Server 2016 上的 HYPER-V 系統需求](System-requirements-for-Hyper-V-on-Windows.md)。 在某些情況下，一項功能可以搭配任何支援的客體作業系統。 支援作業系統的詳細資訊，請參閱：  
  
* [支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [支援的 Windows 客體作業系統](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>可用性和備份  
  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
檢查點 | 1 和 2 | 任何支援的客體  
客體叢集 | 1 和 2 | 來賓可執行叢集感知的應用程式，並已安裝的 iSCSI 目標軟體  
複寫 | 1 和 2 | 任何支援的客體  
網域控制站 | 1 和 2 | 任何支援的 Windows Server 來賓使用僅為生產檢查點。 請參閱[客體作業系統支援的 Windows Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>計算  
  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
動態記憶體 | 1 和 2 | 特定版本的支援來賓。 請參閱[HYPER-V 動態記憶體概觀](https://technet.microsoft.com/library/hh831766.aspx)版本早於 Windows Server 2016 和 Windows 10。  
熱新增/移除記憶體 | 1 和 2 | Windows Server 2016 中，Windows 10  
虛擬 NUMA | 1 和 2 | 任何支援的客體  
  
## <a name="development-and-test"></a>開發和測試  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
COM/序列通訊埠 | 1 和 2 <br>**注意：** 針對層代 2 中，使用 Windows PowerShell 來設定。 如需詳細資訊，請參閱 <<c0> [ 新增的 COM 連接埠的核心偵錯](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging)。 | 任何支援的客體  
  
## <a name="mobility"></a>行動力  
  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
即時移轉  | 1 和 2 |  任何支援的客體  
匯入/匯出 | 1 和 2 |  任何支援的客體  
  
## <a name="networking"></a>網路功能  
  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
熱新增/移除虛擬網路介面卡 | 2 | 任何支援的客體  
傳統虛擬網路介面卡 | 1 | 任何支援的客體  
單一根目錄輸入/輸出虛擬化 (SR-IOV) | 1 和 2 | 64 位元 Windows 來賓，從 Windows Server 2012 和 Windows 8 開始。  
虛擬機器多個佇列 (VMMQ) | 1 和 2  | 任何支援的客體  
  
## <a name="remote-connection-experience"></a>遠端連線體驗  
  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
不連續的裝置指派 (DDA) | 1 和 2 | Windows Server 2016 中，只能搭配 Update 3133690 的 Windows Server 2012 R2 安裝，Windows 10 <br> **注意：** 如需更新 3133690 的詳細資訊，請參閱[這](https://support.microsoft.com/kb/3133690)技術支援文件。  
增強的工作階段模式 | 1 和 2 | 啟用 Windows Server 2016、 Windows Server 2012 R2、 Windows 10 和 Windows 8.1，使用遠端桌面服務 <br>**注意**：您可能需要也設定主機。 如需詳細資訊，請參閱 <<c0> [ 搭配 VMConnect 的 HYPER-V 虛擬機器上使用本機資源](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)。  
RemoteFx | 1 和 2 | 從 Windows 8 的 32 位元和 64 位元的 Windows 版本上第 1 代。 <br> 在 64 位元 Windows 10 版本上的第 2 代  
  
## <a name="security"></a>安全性  
  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
安全開機 | 2 | **Linux**:Ubuntu 14.04 和更新版本、 SUSE Linux Enterprise Server 12 和更新版本、 Red Hat Enterprise Linux 7.0 和更新版本和 CentOS 7.0 和更新版本<br>**Windows**:所有支援的版本，可在第 2 代虛擬機器上執行  
受防護的虛擬機器 | 2 | **Windows**:所有支援的版本，可在第 2 代虛擬機器上執行  
  
## <a name="storage"></a>儲存體  
  
功能  | 產生 | 客體作業系統  
------------- | ------------- | -----------  
共用虛擬硬碟 (VHDX 只) | 1 和 2  | Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012  
SMB3 | 1 和 2 | 所有支援 SMB3  
儲存空間直接存取 | 2 | Windows Server 2016  
虛擬光纖通道 | 1 和 2 | Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012  
VHDX 格式 | 1 和 2 | 任何支援的客體   
  
  
  
  
    


