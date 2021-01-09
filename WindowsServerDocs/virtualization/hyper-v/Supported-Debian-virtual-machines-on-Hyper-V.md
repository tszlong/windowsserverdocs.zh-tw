---
title: Hyper-V 上支援的 Debian 虛擬機器
description: 列出每個版本中所包含的 Debin/Linux integration services 和功能
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/08/2021
ms.openlocfilehash: d98f53c4adec614d8cb132f4c13c1d3ab21fe583
ms.sourcegitcommit: 209b0995a11c89bb9ece3db0d48a35d7ba5bbd9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2021
ms.locfileid: "98053611"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Hyper-V 上支援的 Debian 虛擬機器

>適用于： Azure Stack HCI、版本 20H2;Windows Server 2019、Hyper-v Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows 10、Windows 8。1

下列功能發佈對應會指出每個版本中的功能。 每個散發的已知問題和因應措施會列在資料表之後。

## <a name="table-legend"></a>資料表圖例

* **內建的內建** 元件包含在此 Linux 發行版本中。 Microsoft 提供的 .LIS 下載套件不適用於此散發套件，因此請勿安裝。 內建的 .LIS (的核心模組版本號碼如 **lsmod** 所示，例如) 與 Microsoft 提供的 .lis 下載套件上的版本號碼不同。 不相符並不表示內建的 .LIS 已過期。

* &#10004;-可用功能

*  (*空白*) -功能無法使用

| **功能**                                                                                                                                  | **Windows Server 作業系統版本** | **10.0-10.3 (buster)** | **9.0-9.12 (延展)** | **8.0-8.11 (jessie)** | **7.0-7.11 (wheezy)** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **可用性**                                                                                                                             |                                             | 內建              | 內建              | 內建              | 內建 (附注 5)      |
| **[核心](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019、2016、2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Windows Server 2016 精確時間                                                                                                            | 2019、2016                                  | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| **[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| 大型訊框                                                                                                                                 | 2019、2016、2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| VLAN 標記和中繼                                                                                                                    | 2019、2016、2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 即時移轉                                                                                                                               | 2019、2016、2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 靜態 IP 插入                                                                                                                          | 2019、2016、2012 R2                   |                       |                       |                       |                       |
| vRSS                                                                                                                                         | 2019、2016、2012 R2                         | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| TCP 分割和總和檢查碼卸載                                                                                                       | 2019、2016、2012 R2          | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| SR-IOV                                                                                                                                       | 2019、2016                                  | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| **[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| VHDX 調整大小                                                                                                                                  | 2019、2016、2012 R2                         | &#10004; 附注1       | &#10004; 附注1       | &#10004; 附注1       | &#10004; 附注1       |
| 虛擬光纖通道                                                                                                                        | 2019、2016、2012 R2                         |                       |                       |                       |                       |
| 即時虛擬機器備份                                                                                                                  | 2019、2016、2012 R2                         | &#10004; Note2 | &#10004; Note2 | &#10004; Note2 | &#10004; Note2 |
| TRIM 支援                                                                                                                                 | 2019、2016、2012 R2                         | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| SCSI WWN                                                                                                                                     | 2019、2016、2012 R2                         | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| **[記憶體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| PAE 核心支援                                                                                                                           | 2019、2016、2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 設定 MMIO 間距                                                                                                                    | 2019、2016、2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 動態記憶體-Hot-Add                                                                                                                     | 2019、2016、2012 R2                   | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| 動態記憶體-佔用                                                                                                                  | 2019、2016、2012 R2                   | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| 執行時間記憶體大小調整                                                                                                                        | 2019、2016                                  | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| **[視訊](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Hyper-v 特定的影片裝置                                                                                                                | 2019、2016、2012 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| Key-Value 配對                                                                                                                               | 2019、2016、2012 R2          | &#10004; 附注2       | &#10004; 附注2       | &#10004; 附注2       |                       |
| 非遮罩式插斷                                                                                                                       | 2019、2016、2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| 從主機到來賓的檔案複製                                                                                                                 | 2019、2016、2012 R2                         | &#10004; 附注2       | &#10004; 附注2       | &#10004; 附注2       |                       |
| lsvmbus 命令                                                                                                                              | 2019、2016、2012 R2          |                       |                       |                       |                       |
| Hyper-v 通訊端                                                                                                                              | 2019、2016                                  | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| PCI 傳遞/DDA                                                                                                                          | 2019、2016                                  | &#10004; 附注4       | &#10004; 附注4       |                       |                       |
| **[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| 使用 UEFI 開機                                                                                                                              | 2019、2016、2012 R2                         | &#10004; 附注3       | &#10004; 附注3       | &#10004; 附注3       |                       |
| 安全開機                                                                                                                                  | 2019、2016                                  | &#10004;              |                       |                       |                       |


## <a name="notes"></a>注意

1. 不支援在大於2TB 的 Vhd 上建立檔案系統。

2. 從 Debian 8.3 開始，手動安裝的 Debian 套件 "hyperv-守護程式" 包含機碼值組、fcopy 和 VSS 守護程式。 在 Debian 7.x 和 8.0-8.2 上，hyperv 的守護程式套件必須來自 [Debian 反向移植](https://wiki.debian.org/Backports)。

3. 在 Windows Server 2012 R2 第2代虛擬機器上，預設會啟用安全開機，除非停用安全開機選項，否則部分 Linux 虛擬機器將不會開機。 您可以在 **Hyper-v 管理員** 的虛擬機器設定的 [**固件**] 區段中停用安全開機，也可以使用 Powershell 來停用它：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
   ```
4. 最新的上游核心功能只能使用 [Debian 反向移植存放庫](https://wiki.debian.org/Backports)中提供的核心提供。

5. 雖然 Debian 7.x 不支援並使用較舊的核心，但 Debian 反向移植 for Debian 7.x 中包含的核心已改進 Hyper-v 功能。

## <a name="see-also"></a>另請參閱

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳作法](Best-Practices-for-running-Linux-on-Hyper-V.md)
