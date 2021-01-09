---
title: Hyper-v 上支援的 SUSE 虛擬機器
description: 列出每個版本中包含的 SUSE/Linux integration services 和功能
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/08/2021
ms.openlocfilehash: 9e75e48e51b7ae77bd084dc092a27184c303a3ae
ms.sourcegitcommit: 209b0995a11c89bb9ece3db0d48a35d7ba5bbd9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2021
ms.locfileid: "98053561"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Hyper-v 上支援的 SUSE 虛擬機器

>適用于： Azure Stack HCI、版本 20H2;Windows Server 2019、Hyper-v Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows 10、Windows 8。1

以下是一種功能發佈對應，表示每個版本中的功能。 每個散發的已知問題和因應措施會列在資料表之後。

適用于 Hyper-v 的內建 SUSE Linux Enterprise Service 驅動程式是由 SUSE 認證。 您可以在此公告中查看範例設定： [SUSE YES 認證公告](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176)。

## <a name="table-legend"></a>資料表圖例

* **內建的內建** 元件包含在此 Linux 發行版本中。Microsoft 提供的 .LIS 下載套件不適用於此散發套件，因此請勿安裝。內建的 .LIS (的核心模組版本號碼如 **lsmod** 所示，例如) 與 Microsoft 提供的 .lis 下載套件上的版本號碼不同。 不相符並不表示內建的 .LIS 已過期。

* &#10004;-可用功能

*  (*空白*) -功能無法使用

SLES12 + 僅限64位。

|**功能**|**Windows Server 作業系統版本**|**SLES 15 SP1-SP2**|**SLES 15**|**SLES 12 SP3-SP5**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|內建|內建|內建|
|**[核心](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 精確時間|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||||
|大型訊框|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和中繼|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 插入|2019、2016、2012 R2|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|TCP 分割和總和檢查碼卸載|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|||||||||
|VHDX 調整大小|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|虛擬光纖通道|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時虛擬機器備份|2019、2016、2012 R2|&#10004; 附注2，3，8|&#10004;附注2，3，8|&#10004; 附注2，3，8|&#10004; 附注2，3，8|&#10004; 附注2，3，8|&#10004; 附注2，3，8|&#10004; 附注2，3，8|
|TRIM 支援|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[記憶體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||||||
|PAE 核心支援|2019、2016、2012 R2|N/A|N/A|N/A|N/A|N/A|&#10004;|&#10004;|
|設定 MMIO 間距|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體-Hot-Add|2019、2016、2012 R2|&#10004; 附注6|&#10004;附注6|&#10004; 附注6|&#10004; 附注6|&#10004; 附注6|&#10004; 附注4，5，6|&#10004; 附注4，5，6|
|動態記憶體-佔用|2019、2016、2012 R2|&#10004; 附注6|&#10004; 附注6|&#10004; 附注6|&#10004; 附注6|&#10004; 附注6|&#10004; 附注4，5，6|&#10004; 附注4，5，6|
|執行時間記憶體大小調整|2019、2016|&#10004; 附注6|&#10004; 附注6|&#10004; 附注6|&#10004; 附注6||||
|**[視訊](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||||
|Hyper-v 特定的影片裝置|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||||||
|索引鍵/值組|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; 附注7|&#10004; 附注7|
|非遮罩式插斷|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到來賓的檔案複製|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|Hyper-v 通訊端|2019、2016|&#10004;|&#10004;|&#10004;|||||
|PCI 傳遞/DDA|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||||||
|使用 UEFI 開機|2019、2016、2012 R2|&#10004; 附注9|&#10004; 附注9|&#10004; 附注9|&#10004; 附注9|&#10004; 附注9|&#10004; 附注9||
|安全開機|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="notes"></a><a name="BKMK_notes"></a>附註

1. 如果 **網路系統管理員** 已針對虛擬機器上指定的 hyper-v 特定網路介面卡進行設定，則靜態 IP 插入可能無法運作。 若要確保靜態 IP 插入的正常運作，請確定網路系統管理員已完全關閉，或已透過其 **>ifcfg-eth0 ethX** 檔關閉特定網路介面卡的功能。

2. 如果在即時虛擬機器備份作業期間有開啟的檔案控制代碼，則在某些角落的情況下，備份的 Vhd 可能必須在還原時 (fsck) 進行檔案系統一致性檢查。

3. 如果虛擬機器具有連結的 iSCSI 裝置或直接連接的存放裝置 (也稱為傳遞磁片) ，即時備份作業可能會以無訊息方式失敗。

4. 如果客體作業系統的記憶體不足，則動態記憶體作業可能會失敗。 以下是一些最佳作法：

   * 啟動記憶體和最小記憶體應等於或大於散發廠商建議的記憶體數量。

   * 通常會耗用系統上的整個可用記憶體的應用程式，只能耗用高達80% 的可用 RAM。

5. 動態記憶體支援僅適用于64位的虛擬機器。

6. 如果您在 Windows Server 2016 或 Windows Server 2012 作業系統上使用動態記憶體，請指定 **啟動記憶體**、 **最小記憶體** 和 **最大記憶體** 參數（以 128 mb (MB) 的倍數表示）。 如果無法這麼做，可能會導致 Hot-Add 失敗，而且您可能不會在客體作業系統中看到任何記憶體增加。

7. 在 Windows Server 2016 或 Windows Server 2012 R2 中，金鑰/值組基礎結構可能無法在沒有 Linux 軟體更新的情況下正常運作。 如果您看到此功能有問題，請洽詢您的散發廠商以取得軟體更新。

8. 如果單一分割區已多次裝載，VSS 備份將會失敗。

9. 在 Windows Server 2012 R2 上，第2代虛擬機器預設會啟用安全開機，除非停用安全開機選項，否則第2代 Linux 虛擬機器將不會開機。 您可以在 [Hyper-V 管理員] 虛擬機器設定的 [韌體] 區段停用安全開機，或是使用 Powershell 停用安全開機：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>另請參閱

* [設定-Get-vmfirmware](/powershell/module/hyper-v/set-vmfirmware)

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳作法](Best-Practices-for-running-Linux-on-Hyper-V.md)
