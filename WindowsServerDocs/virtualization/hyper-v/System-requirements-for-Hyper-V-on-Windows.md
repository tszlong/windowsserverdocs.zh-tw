---
title: Windows Server 上的 Hyper-v 系統需求
description: 列出 Windows Server 中 Hyper-v 的硬體和固件需求
manager: dongill
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: kbdazure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: d0cbbc79fe1dc942dfe79ca9dbe81769dd112730
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997617"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-v 系統需求

>適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

Hyper-v 具有特定的硬體需求，而某些 Hyper-v 功能則有額外的需求。 請使用本文中的詳細資料來決定您的系統必須符合哪些需求，才能以您規劃的方式使用 Hyper-v。 然後，請參閱[Windows Server catalog](https://www.windowsservercatalog.com/)。 請記住，Hyper-v 的需求超過 Windows Server 2016 的一般最低需求，因為虛擬化環境需要更多運算資源。

如果您已經在使用 Hyper-v，很可能可以使用現有的硬體。 Windows Server 2012 R2 並未大幅改變一般硬體需求。  但是，您將需要較新的硬體，才能使用受防護的虛擬機器或個別的裝置指派。 這些功能依賴特定硬體支援，如下所述。 此外，硬體的主要差異在於第二層位址轉譯 (SLAT) 現在是必要的，而不是建議的做法。

如需 Hyper-v 支援的最大設定（例如執行中虛擬機器的數目）的詳細資訊，請參閱[規劃 Windows Server 2016 中的 hyper-v 擴充性](./plan/plan-hyper-v-scalability-in-windows-server.md)。 您可以在虛擬機器中執行的作業系統清單涵蓋于[Windows Server 上的 Hyper-v 支援的 windows 客體作業系統](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)。

## <a name="general-requirements"></a>一般需求

無論您想要使用的 Hyper-v 功能為何，都需要：

- 具有第二層位址轉譯 (SLAT) 的64位處理器。 若要安裝 Hyper-v 虛擬化元件（例如 Windows 虛擬程式），處理器必須具有 SLAT。 不過，不需要安裝 Hyper-v 管理工具，例如虛擬機器連線 (VMConnect) 、Hyper-v 管理員，以及適用于 Windows PowerShell 的 Hyper-v Cmdlet。 請參閱下方的「如何檢查 Hyper-v 需求」，以找出您的處理器是否有 SLAT。

- VM 監視器模式擴充功能

- 足夠的記憶體-規劃*至少*4 GB 的 RAM。 記憶體愈多越好。 您需要有足夠的記憶體供主機和您想要同時執行的所有虛擬機器。

- 虛擬化支援已在 BIOS 或 UEFI 中開啟：

  - 硬體協助虛擬化。 這適用于包含虛擬化選項的處理器，特別是 intel 虛擬化技術的處理器， (Intel VT) 或 AMD Virtualization (AMD) 技術。

  - 必須提供並啟用硬體強制的資料執行防止 (DEP)。 針對 Intel 系統，這是 (執行停用位) 的 XD 位。 針對 AMD 系統，這是 NX 位， (沒有執行位) 。

## <a name="how-to-check-for-hyper-v-requirements"></a>如何檢查 Hyper-v 需求

開啟 Windows PowerShell 或命令提示字元，然後輸入：

```cmd
Systeminfo.exe
```

流覽至 [Hyper-v 需求] 區段以查看報告。

## <a name="requirements-for-specific-features"></a>特定功能的需求

以下是個別裝置指派和受防護虛擬機器的需求。 如需這些功能的說明，請參閱[Windows Server 上的 hyper-v 的新](What-s-new-in-Hyper-V-on-Windows.md)功能。

### <a name="discrete-device-assignment"></a>離散裝置指派

**主機**需求與 HYPER-V 中 sr-iov 功能的現有需求類似。

- 處理器必須有 Intel 的擴充頁面資料表 (EPT) 或 AMD 的嵌套頁面資料表 (NPT) 。

- 晶片必須具有：

  - 中斷重新對應-具有中斷對應功能的 Intel VT (VT-d2) 或任何版本的 AMD i/o Memory 管理單元 (i/o MMU) 。

  - DMA 重新對應-具有佇列失效或任何 AMD i/o MMU 的 Intel VT。

  - 在 PCI Express 根埠上 (ACS) 的存取控制服務。

- 固件資料表必須向 Windows 虛擬機器公開 i/o MMU。 請注意，這項功能可能會在 UEFI 或 BIOS 中關閉。 如需相關指示，請參閱硬體檔或聯絡您的硬體製造商。

**裝置**需要 GPU 或非動態記憶體 Express (NVMe) 。 針對 GPU，只有特定裝置支援個別裝置指派。 若要確認，請參閱硬體檔或聯絡您的硬體製造商。 如需這項功能的詳細資訊，包括如何使用它和考慮，請參閱虛擬化 blog 中的「[離散裝置指派--描述和背景](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)」文章。

### <a name="shielded-virtual-machines"></a>受防護的虛擬機器

這些虛擬機器依賴虛擬化安全性，並從 Windows Server 2016 開始提供。

**主機**需求如下：

- UEFI 2.3.1 c-支援安全、測量的開機

  以下兩個選項對於一般虛擬化安全性而言是選擇性的，但如果您想要保護這些功能所提供的保護，則主機需要：

- TPM v2.0-保護平臺安全性資產
-  (Intel VT) 的 IOMMU，讓虛擬機器能夠提供直接記憶體存取 (DMA) 保護

**虛擬機器**需求如下：

- 第 2 代
- Windows Server 2012 或更新版本做為客體作業系統