---
title: 在 HYPER-V 上支援的 Ubuntu 虛擬機器
description: 列出的 Linux integration services 和每個版本中所包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 11/19/2018
ms.openlocfilehash: 662541658fe6e7b99e66fe31344450e0a1cbd201
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447835"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>在 HYPER-V 上支援的 Ubuntu 虛擬機器

>適用於：Windows Server 2019，2016 年之 HYPER-V 伺服器 2019 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

從 Ubuntu 12.04，載入 「 linux-虛擬 」 的套件會安裝適用於核心客體虛擬機器。 此封裝一定會相依於的最新的最小的一般核心映像和虛擬機器所使用的標頭。 雖然它的使用是選擇性的 linux 虛擬核心會載入較少的驅動程式可能更快速開機和有較少的記憶體額外負荷比一般的映像。

若要充分利用 HYPER-V，安裝適當的 linux 工具和 linux 雲端工具封裝，以安裝工具和精靈使用與虛擬機器。 當使用的 linux 虛擬核心，載入 linux 工具-虛擬和 linux 雲端-工具-虛擬。

下列功能發佈對應會指出每個版本的功能。 表格後面列出已知的問題和因應措施的每個散發。

## <a name="table-legend"></a>表格圖例

* **內建**-LIS 是此 Linux 散發套件的一部分。 Microsoft 所提供的 LIS 下載套件不適用於此散發，因此不會安裝。 核心模組的版本號碼的內建的 LIS (如所示**lsmod**，例如) 上的 Microsoft 所提供的 LIS 下載套件的版本號碼不同。 不相符並不表示內建的 LIS 已過期。

* &#10004;-功能

* (*空白*)-功能無法使用

|**功能**|**Windows Server 作業系統版本**|**18.10**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|內建|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 正確時間|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|大型訊框|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和主幹連線|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2019、 2016、 2012 R2，2012年|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|TCP 分割和總和檢查碼卸載|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|VHDX 調整大小|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|虛擬光纖通道|2019、 2016、 2012 R2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2||
|即時虛擬機器備份|2019、 2016、 2012 R2|&#10004;請注意 3、 4、 6|&#10004;請注意 3、 4、 5|&#10004;請注意 3、 4、 5|&#10004;請注意 3、 4、 5||
|修剪支援|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|PAE 核心支援|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|MMIO 間距的組態|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2019、 2016、 2012 R2，2012年|&#10004;請注意 7、 8、 9|&#10004;請注意 7、 8、 9|&#10004;請注意 7、 8、 9|&#10004;請注意 7、 8、 9||
|動態記憶體-佔用|2019、 2016、 2012 R2，2012年|&#10004;請注意 7、 8、 9|&#10004;請注意 7、 8、 9|&#10004;請注意 7、 8、 9|&#10004;請注意 7、 8、 9||
|執行階段記憶體大小調整|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|HYPER-V 特定視訊裝置|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|索引鍵/值組|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;附註 6 10|&#10004;請注意 5、 10|&#10004;請注意 5、 10|&#10004;請注意 5、 10|&#10004;請注意 5、 10|
|非遮罩式插斷|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到客體檔案複製|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|HYPER-V 通訊端|2019, 2016||||||
|PCI 通過/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|使用 UEFI 開機|2019、 2016、 2012 R2|&#10004;請注意 11、 12|&#10004;請注意 11、 12|&#10004;請注意 11、 12|&#10004;請注意 11、 12||
|安全開機|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>附註

1. 如果靜態 IP 資料隱碼攻擊可能無法運作**網路管理員**已針對指定的 Hyper-v 特定網路介面卡，虛擬機器上設定。 若要確保能夠順利運作的靜態 IP 資料隱碼請確定網路管理員完全關閉，或已關閉特定的網路介面卡透過其**ifcfg ethX**檔案。

