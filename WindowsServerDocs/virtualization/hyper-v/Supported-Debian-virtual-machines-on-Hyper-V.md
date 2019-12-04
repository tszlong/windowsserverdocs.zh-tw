---
title: Hyper-v 上支援的 Debian 虛擬機器
description: 列出每個版本中包含的 Linux 整合服務和功能
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 60f2f3a6ed885f2da80b9beac51eeb703789ec91
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791760"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Hyper-v 上支援的 Debian 虛擬機器

>適用于： Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

下列功能發佈對應會指出每個版本中出現的功能。 每個散發套件的已知問題和因應措施會列在表格之後。

## <a name="table-legend"></a>資料表圖例

* **內建**的 .lis 版本包含在此 Linux 散發套件中。 Microsoft 提供的 .LIS 版下載套件不適用於此發佈，因此不會進行安裝。 內建的 .LIS 的核心模組版本號碼（如**lsmod**所示）與 Microsoft 提供的 .lis 下載套件上的版本號碼不同。 不相符並不表示內建的 .LIS 已過期。

* &#10004;-可用的功能

* （*空白*）-無法使用功能

| **特徵**                                                                                                                                  | **Windows Server 作業系統版本** | **10（buster）** | **9.0-9.6 （延展）** | **8.0-8.11 （jessie）** | **7.0-7.11 （wheezy）** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **可用性**                                                                                                                             |                                             | 內建              | 內建              | 內建              | 內建（注6）     |
| **[雙核處理器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019、2016、2012 R2、2012、2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Windows Server 2016 精確時間                                                                                                            | 2019、2016                                  | &#10004;附注8       | &#10004;附注8       |                       |                       |
| **[連](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| 大型訊框                                                                                                                                 | 2019、2016、2012 R2、2012、2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| VLAN 標記和中繼                                                                                                                    | 2019、2016、2012 R2、2012、2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 即時移轉                                                                                                                               | 2019、2016、2012 R2、2012、2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 靜態 IP 插入                                                                                                                          | 2019、2016、2012 R2、2012                   |                       |                       |                       |                       |
| vRSS                                                                                                                                         | 2019、2016、2012 R2                         | &#10004;附注8       | &#10004;附注8       |                       |                       |
| TCP 分割和總和檢查碼卸載                                                                                                       | 2019、2016、2012 R2、2012、2008 R2          | &#10004;附注8       | &#10004;附注8       |                       |                       |
| SR-IOV                                                                                                                                       | 2019、2016                                  | &#10004;附注8       | &#10004;附注8       |                       |                       |
| **[容量](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| VHDX 調整大小                                                                                                                                  | 2019、2016、2012 R2                         | &#10004;附注1       | &#10004;附注1       | &#10004;附注1       | &#10004;附注1       |
| 虛擬光纖通道                                                                                                                        | 2019、2016、2012 R2                         |                       |                       |                       |                       |
| 即時虛擬機器備份                                                                                                                  | 2019、2016、2012 R2                         | &#10004;注4，5     | &#10004;注4，5     | &#10004;注4，5     | &#10004;注4       |
| 修剪支援                                                                                                                                 | 2019、2016、2012 R2                         | &#10004;附注8       | &#10004;附注8       |                       |                       |
| SCSI WWN                                                                                                                                     | 2019、2016、2012 R2                         | &#10004;附注8       | &#10004;附注8       |                       |                       |
| **[快閃記憶體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| PAE 核心支援                                                                                                                           | 2019、2016、2012 R2、2012、2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 設定 MMIO 間隙                                                                                                                    | 2019、2016、2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| 動態記憶體-熱新增                                                                                                                     | 2019、2016、2012 R2、2012                   | &#10004;附注8       | &#10004;附注8       |                       |                       |
| 動態記憶體-佔用                                                                                                                  | 2019、2016、2012 R2、2012                   | &#10004;附注8       | &#10004;附注8       |                       |                       |
| 執行時間記憶體大小調整                                                                                                                        | 2019、2016                                  | &#10004;附注8       | &#10004;附注8       |                       |                       |
| **[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Hyper-v 特定的影片裝置                                                                                                                | 2019、2016、2012 R2、2012、2008 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| 索引鍵/值組                                                                                                                               | 2019、2016、2012 R2、2012、2008 R2          | &#10004;注4       | &#10004;注4       | &#10004;注4       |                       |
| 非遮罩式插斷                                                                                                                       | 2019、2016、2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| 從主機到來賓的檔案複製                                                                                                                 | 2019、2016、2012 R2                         | &#10004;注4       | &#10004;注4       | &#10004;注4       |                       |
| lsvmbus 命令                                                                                                                              | 2019、2016、2012 R2、2012、2008 R2          |                       |                       |                       |                       |
| Hyper-v 通訊端                                                                                                                              | 2019、2016                                  | &#10004;附注8       | &#10004;附注8       |                       |                       |
| PCI 通過/DDA                                                                                                                          | 2019、2016                                  | &#10004;附注8       | &#10004;附注8       |                       |                       |
| **[第2代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| 使用 UEFI 開機                                                                                                                              | 2019、2016、2012 R2                         | &#10004;附注7       | &#10004;附注7       | &#10004;附注7       |                       |
| 安全開機                                                                                                                                  | 2019、2016                                  | &#10004;              |                       |                       |                       |


## <a name="BKMK_notes"></a>紀錄

1. 不支援在大於2TB 的 Vhd 上建立檔案系統。

2. 在 Windows Server 2008 R2 SCSI 磁片上，于/dev/sd * 中建立8個不同的專案。

3. Windows Server 2012 R2 具有8個核心或更多的 VM 將會有路由傳送至單一 vCPU 的所有中斷。

4. 從 Debian 8.3 開始，手動安裝的 Debian 套件 "hyperv-守護程式" 包含機碼值組、fcopy 和 VSS 守護程式。 在 Debian 7.x 和 8.0-8.2 上，hyperv-守護程式套件必須來自[Debian 反向移植](https://wiki.debian.org/Backports)。

5. 即時虛擬機器備份不會與 ext2 檔案系統搭配使用。 Debian 安裝程式所建立的預設版面配置包含 ext2 檔案系統，您必須自訂配置，以不建立此檔案系統類型。

6. 雖然 Debian 7.x 不支援，而且使用較舊的核心，但[Debian 反向移植](https://wiki.debian.org/Backports)for Debian 7.x 所包含的核心已改善 hyper-v 功能。

7. 在 Windows Server 2012 R2 第2代虛擬機器上，預設會啟用安全開機，而且除非停用安全開機選項，否則部分 Linux 虛擬機器將無法開機。 您可以在**Hyper-v 管理員**的虛擬機器設定的 [**固件**] 區段中停用安全開機，也可以使用 Powershell 將它停用：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. 最新的上游核心功能僅適用于使用核心內含的[Debian 反向移植](https://wiki.debian.org/Backports)。

請參閱

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
