---
title: 適用於 Windows Server 上的 HYPER-V 系統需求
description: 列出在 Windows Server 中的 HYPER-V 的硬體與韌體需求
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 97fb1b9003705ba8ad26c2b3e71eda34e88642ee
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812614"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>適用於 Windows Server 上的 HYPER-V 系統需求

>適用於：Windows Server 2016、 Microsoft HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

HYPER-V 有特定的硬體需求，部分 HYPER-V 功能有其他需求。 使用這篇文章中的詳細資料，可決定讓您可以使用 HYPER-V 您計劃的方式，您的系統必須符合哪些需求。 接著，檢閱[Windows Server catalog](https://www.windowsservercatalog.com/)。 請注意，適用於 HYPER-V 的需求超過一般的最低需求 Windows Server 2016，因為虛擬化環境需要更多運算資源。

如果您已經在使用 HYPER-V，它可能是您可以使用您現有的硬體。 一般硬體需求尚未有重大變更，從 Windows Server 2012 R2。  但是，您必須使用受防護的虛擬機器或不連續的裝置指派至較新的硬體。 這些功能會依賴特定硬體支援，如下所述。 除此之外，其他硬體中的主要差異是，第二層位址轉譯 (SLAT) 現在改為需要建議使用。

如需 HYPER-V，例如執行中的虛擬機器，數目最大支援的組態詳細資訊，請參閱[規劃 Windows Server 2016 中的 HYPER-V 延展性](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。 您可以在您的虛擬機器執行的作業系統清單涵蓋[支援 Windows 的 Windows Server 上的 HYPER-V 客體作業系統](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)。

## <a name="general-requirements"></a>一般要求條件

不論您想要使用的 HYPER-V 功能，您將需要：

- 與第二層位址轉譯 (SLAT) 的 64 位元處理器。 若要安裝 HYPER-V 虛擬化元件，例如 Windows hypervisor，處理器必須具有 SLAT。 不過，它不需要安裝適用於 Windows PowerShell 的 HYPER-V 管理工具，例如虛擬機器連線 (VMConnect) 」、 「 HYPER-V 管理員 中和 「 HYPER-V cmdlet。 請參閱以下的 < 如何檢查 HYPER-V 需求 >，以找出是否有您的處理器具有 SLAT。

- VM 監視器模式擴充功能

- 足夠的記憶體-計劃*至少*4 GB 的 RAM。 更多的記憶體是更好。 主機和您想要在同一時間執行的所有虛擬機器，您將需要足夠的記憶體。

- 虛擬化支援在 BIOS 或 UEFI 中開啟：

  - 硬體協助虛擬化。 這是可用的處理器虛擬化選項-特別是與 Intel Virtualization Technology (Intel VT) 或 AMD 虛擬化 (AMD-V) 技術的處理器。

  - 必須提供並啟用硬體強制的資料執行防止 (DEP)。 針對 Intel 系統，這是 XD 位元 （執行停用位元）。 適用於 AMD 系統，這是 NX 位元 （無執行位元）。

## <a name="how-to-check-for-hyper-v-requirements"></a>如何檢查 HYPER-V 需求

開啟 Windows PowerShell 或命令提示字元並輸入：

```cmd
Systeminfo.exe
```

捲動到 檢閱報告的 HYPER-V 需求 > 一節。

## <a name="requirements-for-specific-features"></a>特定功能的需求

以下是不連續的裝置指派和受防護的虛擬機器的需求。 如需這些功能的描述，請參閱 <<c0> [ 的 Windows Server 上的 HYPER-V 新功能](What-s-new-in-Hyper-V-on-Windows.md)。

### <a name="discrete-device-assignment"></a>不連續的裝置指派

**主機**需求會在 HYPER-V 中的 SR-IOV 虛擬功能現有需求類似。

- 處理器必須具有任一 Intel 的擴充頁面資料表 (EPT) 或 AMD 的巢狀網頁表格 (NPT)。

- 晶片組必須具備：

  - 插斷重新對應-Intel vt-d 插斷重新對應功能 (VT d2) 或 AMD I/O 記憶體管理單元 (I/O MMU) 的任何版本。

  - DMA 重新對應-Intel vt-d 排入佇列的失效或任何 AMD I/O MMU。

  - PCI express 的存取控制服務 (ACS) 根連接埠。

- 韌體資料表必須公開到 Windows hypervisor I/O MMU。 請注意，這項功能可能會關閉在 BIOS 或 UEFI。 如需指示，請參閱硬體文件或連絡硬體製造商。

**裝置**需要 GPU 或非揮發性記憶體 express (NVMe)。 適用於 GPU，只有某些裝置支援不連續的裝置指派。 若要確認，請參閱硬體文件或連絡硬體製造商。 如需這項功能的詳細資訊，包括如何使用它，並考量，請參閱文章 「[離散的裝置指派-描述和背景](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)」 在虛擬化部落格。

### <a name="shielded-virtual-machines"></a>受防護的虛擬機器

這些虛擬機器使用虛擬化型安全性，並可從 Windows Server 2016 開始。

**主機**需求如下：

- UEFI 2.3.1c-支援安全、 嚴測開機

  下列兩個是選用的虛擬化型安全性的一般情況下，但如果您想要保護這些功能提供主應用程式所需：

- TPM v2.0-保護平台安全性資產
- Iommu 所致 (Intel VT-D)-讓 hypervisor 可以提供直接記憶體存取 (DMA) 保護

**虛擬機器**需求如下：

- 第 2 代
- Windows Server 2012 或更新版本做為客體作業系統