2. 在使用虛擬光纖通道裝置時，請確定邏輯單元編號 (LUN 0) 0 已填入。 如果尚未擴展 LUN 0，Linux 虛擬機器可能無法以原生方式掛接光纖通道裝置。

3. 如果有開啟檔案控制代碼在上線的虛擬機器備份作業時，則某些角情況下，備份 Vhd 可能必須進行的檔案系統一致性檢查 (`fsck`) 上還原。

4. 如果虛擬機器有附加的 iSCSI 裝置或直接附加儲存體 （也稱為傳遞磁碟），即時的備份作業可能會以無訊息模式失敗。

5. 在長期支援 (LTS) 版本會使用最新的 Linux Integration Services 的最新的虛擬硬體啟用 (HWE) 核心。

   若要安裝 Azure 調整核心 14.04、 16.04 及 18.04 上，執行下列命令為根 （sudo）：

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   12.04 沒有個別的虛擬核心。 若要安裝一般 HWE 核心 12.04 上，執行下列命令為根 （sudo）：

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty
   ```

   在 Ubuntu 12.04 下列 HYPER-V 精靈是在個別安裝的套件：

   * **VSS 快照集精靈**-建立即時 Linux 虛擬機器備份需要此精靈。
   * **KVP 精靈**-此精靈可用於設定及查詢內建與外來的機碼值組。
   * **fcopy 精靈**-此精靈可實作檔案，複製在主機與客體之間的服務。

   若要安裝 KVP 精靈 12.04 上，執行下列命令為根 （sudo）。

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty
   ```

   每當更新核心時，虛擬機器必須將它重新開機。

6. 在 Ubuntu 18.10 使用最新的虛擬核心来擁有最新的 HYPER-V 功能。

   若要安裝的虛擬核心 18.10 上，執行下列命令為根 （sudo）：

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   每當更新核心時，虛擬機器必須將它重新開機。

7. 只有在 64 位元虛擬機器上使用動態記憶體支援。

8. 如果客體作業系統上的記憶體執行太低，動態記憶體的作業可能會失敗。 以下是一些最佳作法：

   * 啟動記憶體和最少的記憶體應該等於或大於發佈廠商建議的記憶體數量。

   * 應用程式，會消耗整個系統上可用的記憶體僅限於耗用最多 80%的可用的 RAM。

9. 如果您使用動態記憶體在 Windows Server 2019、 Windows Server 2016 或 Windows Server 2012/2012 R2 作業系統上，指定**啟動記憶體**，**最小記憶體**，和**最大值記憶體**128 mb 的倍數中的參數。 熱新增失敗，可能會導致失敗，若要這樣做，您可能看不到任何來賓作業系統上增加的記憶體。

10. 在 Windows Server 2019、 Windows Server 2016 或 Windows Server 2012 R2 中，索引鍵/值組基礎結構可能會無法正確運作而不需要 Linux 軟體更新。 請連絡您的發佈廠商，以取得軟體更新，萬一您看到這項功能的問題。

11. 在 Windows Server 2012 R2，第 2 代虛擬機器會有預設和某些 Linux 虛擬機器將無法開機，除非已停用安全開機選項已啟用安全開機。 您可以停用安全開機**韌體**中的虛擬機器的設定 區段**HYPER-V 管理員**也可以使用 Powershell 加以停用：

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. 然後再嘗試複製現有的層代 2 VHD 虛擬機器的 VHD，以建立新的第 2 代虛擬機器，請遵循下列步驟：

    1. 登入現有的第 2 代虛擬機器。

    2. 將目錄變更到開機的 EFI 目錄：

       ```bash
       # cd /boot/efi/EFI
       ```

    3. 將中的 ubuntu 目錄複製到名為開機的新目錄：

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. 將目錄變更為新建立的開機目錄：

       ```bash
       # cd boot
       ```

    5. 重新命名 shimx64.efi 檔案：

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>另請參閱

* [支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [層代 2 中的 Ubuntu 14.04 VM-Ben Armstrong 的虛擬化部落格](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
