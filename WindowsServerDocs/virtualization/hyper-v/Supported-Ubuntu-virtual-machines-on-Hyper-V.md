---
title: Hyper-v 上支援的 Ubuntu 虛擬機器
description: 列出每個版本中包含的 Linux 整合服務和功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 06/13/2019
ms.openlocfilehash: ad0f79767310595244d0d57876c20b9548a81a96
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69584814"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Hyper-v 上支援的 Ubuntu 虛擬機器

>適用於：Windows Server 2019、2016、Hyper-v Server 2019、2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

從 Ubuntu 12.04 開始, 載入 "linux-virtual" 套件會安裝適合做為來賓虛擬機器使用的核心。 此套件一律取決於用於虛擬機器的最新最小一般核心映射和標頭。 雖然其使用是選擇性的, 但 linux 虛擬核心會載入較少的驅動程式, 而且開機速度會更快, 而且記憶體額外負荷會比一般映射少。

若要充分利用 Hyper-v, 請安裝適當的 linux 工具和 linux 雲端工具套件, 以安裝與虛擬機器搭配使用的工具和守護程式。 使用 linux 虛擬核心時, 請載入 linux 工具-虛擬和 linux-雲端工具-虛擬。

下列功能發佈對應會指出每個版本的功能。 每個散發套件的已知問題和因應措施會列在表格之後。

## <a name="table-legend"></a>資料表圖例

* **內建**的 .lis 版本包含在此 Linux 散發套件中。 Microsoft 提供的 .LIS 版下載套件不適用於此發佈, 因此請勿安裝。 內建的 .LIS 的核心模組版本號碼 (如**lsmod**所示) 與 Microsoft 提供的 .lis 下載套件上的版本號碼不同。 不相符並不表示內建的 .LIS 已過期。

* &#10004;-可用的功能

* (*空白*)-無法使用功能

