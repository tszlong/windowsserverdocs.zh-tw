---
title: Hyper-v 上支援的 SUSE 虛擬機器
description: 列出每個版本中包含的 Linux 整合服務和功能
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 04/07/2020
ms.openlocfilehash: 96dadc56c17dcdbf391c480029e3124bf70dbec7
ms.sourcegitcommit: 7b1ebc4934998af2472962ca8cce1c872f39946f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994515"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Hyper-v 上支援的 SUSE 虛擬機器

>適用于： Windows Server 2019、Hyper-v Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows 10、Windows 8。1

以下是功能發佈對應，表示每個版本的功能。 每個散發套件的已知問題和因應措施會列在表格之後。

Hyper-v 的內建 SUSE Linux Enterprise Service 驅動程式已通過 SUSE 認證。 您可以在此公告中查看範例設定： [SUSE YES 認證公告](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176)。

## <a name="table-legend"></a>資料表圖例

* **內建**的 .lis 版本包含在此 Linux 散發套件中。Microsoft 提供的 .LIS 版下載套件無法用於此發佈，因此請勿安裝。內建的 .LIS 的核心模組版本號碼（如**lsmod**所示）與 Microsoft 提供的 .lis 下載套件上的版本號碼不同。 不相符並不表示內建的 .LIS 已過期。

* &#10004;-可用的功能

* （*空白*）-無法使用功能

SLES12 + 僅限64位。

|**特徵**|**Windows Server 作業系統版本**|**SLES 15 SP1**|**SLES 15**|**SLES 12 SP3-SP5**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|內建|內建|內建|
|**[雙核處理器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 精確時間|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[連](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||||
|Jumbo 框架|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和中繼|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 插入|2019、2016、2012 R2|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|TCP 分割和總和檢查碼卸載|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[容量](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|||||||||
|VHDX 調整大小|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|虛擬光纖通道|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時虛擬機器備份|2019、2016、2012 R2|&#10004;附注2、3、8|&#10004;附注2、3、8|&#10004;附注2、3、8|&#10004;附注2、3、8|&#10004;附注2、3、8|&#10004;附注2、3、8|&#10004;附注2、3、8|
|修剪支援|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[快閃記憶體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||||||
|PAE 核心支援|2019、2016、2012 R2|N/A|N/A|N/A|N/A|N/A|&#10004;|&#10004;|
|設定 MMIO 間隙|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體-熱新增|2019、2016、2012 R2|&#10004;附注6|&#10004;附注6|&#10004;附注6|&#10004;附注6|&#10004;附注6|&#10004;注4、5、6|&#10004;注4、5、6|
|動態記憶體-佔用|2019、2016、2012 R2|&#10004;附注6|&#10004;附注6|&#10004;附注6|&#10004;附注6|&#10004;附注6|&#10004;注4、5、6|&#10004;注4、5、6|
|執行時間記憶體大小調整|2019、2016|&#10004;附注6|&#10004;附注6|&#10004;附注6|&#10004;附注6||||
|**[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||||
|Hyper-v 特定的影片裝置|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||||||
|索引鍵/值組|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;附注7|&#10004;附注7|
|非遮罩式插斷|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到來賓的檔案複製|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|Hyper-v 通訊端|2019、2016|&#10004;|&#10004;|&#10004;|||||
|PCI 通過/DDA|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[第2代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||||||
|使用 UEFI 開機|2019、2016、2012 R2|&#10004;附注9|&#10004;附注9|&#10004;附注9|&#10004;附注9|&#10004;附注9|&#10004;附注9||
|安全開機|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="notes"></a><a name="BKMK_notes"></a>紀錄

1. 如果已針對虛擬機器上指定的 Hyper-v 特定網路介面卡設定**網路系統管理員**，則靜態 IP 插入可能無法正常執行。 為確保能順利運作靜態 IP 插入，請確認網路系統管理員已完全關閉，或已透過其**Ifcfg-eth0 ethX**檔關閉特定網路介面卡。

2. 如果在即時虛擬機器備份作業期間有開啟的檔案控制代碼，則在某些角落的情況下，備份的 Vhd 可能必須在還原時執行檔案系統一致性檢查（fsck）。

3. 如果虛擬機器有連結的 iSCSI 裝置或直接連接的存放裝置（也稱為傳遞磁片），即時備份作業可能會以無訊息模式失敗。

4. 如果客體作業系統的記憶體不足，動態記憶體作業可能會失敗。 以下是一些最佳作法：

   * [啟動記憶體] 和 [最小記憶體] 應該等於或大於散發廠商建議的記憶體數量。

   * 通常會耗用系統上整個可用記憶體的應用程式，只能耗用最多80% 的可用 RAM。

5. 動態記憶體支援僅適用于64位的虛擬機器。

6. 如果您使用 Windows Server 2016 或 Windows Server 2012 作業系統上的動態記憶體，請將 [**啟動記憶體**]、[**最小記憶體**] 和 [**最大記憶體**] 參數指定為 128 mb 的倍數。 如果無法這樣做，可能會導致熱新增失敗，而且您可能不會在客體作業系統中看到任何記憶體增加。

7. 在 Windows Server 2016 或 Windows Server 2012 R2 中，如果沒有 Linux 軟體更新，金鑰/值組基礎結構可能無法正常運作。 請洽詢您的散發廠商，以取得此功能發生問題時的軟體更新。

8. 如果單一分割區已掛接多次，VSS 備份將會失敗。

9. 在 Windows Server 2012 R2 上，第2代虛擬機器預設會啟用安全開機，第2代 Linux 虛擬機器將不會開機，除非已停用安全開機選項。 您可以在 [Hyper-V 管理員] 虛擬機器設定的 [韌體] 區段停用安全開機，或是使用 Powershell 停用安全開機：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>另請參閱

* [設定-Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
