---
title: Hyper-v 上支援的 Ubuntu 虛擬機器
description: 列出每個版本中所包含的 Linux integration services 和功能
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
ms.author: benarm
author: BenjaminArmstrong
ms.date: 08/29/2020
ms.openlocfilehash: cc59a9c45a1dee797196c8a12550945d3d834cd7
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746573"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Hyper-v 上支援的 Ubuntu 虛擬機器

>適用于： Windows Server 2019、Hyper-v Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows 10、Windows 8。1

下列功能發佈對應會指出每個版本中的功能。 每個散發的已知問題和因應措施會列在資料表之後。

## <a name="table-legend"></a>資料表圖例

* **內建的內建** 元件包含在此 Linux 發行版本中。 Microsoft 提供的 .LIS 下載套件不適用於此發佈，因此請勿安裝。 內建的 .LIS (的核心模組版本號碼如 **lsmod**所示，例如) 與 Microsoft 提供的 .lis 下載套件上的版本號碼不同。 不相符並不表示內建的 .LIS 已過期。

* &#10004;-可用功能

*  (*空白*) -功能無法使用

|**功能**|**Windows Server 作業系統版本**|**20.04 LTS**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|
|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|
|**[核心](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 精確時間|2019、2016|&#10004;|&#10004;|&#10004;||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||
|大型訊框|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和中繼|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 插入|2019、2016、2012 R2|&#10004; 附注1|&#10004; 附注1|&#10004; 附注1|&#10004; 附注1|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|TCP 分割和總和檢查碼卸載|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|||||
|VHDX 調整大小|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|虛擬光纖通道|2019、2016、2012 R2|&#10004; 附注2|&#10004; 附注2|&#10004; 附注2|&#10004; 附注2|
|即時虛擬機器備份|2019、2016、2012 R2|&#10004; 附注3、4、5|&#10004; 附注3、4、5|&#10004; 附注3、4、5|&#10004; 附注3、4、5|
|TRIM 支援|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|**[記憶](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||
|PAE 核心支援|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|設定 MMIO 間距|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體-熱新增|2019、2016、2012 R2|&#10004; 附注6、7、8|&#10004; 附注6、7、8|&#10004; 附注6、7、8|&#10004; 附注6、7、8|
|動態記憶體-佔用|2019、2016、2012 R2|&#10004; 附注6、7、8|&#10004; 附注6、7、8|&#10004; 附注6、7、8|&#10004; 附注6、7、8|
|執行時間記憶體大小調整|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|
|**[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||
|Hyper-v 特定影片裝置|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||
|索引鍵/值組|2019、2016、2012 R2|&#10004; 附注5、9|&#10004; 附注5、9|&#10004; 附注5、9|&#10004; 附注5、9|
|非遮罩式插斷|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到來賓的檔案複製|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|lsvmbus 命令|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Hyper-v 通訊端|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|
|PCI 傳遞/DDA|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||
|使用 UEFI 開機|2019、2016、2012 R2|&#10004; 附注10，11|&#10004; 附注10，11|&#10004; 附注10，11|&#10004; 附注10，11|
|安全開機|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="notes"></a>備註

1. 如果 **網路系統管理員** 已針對虛擬機器上指定的 hyper-v 特定網路介面卡進行設定，則靜態 IP 插入可能無法運作。 若要確保靜態 IP 插入的正常運作，請確定網路系統管理員已完全關閉，或已透過其 **>ifcfg-eth0 ethX** 檔關閉特定網路介面卡的功能。

2. 使用虛擬光纖通道裝置時，請確定已填入邏輯單元編號 0 (LUN 0) 。 如果 LUN 0 尚未填入，Linux 虛擬機器可能無法以原生方式掛接光纖通道裝置。

3. 如果在即時虛擬機器備份作業期間有開啟的檔案控制代碼，則在某些角落的情況下，備份的 Vhd 可能必須在還原時執行檔案系統一致性檢查 (`fsck`) 。

4. 如果虛擬機器具有連結的 iSCSI 裝置或直接連接的存放裝置 (也稱為傳遞磁片) ，即時備份作業可能會以無訊息方式失敗。

5. 長期支援 (LTS) 版本使用最新的虛擬硬體啟用 (HWE) 核心的最新版本，以取得最新的 Linux Integration Services。

   若要在16.04、18.04 和20.04 上安裝 Azure 調整的核心，請以 root (或 sudo) 執行下列命令：

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```
6. 動態記憶體支援僅適用于64位的虛擬機器。

7. 如果客體作業系統執行的記憶體太低，動態記憶體作業可能會失敗。 以下是一些最佳作法：

   * 啟動記憶體和最小記憶體應等於或大於散發廠商建議的記憶體數量。

   * 通常會耗用系統上的整個可用記憶體的應用程式，只能耗用高達80% 的可用 RAM。

8. 如果您在 Windows Server 2019、Windows Server 2016 或 Windows Server 2012/2012 R2 作業系統上使用動態記憶體，請指定 **啟動記憶體**、 **最小記憶體**和 **最大記憶體** 參數（以 128 mb (MB) 的倍數表示）。 如果無法這麼做，可能會導致熱新增失敗，而且您可能不會在客體作業系統上看到任何記憶體增加。

9. 在 Windows Server 2019、Windows Server 2016 或 Windows Server 2012 R2 中，金鑰/值組基礎結構可能無法在沒有 Linux 軟體更新的情況下正常運作。 如果您看到此功能有問題，請洽詢您的散發廠商以取得軟體更新。

10. 在 Windows Server 2012 R2 上，第2代虛擬機器預設會啟用安全開機，除非停用安全開機選項，否則部分 Linux 虛擬機器將不會開機。 您可以在**Hyper-v 管理員**的虛擬機器設定的 [**固件**] 區段中停用安全開機，也可以使用 Powershell 來停用它：

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

11. 在嘗試複製現有第2代 VHD 虛擬機器的 VHD 以建立新的第2代虛擬機器之前，請遵循下列步驟：

    1. 登入現有的第2代虛擬機器。

    2. 將目錄變更為開機 EFI 目錄：

       ```bash
       # cd /boot/efi/EFI
       ```

    3. 將 ubuntu 目錄複寫到名為 boot 的新目錄中：

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. 將目錄變更為新建立的開機目錄：

       ```bash
       # cd boot
       ```

    5. 重新命名 shimx64 efi 檔案：

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>另請參閱

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳作法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [設定-Get-vmfirmware](/powershell/module/hyper-v/set-vmfirmware?view=win10-ps)

* [第2代 VM 中的 Ubuntu 14.04-Ben Armstrong 的虛擬化 Blog](/archive/blogs/virtual_pc_guy/ubuntu-14-04-in-a-generation-2-vm)