|**功能**|**Windows Server 作業系統版本**|**18.10/19.04**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**Availability**||內建|內建|內建|內建|內建|
|**[雙核處理器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 精確時間|2019、2016|&#10004;|&#10004;|&#10004;|||
|**[連](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|大型訊框|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和中繼|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 插入|2019、2016、2012 R2、2012|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|&#10004;附注1|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|TCP 分割和總和檢查碼卸載|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;|||
|**[容量](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|VHDX 調整大小|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|虛擬光纖通道|2019、2016、2012 R2|&#10004;附注2|&#10004;附注2|&#10004;附注2|&#10004;附注2||
|即時虛擬機器備份|2019、2016、2012 R2|&#10004;注3、4、6|&#10004;注3、4、5|&#10004;注3、4、5|&#10004;注3、4、5||
|修剪支援|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[快閃記憶體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|PAE 核心支援|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|設定 MMIO 間隙|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體-熱新增|2019、2016、2012 R2、2012|&#10004;附注7、8、9|&#10004;附注7、8、9|&#10004;附注7、8、9|&#10004;附注7、8、9||
|動態記憶體-佔用|2019、2016、2012 R2、2012|&#10004;附注7、8、9|&#10004;附注7、8、9|&#10004;附注7、8、9|&#10004;附注7、8、9||
|執行時間記憶體大小調整|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|Hyper-v 特定的影片裝置|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|索引鍵/值組|2019、2016、2012 R2、2012、2008 R2|&#10004;注6、10|&#10004;注5、10|&#10004;注5、10|&#10004;注5、10|&#10004;注5、10|
|非遮罩式插斷|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到來賓的檔案複製|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Hyper-v 通訊端|2019、2016||||||
|PCI 通過/DDA|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[第2代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|使用 UEFI 開機|2019、2016、2012 R2|&#10004;注 11, 12|&#10004;注 11, 12|&#10004;注 11, 12|&#10004;注 11, 12||
|安全開機|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>注意

1. 如果已針對虛擬機器上指定的 Hyper-v 特定網路介面卡設定**網路系統管理員**, 則靜態 IP 插入可能無法正常執行。 為確保能順利運作靜態 IP 插入, 請確認網路系統管理員已完全關閉, 或已透過其**Ifcfg-eth0 ethX**檔關閉特定網路介面卡。

2. 使用虛擬光纖通道裝置時, 請確定已填入邏輯單元編號 0 (LUN 0)。 如果尚未填入 LUN 0, Linux 虛擬機器可能無法以原生方式掛接光纖通道裝置。

3. 如果在即時虛擬機器備份作業期間有開啟的檔案控制代碼, 則在某些角落的情況下, 備份的 vhd 可能必須在還原時執行檔案系統一致性`fsck`檢查 ()。

4. 如果虛擬機器有連結的 iSCSI 裝置或直接連接的存放裝置 (也稱為傳遞磁片), 即時備份作業可能會以無訊息模式失敗。

5. 長期支援 (LTS) 版本使用最新的虛擬硬體啟用 (HWE) 核心, 以取得最新的 Linux Integration Services。

   若要在14.04、16.04 和18.04 上安裝 Azure 微調的核心, 請以 root (或 sudo) 的身分執行下列命令:

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   12.04 沒有個別的虛擬核心。 若要在12.04 上安裝泛型 HWE 核心, 請以 root (或 sudo) 身分執行下列命令:

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty
   ```

   在 Ubuntu 12.04 上, 下列 Hyper-v 守護程式位於個別安裝的套件中:

   * **VSS 快照**集背景程式-建立即時 Linux 虛擬機器備份時, 必須要有此 daemon。
   * **KVP daemon** -此守護程式可讓您設定及查詢內建和外部金鑰值組。
   * **fcopy daemon** -此守護程式會在主機和來賓之間執行檔案複製服務。

   若要在12.04 上安裝 KVP daemon, 請以 root (或 sudo) 身分執行下列命令。

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty
   ```

   當核心更新時, 必須重新開機虛擬機器才能使用它。

6. 在 Ubuntu 18.10 或19.04 上, 使用最新的虛擬核心, 以取得最新的 Hyper-v 功能。

   若要在18.10 或19.04 上安裝虛擬核心, 請以 root (或 sudo) 的身分執行下列命令:

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   當核心更新時, 必須重新開機虛擬機器才能使用它。

7. 動態記憶體支援僅適用于64位的虛擬機器。

8. 如果客體作業系統的記憶體過低, 動態記憶體作業會失敗。 以下是一些最佳作法:

   * [啟動記憶體] 和 [最小記憶體] 應該等於或大於散發廠商建議的記憶體數量。

   * 通常會耗用系統上整個可用記憶體的應用程式, 只能耗用最多 80% 的可用 RAM。

9. 如果您使用 Windows Server 2019、Windows Server 2016 或 Windows Server 2012/2012 R2 作業系統上的動態記憶體, 請指定**啟動記憶體**、**最小記憶體**和**最大記憶體**參數 (以 128 mb 的倍數表示)。 如果無法這樣做, 可能會導致熱新增失敗, 而且您可能不會在客體作業系統上看到任何記憶體增加。

10. 在 Windows Server 2019、Windows Server 2016 或 Windows Server 2012 R2 中, 如果沒有 Linux 軟體更新, 金鑰/值組基礎結構可能無法正常運作。 請洽詢您的散發廠商, 以取得此功能發生問題時的軟體更新。

11. 在 Windows Server 2012 R2 上, 第2代虛擬機器預設會啟用安全開機, 而且除非停用安全開機選項, 否則部分 Linux 虛擬機器將無法開機。 您可以在**Hyper-v 管理員**的虛擬機器設定的 [**固件**] 區段中停用安全開機, 也可以使用 Powershell 將它停用:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. 在嘗試複製現有第2代 VHD 虛擬機器的 VHD 以建立新的第2代虛擬機器之前, 請遵循下列步驟:

    1. 登入現有的第2代虛擬機器。

    2. 將目錄變更為開機 EFI 目錄:

       ```bash
       # cd /boot/efi/EFI
       ```

    3. 將 ubuntu 目錄複寫到名為 boot 的新目錄:

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. 將目錄變更為新建立的開機目錄:

       ```bash
       # cd boot
       ```

    5. 重新命名 shimx64 efi 檔案:

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>另請參閱

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [設定-Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [第2代 VM 中的 Ubuntu 14.04-Ben Armstrong 的虛擬化 Blog](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
