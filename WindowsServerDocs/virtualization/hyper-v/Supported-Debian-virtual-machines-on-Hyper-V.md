---
title: 在 HYPER-V 上支援的 Debian 虛擬機器
description: 列出的 Linux integration services 和每個版本中所包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 6ec089f501a0999a4460501dbc4d03428d36af40
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863829"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>在 HYPER-V 上支援的 Debian 虛擬機器

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

下列功能發佈對應指示會出現在每個版本的功能。 表格後面列出已知的問題和因應措施的每個散發。

## <a name="table-legend"></a>表格圖例

* **內建**-LIS 是此 Linux 散發套件的一部分。 Microsoft 所提供的 LIS 下載套件不適用於此發佈，所以不會安裝它。 核心模組的版本號碼的內建的 LIS (如所示**lsmod**，例如) 上的 Microsoft 所提供的 LIS 下載套件的版本號碼不同。 不相符並不代表內建的 LIS 已過期。

* &#10004;-功能

* (*空白*)-功能無法使用

|**功能**|**Windows Server 作業系統版本**|**9.0-9.6 (stretch)**|**8.0-8.11 (jessie)**|**7.0 7.11 (wheezy)**|
|-|-|-|-|-|
|**可用性**||內建|內建|內建 （註 6）|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 正確時間|2019, 2016|&#10004;請注意 8||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|
|大型訊框|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|VLAN 標記和主幹連線|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2019、 2016、 2012 R2，2012年|||
|vRSS|2019、 2016、 2012 R2|&#10004;請注意 8|||
|TCP 分割和總和檢查碼卸載|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;請注意 8|||
|SR-IOV|2019, 2016|&#10004;請注意 8||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**|
|VHDX 調整大小|2019、 2016、 2012 R2|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|
|虛擬光纖通道|2019、 2016、 2012 R2|||
|即時虛擬機器備份|2019、 2016、 2012 R2|&#10004;附註 4 5|&#10004;附註 4 5|&#10004;附註 4|
|修剪支援|2019、 2016、 2012 R2|&#10004;請注意 8|||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;請注意 8||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|PAE 核心支援|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|MMIO 間距的組態|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2019、 2016、 2012 R2，2012年|&#10004;請注意 8|||
|動態記憶體-佔用|2019、 2016、 2012 R2，2012年|&#10004;請注意 8|||
|執行階段記憶體大小調整|2019, 2016|&#10004;請注意 8|||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|
|Hyper-v 特定視訊裝置|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**|
|索引鍵 / 值組|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;附註 4|&#10004;附註 4||
|非遮罩式插斷|2019、 2016、 2012 R2|&#10004;|&#10004;|
|從主機到客體檔案複製|2019、 2016、 2012 R2|&#10004;附註 4|&#10004;附註 4||
|lsvmbus 命令|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|||
|HYPER-V 通訊端|2019, 2016|&#10004;請注意 8|||
|PCI 通過/DDA|2019, 2016|&#10004;請注意 8|||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|使用 UEFI 開機|2019、 2016、 2012 R2|&#10004;請注意 7|&#10004;請注意 7||
|安全開機|2019, 2016|||

## <a name="BKMK_notes"></a>附註

1. 不支援建立的 Vhd 上的檔案系統大於 2 TB。

2. 在 Windows Server 2008 R2 SCSI 磁碟會建立 8 個不同的項目中/sd *。

3. Windows Server 2012 R2 的 8 個核心或多個 VM 會有所有路由傳送至單一 vCPU 的插斷。

4. 開始使用 Debian 8.3 手動安裝 Debian 套件"hyperv-精靈 」 包含索引鍵 / 值組、 fcopy 和 VSS 精靈。 Debian 7.x 和 8.0 8.2 hyperv 精靈封裝必須來自[Debian 反向移植](https://wiki.debian.org/Backports)。

5. 即時虛擬機器備份無法搭配 ext2 檔案系統。 Debian 的安裝程式所建立的預設版面配置包含 ext2 檔案系統，您必須自訂版面配置，以建立此檔案系統類型。

6. Debian 7.x 是不受支援，並使用較舊的核心，而中包含的核心[Debian 反向移植](https://wiki.debian.org/Backports)Debian 7.x 已改善對 HYPER-V 功能。

7. 在 Windows Server 2012 R2 第 2 代虛擬機器已預設啟用安全開機，某些 Linux 虛擬機器將無法開機，除非已停用安全開機選項。 您可以停用安全開機**韌體**中的虛擬機器的設定 區段**HYPER-V 管理員**也可以使用 Powershell 加以停用：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. 最新的上游核心功能是只使用包含的核心[Debian 反向移植](https://wiki.debian.org/Backports)。

另請參閱

* [支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
