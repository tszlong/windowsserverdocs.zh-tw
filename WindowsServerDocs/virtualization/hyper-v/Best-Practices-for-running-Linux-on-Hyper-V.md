---
title: 在 HYPER-V 上執行 Linux 的最佳做法
description: 提供的虛擬機器上執行 Linux 的建議
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: a24e2b1a1d79d52c1cc16f9e7c1b253d9b477aae
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284445"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>在 HYPER-V 上執行 Linux 的最佳做法

>適用於：Windows Server 2019，Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

本主題包含一份在 HYPER-V 上執行 Linux 虛擬機器建議。

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>微調動態 VHDX 檔案上的 Linux 檔案系統

即使檔案系統大多空白時，有些 Linux 檔案系統可能會耗用大量的實際磁碟空間。 若要減少動態 VHDX 檔案的實際磁碟空間使用量的數量，請考慮下列建議：

* 在建立 VHDX 時，使用 （從預設值 32 MB） 的 1 MB BlockSizeBytes 在 PowerShell 中，例如：

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* 建議要 ext3 ext4 格式，因為 ext4 更多的空間效率 ext3 動態 VHDX 檔案搭配使用。

* 當建立檔案系統所指定的數目為 4096，例如群組：

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>第 2 代虛擬機器上的 grub 功能表逾時

從第 2 代虛擬機器中的模擬移除傳統的硬體，因為 grub 功能表倒數計時器倒數太快 grub 功能表要顯示立即載入的預設項目。 Grub 修正之前使用 EFI 支援計時器，修改 **/boot/grub/grub.conf**，/**等/預設/grub**，或具有同等權限 「 逾時 = 100000"而非預設的 「 逾時 = 5"。

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>第 2 代虛擬機器上的 PxE 開機

因為 PIT 計時器沒有出現在 Generation 2 Virtual Machines，PxE TFTP 伺服器的網路連線可以提前終止，並防止從讀取 Grub 組態，並從伺服器載入的核心開機載入器。

在 RHEL 6.x，舊版 grub v0.97 EFI 開機載入器可用來取代 grub2，如下所示： [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

RHEL 以外的 Linux 發行版本 6.x，可以遵循類似的步驟，才能設定 grub v0.97 從 PxE 伺服器載入的 Linux 核心。

此外，在 RHEL/CentOS 6.6 鍵盤和滑鼠輸入不適用於前置安裝核心可防止指定的功能表中的安裝選項。 序列主控台必須設定為允許 選擇安裝選項。

* 在  **efidefault** PxE 伺服器上的檔案中，加入下列的核心參數 **」 主控台 = ttyS1"**

* HYPER-V 在 VM 上安裝的 COM 連接埠，使用下列 PowerShell cmdlet:

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

指定前置安裝核心 kickstart 檔案也會避免需要鍵盤和滑鼠輸入在安裝期間。

## <a name="use-static-mac-addresses-with-failover-clustering"></a>容錯移轉叢集搭配使用靜態 MAC 位址

將使用容錯移轉叢集部署的 Linux 虛擬機器應該設有每個虛擬網路介面卡的靜態媒體存取控制 (MAC) 位址。 在某些 Linux 版本中，網路設定可能會遺失在容錯移轉之後因為新的 MAC 位址指派給虛擬網路介面卡。 為避免遺失的網路組態，請確定每個虛擬網路介面卡具有靜態 MAC 位址。 您可以藉由編輯 HYPER-V 管理員] 或 [容錯移轉叢集管理員中的虛擬機器的設定來設定 MAC 位址。

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>使用 Hyper V 特定網路介面卡，未傳統網路介面卡

設定及使用虛擬乙太網路介面卡，也就是強化效能的 Hyper-v 特定網路卡。 如果舊版和 Hyper-v 特定網路介面卡已連接至虛擬機器，網路名稱中的輸出**ifconfig-a**這類可能會顯示值隨機 **_tmp12000801310**。 使用 Linux 虛擬機器中的 Hyper-v 特定網路介面卡時，若要避免這個問題，移除所有的傳統網路介面卡。

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>用於更佳的磁碟 I/O 效能的 I/O 排程器 NOOP

Linux 核心會有四個不同的 I/O 排程器，以重新排列要求不同的演算法。 NOOP 是先進先出佇列傳遞由 hypervisor 所進行的排程決策。 建議您在 HYPER-V 上執行 Linux 虛擬機器時，使用 NOOP 排程器為。 若要變更特定的裝置，開機載入器的組態中的排程器 (/ etc/grub.conf，例如)，新增**電梯 = noop**核心參數，並然後重新啟動。

## <a name="numa"></a>NUMA

Linux 核心版本更舊版本 2.6.37 以前版本不支援 NUMA 在 HYPER-V 上較大 VM 大小。 這個問題主要會影響較舊散發套件使用上游 Red Hat 2.6.32 kernel 和已修正在 Red Hat Enterprise Linux (RHEL) 6.6 (核心-2.6.32-504)。 執行 2.6.37 以前，自訂核心或 RHEL 為基礎的核心，比 2.6.32-504 以前的開機參數必須設定系統`numa=off`在 grub.conf 的核心命令列上。 如需詳細資訊，請參閱 < [Red Hat KB 436883](https://access.redhat.com/solutions/436883)。

## <a name="reserve-more-memory-for-kdump"></a>保留更多記憶體給 kdump

萬一傾印擷取核心最後異常，以在開機時，會保留更多記憶體給核心。 例如，將參數變更**crashkernel = 384M-:128M**要**crashkernel = 384M-:256M** Ubuntu grub 組態檔中。

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>縮小 VHDX 或擴充 VHD 和 VHDX 檔案，可能導致錯誤的 GPT 磁碟分割資料表

HYPER-V 可讓壓縮虛擬磁碟 (VHDX) 檔案，而不必考慮任何分割區、 磁碟區或可能存在於磁碟的檔案系統資料結構。 如果 VHDX 會縮小以 VHDX 結尾處分割區結尾之前，可能會遺失資料，可以傳回資料分割可能會損毀或無效的資料，分割區讀取時。

調整大小的 VHD 或 VHDX 之後, 系統管理員應該使用 fdisk 之類的公用程式，或 parted 更新資料分割、 量，以及檔案系統結構，以反映中的磁碟大小的變更。 縮小或擴充 VHD 或 VHDX 具有 GUID 磁碟分割表格 (GPT) 的大小就會在資料分割管理工具用來檢查資料分割配置，以及系統管理員將會收到警告，若要修正的第一個和第二個 GPT 標頭時造成警告。 此手動步驟可以安全地執行不會遺失資料。

## <a name="see-also"></a>另請參閱

* [Windows 上 HYPER-V 的受支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [在 HYPER-V 上執行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [部署 HYPER-V 叢集](https://technet.microsoft.com/library/jj863389.aspx)

* [建立適用於 Azure 的 Linux 映像](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [最佳化 Azure 上 Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
