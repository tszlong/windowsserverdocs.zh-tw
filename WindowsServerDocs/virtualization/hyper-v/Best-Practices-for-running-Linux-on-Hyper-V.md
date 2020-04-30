---
title: 在 Hyper-v 上執行 Linux 的最佳做法
description: 提供在虛擬機器上執行 Linux 的建議
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 04/15/2020
ms.openlocfilehash: d8861369abe24ea0d34dce209a5d98e854c4c95d
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "82072234"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>在 Hyper-v 上執行 Linux 的最佳做法

>適用于： Windows Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

本主題包含在 Hyper-v 上執行 Linux 虛擬機器的建議清單。

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>在動態 VHDX 檔案上調整 Linux 檔案系統

某些 Linux 檔案系統可能會耗用大量的實際磁碟空間，即使檔案系統大多是空的。 若要減少動態 VHDX 檔案的實際磁碟空間使用量，請考慮下列建議：

* 建立 VHDX 時，請在 PowerShell 中使用 1MB BlockSizeBytes （從預設的32MB），例如：

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* Ext4 格式偏好 ext3，因為 ext4 在與動態 VHDX 檔案搭配使用時，比 ext3 更具空間效率。

* 建立 filesystem 時，請將群組的數目指定為4096，例如：

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>第2代虛擬機器的 Grub 功能表超時

由於從第2代虛擬機器的模擬中移除舊版硬體，因此 grub 功能表倒數計時計時器會太快關閉以顯示 grub 功能表，立即載入預設專案。 除非已修正 grub，才能使用 EFI 支援的計時器、修改 **/boot/grub/grub.conf**、/**等/預設/grub**，或相當於具有 "timeout = 100000"，而不是預設值 "timeout = 5"。

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>第2代虛擬機器上的 PxE 開機

因為第2代虛擬機器中的 PIT 計時器不存在，所以與 PxE TFTP 伺服器的網路連線可能會提前終止，並防止開機載入器讀取 Grub 設定並從伺服器載入核心。

在 RHEL 6.x 上，您可以使用舊版 grub v 0.97 EFI 開機載入器，而不是 grub2，如下所述：[https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

在 RHEL 6.x 以外的 Linux 散發套件上，可以遵循類似的步驟，將 grub v 0.97 設定為從 PxE 伺服器載入 Linux 核心。

此外，在 RHEL/CentOS 6.6 鍵盤和滑鼠輸入中，不會使用可防止在功能表中指定安裝選項的預先安裝核心。 序列主控台必須設定為允許選擇安裝選項。

* 在 PxE 伺服器的**efidefault**檔案中，新增下列核心參數 **"console = ttyS1"**

* 在 Hyper-v 的 VM 上，使用下列 PowerShell Cmdlet 設定 COM 埠：

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

將 kickstart 檔案指定給預先安裝核心也可以避免在安裝期間需要鍵盤和滑鼠輸入。

## <a name="use-static-mac-addresses-with-failover-clustering"></a>搭配容錯移轉叢集使用靜態 MAC 位址

將使用容錯移轉叢集部署的 Linux 虛擬機器，應該使用每個虛擬網路介面卡的靜態媒體存取控制（MAC）位址進行設定。 在某些 Linux 版本中，網路設定可能會在容錯移轉後遺失，因為已將新的 MAC 位址指派給虛擬網路介面卡。 為避免遺失網路設定，請確定每個虛擬網路介面卡都有靜態 MAC 位址。 您可以在 [Hyper-v 管理員] 或容錯移轉叢集管理員中編輯虛擬機器的設定，以設定 MAC 位址。

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>使用 Hyper-v 專用的網路介面卡，而不是傳統網路介面卡

設定和使用虛擬 Ethernet 介面卡，這是一種具有增強效能的 Hyper-v 專用網路介面卡。 如果舊版和 Hyper-v 特有的網路介面卡都連接到虛擬機器， **ifconfig**輸出中的網路名稱可能會顯示隨機值，例如 **_tmp12000801310**。 若要避免此問題，請在 Linux 虛擬機器中使用 Hyper-v 特定的網路介面卡時，移除所有傳統網路介面卡。

## <a name="use-io-scheduler-noopnone-for-better-disk-io-performance"></a>使用 i/o 排程器 noop/none 以獲得更好的磁片 i/o 效能

Linux 核心提供兩組磁片 i/o 排程器來重新排序要求。  其中一個集合適用于較舊的 ' blk ' 子系統，而另一個集合則適用于較新的 ' blk-mq ' 子系統。 不論是哪一種情況，使用今天的固態硬碟時，建議使用排程器，將排程決策傳遞給基礎的 Hyper-v 虛擬程式。 針對使用 ' blk ' 子系統的 Linux 核心，這是「noop」排程器。 針對使用 ' blk-mq ' 子系統的 Linux 核心，這是「無」排程器。

針對特定磁片，可在此檔案系統位置看到可用的排程器：/sys/class/block/`<diskname>`/queue/scheduler，並以方括弧括住目前選取的排程器。 您可以藉由寫入此檔案系統位置來變更排程器。 必須將變更新增至初始化腳本，才能在重新開機期間保存。 如需詳細資訊，請參閱您的 Linux 散發版本檔。

## <a name="numa"></a>NUMA

早於 2.6.37 的 Linux 核心版本不支援具較大 VM 大小之 Hyper-V 上的 NUMA。 這個問題主要會影響使用上游 Red Hat 2.6.32 核心的較舊發行版本，而且已在 Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504) 中加以修正。 執行的自訂核心是 2.6.37 以前版本的系統，或 2.6.32-504 以前的 RHEL 型核心必須在 grub.conf 的核心命令列上設定開機參數 `numa=off`。 如需詳細資訊，請參閱 [Red Hat KB 436883](https://access.redhat.com/solutions/436883) \(英文\)。

## <a name="reserve-more-memory-for-kdump"></a>為 kdump 保留更多記憶體

萬一傾印捕獲核心在開機時出現死機，請為核心保留更多記憶體。 例如，在 Ubuntu grub 設定檔中，將參數**crashkernel = 384M-： 128M**變更為**crashkernel = 384M-： 256M** 。

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>壓縮 VHDX 或擴充 VHD 和 VHDX 檔案可能會導致錯誤的 GPT 磁碟分割表格

Hyper-v 允許壓縮虛擬磁片（VHDX）檔案，而不考慮磁片上可能存在的任何磁碟分割、磁片區或檔案系統資料結構。 如果 VHDX 已壓縮到 VHDX 結尾前面的位置，則資料可能會遺失、分割區可能損毀，或讀取分割區時，可能會傳回不正確資料。

調整 VHD 或 VHDX 的大小之後，系統管理員應該使用像是 fdisk 或 parted 的公用程式來更新分割區、磁片區和檔案系統結構，以反映磁片大小的變更。 壓縮或擴充具有 GUID 磁碟分割表格（GPT）的 VHD 或 VHDX 大小，將會在使用磁碟分割管理工具來檢查磁碟分割配置時產生警告，而且系統管理員會收到警告，以修正第一個和第二個 GPT 標頭。 在不遺失資料的情況下，可以安全地執行此手動步驟。

## <a name="see-also"></a>另請參閱

* [Windows 上的 Hyper-v 支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [在 Hyper-v 上執行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [部署 Hyper-V 叢集](https://technet.microsoft.com/library/jj863389.aspx)

* [建立適用于 Azure 的 Linux 映射](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [在 Azure 上最佳化 Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
