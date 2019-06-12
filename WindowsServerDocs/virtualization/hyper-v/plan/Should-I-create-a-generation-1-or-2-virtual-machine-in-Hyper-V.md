---
title: 應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？
description: 提供這類支援的考量開機方法和其他可協助您選擇哪一個產生符合您需求的功能差異。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 95ececde8a1b8c591ea2baf367a93f63ee55a6e3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811991"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？

>適用於：Windows 10，Windows Server 2016、 Microsoft HYPER-V Server 2016、windows Server 2019，Microsoft HYPER-V Server 2019

> [!NOTE]
> 如果您打算不斷上傳 Windows 虛擬機器 (VM) 從內部部署 Microsoft Azure、 第 1 代和第 2 代 Vm 的 VHD 檔案格式，而且有固定大小的磁碟支援。 請參閱[第 2 代 Vm 在 Azure 上](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2)若要深入了解 Azure 上支援的第 2 代功能。 如需有關如何上傳 Windows VHD 或 VHDX 的詳細資訊，請參閱 <<c0> [ 準備 Windows VHD 或 VHDX 以上傳至 Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。

您選擇来建立第 1 代或第 2 代虛擬機器取決於您想要安裝和您想要用來部署虛擬機器的開機方法上的客體作業系統。 我們建議您建立以充分利用的功能，例如安全開機，除非其中一個下列的陳述式，則為 true 的第 2 代虛擬機器：  

- 不是您想要從開機的 VHD [UEFI 相容](https://technet.microsoft.com/library/hh824898.aspx)。  
- 第 2 代不支援您想要在虛擬機器上執行的作業系統。  
- 第 2 代不支援您想要使用的開機方法。  

如需所提供第 2 代虛擬機器功能的詳細資訊，請參閱[HYPER-V 功能的相容性世代與客體各自](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。

您無法變更它建立之後，虛擬機器的世代。 因此，我們建議您檢閱的考量，以及選擇作業系統、 開機方法，以及您想要使用您選擇的產生之前的功能。  

## <a name="which-guest-operating-systems-are-supported"></a>支援哪些客體作業系統？

第 1 代虛擬機器支援大部分的客體作業系統。 第 2 代虛擬機器支援最 64 位元版本的 Windows 和 Linux 和 FreeBSD 作業系統的較新版本。 若要查看哪些虛擬機器的世代支援您想要安裝客體作業系統中使用下列各節。  

- [Windows 客體作業系統支援](#windows-guest-operating-system-support)  

- [CentOS 和 Red Hat Enterprise Linux 客體作業系統支援](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Debian 客體作業系統支援](#debian-guest-operating-system-support)  

- [FreeBSD 客體作業系統支援](#freebsd-guest-operating-system-support)  

- [Oracle Linux 客體作業系統支援](#oracle-linux-guest-operating-system-support)  

- [SUSE 客體作業系統支援](#suse-guest-operating-system-support)  

- [Ubuntu 客體作業系統支援](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Windows 客體作業系統支援

下表顯示哪些 64 位元版本的 Windows 可用於為客體作業系統第 1 代和第 2 代虛擬機器。  

|64 位元版本的 Windows|第 1 代|第 2 代|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

下表顯示的 32 位元版本的 Windows 可用於為客體作業系統第 1 代和第 2 代虛擬機器。

|32 位元版本的 Windows|第 1 代|第 2 代|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>CentOS 和 Red Hat Enterprise Linux 客體作業系統支援

下表顯示哪些版本的 Red Hat Enterprise Linux \(RHEL\) CentOS 可以使用，做為客體作業系統的第 1 代和第 2 代虛擬機器。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|RHEL/CentOS 7.x 系列|&#10004;|&#10004;|  
|RHEL/CentOS 6.x 系列|&#10004;|&#10004;<br />**注意：** 只支援 Windows Server 2016 和更新版本。|  
|RHEL/CentOS 5.x 系列|&#10004;| &#10006;|  

如需詳細資訊，請參閱 < [CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="debian-guest-operating-system-support"></a>Debian 客體作業系統支援  

下表顯示的 Debian 版本有哪些可用做為客體作業系統的第 1 代和第 2 代虛擬機器。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Debian 7.x 系列|&#10004;| &#10006;|  
|Debian 8.x 系列|&#10004;|&#10004;|  

如需詳細資訊，請參閱 <<c0> [ 在 HYPER-V 上的 Debian 虛擬機器](../Supported-Debian-virtual-machines-on-Hyper-V.md)。  

### <a name="freebsd-guest-operating-system-support"></a>FreeBSD 客體作業系統支援

下表顯示的 FreeBSD 版本有哪些可用做為客體作業系統的第 1 代和第 2 代虛擬機器。  

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 和 10.1|&#10004;| &#10006;|  
|FreeBSD 9.1 和 9.3|&#10004;| &#10006;|  
|FreeBSD 8.4|&#10004;| &#10006;|  

如需詳細資訊，請參閱 <<c0> [ 在 HYPER-V 上的 FreeBSD 虛擬機器](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md)。  

### <a name="oracle-linux-guest-operating-system-support"></a>Oracle Linux 客體作業系統支援  

下表顯示哪些版本的 Red Hat 相容核心系列您可以針對第 1 代和第 2 代虛擬機器使用做為客體作業系統。  

|Red Hat 相容核心系列版本|第 1 代|第 2 代|  
|---------------------------------------------|----------------|----------------|  
|Oracle Linux 7.x 系列|&#10004;|&#10004;|
|Oracle Linux 6.x 系列|&#10004;| &#10006;|  

下表顯示哪些版本的 Unbreakable Enterprise Kernel 可用做為客體作業系統的第 1 代和第 2 代虛擬機器。

|Unbreakable Enterprise Kernel (UEK) 版本|第 1 代|第 2 代|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

如需詳細資訊，請參閱 <<c0> [ 在 HYPER-V 上的 Oracle Linux 虛擬機器](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="suse-guest-operating-system-support"></a>SUSE 客體作業系統支援

下表顯示的 SUSE 版本有哪些可用做為客體作業系統的第 1 代和第 2 代虛擬機器。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|SUSE Linux Enterprise Server 12 系列|&#10004;|&#10004;|  
|SUSE Linux Enterprise Server 11 系列|&#10004;| &#10006;|  
|Open SUSE 12.3|&#10004;| &#10006;|  

如需詳細資訊，請參閱 < [SUSE 虛擬機器在 HYPER-V 上](../Supported-SUSE-virtual-machines-on-Hyper-V.md)。  

### <a name="ubuntu-guest-operating-system-support"></a>Ubuntu 客體作業系統支援

下表顯示哪些版本的 Ubuntu 可用做為客體作業系統的第 1 代和第 2 代虛擬機器。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 和更新版本|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

如需詳細資訊，請參閱 < [Ubuntu 虛擬機器在 HYPER-V 上](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md)。  

## <a name="how-can-i-boot-the-virtual-machine"></a>如何啟動虛擬機器？

下表顯示哪些開機第 1 代和第 2 代虛擬機器支援的方法。  

|開機方法|第 1 代|第 2 代|  
|---------------|----------------|----------------|  
|使用標準網路介面卡進行 PXE 開機| &#10006;|&#10004;|  
|使用傳統網路介面卡進行 PXE 開機|&#10004;| &#10006;|  
|從 SCSI 虛擬硬碟開機 (。VHDX) 或虛擬 DVD (。ISO)| &#10006;|&#10004;|  
|從 IDE 控制器的虛擬硬碟開機 (。VHD) 或虛擬 DVD (。ISO)|&#10004;| &#10006;|  
|從磁碟開機 (。VFD)|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>使用第 2 代虛擬機器的優點有哪些？

以下是一些您使用第 2 代虛擬機器時的優點：  
- **安全開機**這是驗證開機載入器是否由 UEFI 資料庫中以協助防止未經授權的韌體、 作業系統或 UEFI 驅動程式在開機時執行的受信任授權單位簽署的功能。 第 2 代虛擬機器預設會啟用安全開機。 如果您需要執行客體作業系統不支援安全開機，您可以停用它之後建立的虛擬機器。  如需詳細資訊，請參閱[安全開機](https://technet.microsoft.com/library/dn486875.aspx)。  

    安全開機第 2 代 Linux 虛擬機器，您需要選擇 UEFI CA 安全開機範本，當您建立虛擬機器。  

- **較大的開機磁碟區**第 2 代虛擬機器的最大的開機磁碟區為 64 TB。 這是所支援的最大磁碟大小。VHDX。 第 1 代虛擬機器，最大的開機磁碟區是針對 2 TB。VHDX 和 2040 GB。VHD。 如需詳細資訊，請參閱 < [HYPER-V Virtual Hard Disk Format Overview](https://technet.microsoft.com/library/hh831446.aspx)。  

  您可能也會看到第 2 代虛擬機器的虛擬機器開機和安裝時間稍微的改善。

## <a name="whats-the-difference-in-device-support"></a>在 裝置支援的差異為何？

下表比較的第 1 代和第 2 代虛擬機器之間的可用的裝置。  

|第 1 代裝置|第 2 代替代項目|第 2 代增強功能|  
|-----------------------|----------------------------|-----------------------------|  
|IDE 控制器|虛擬 SCSI 控制器|從.vhdx (最大大小為 64 TB 與線上調整大小功能) 開機|  
|IDE CD-ROM|虛擬 SCSI CD-ROM|每個 SCSI 控制器最多支援 64 個 SCSI DVD 裝置。|  
|舊有的 BIOS|UEFI 韌體|安全開機|  
|傳統網路介面卡|綜合網路介面卡|IPv4 與 IPv6 網路開機|  
|軟碟機控制器與 DMA 控制器|不支援磁碟機控制器|N/A|  
|通用非同步接收器/傳輸器 (UART) COM 連接埠|選擇性 UART 偵錯|更快速且更可靠|  
|i8042 鍵盤控制器|軟體型輸入|因為沒有任何模擬，所以使用較少的資源。 另外降低了客體作業系統的攻擊面。|  
|PS/2 鍵盤|軟體型鍵盤|因為沒有任何模擬，所以使用較少的資源。 另外降低了客體作業系統的攻擊面。|  
|PS/2 滑鼠|軟體型滑鼠|因為沒有任何模擬，所以使用較少的資源。 另外降低了客體作業系統的攻擊面。|  
|S3 視訊|軟體型視訊|因為沒有任何模擬，所以使用較少的資源。 另外降低了客體作業系統的攻擊面。|  
|PCI 匯流排|不再需要|N/A|  
|可程式化插斷控制器 (PIC)|不再需要|N/A|  
|可程式化間隔計時器 (PIT)|不再需要|N/A|  
|進階 I/O 裝置|不再需要|N/A|  

## <a name="more-about-generation-2-virtual-machines"></a>深入了解第 2 代虛擬機器

以下是一些其他的秘訣，需使用第 2 代虛擬機器。

### <a name="attach-or-add-a-dvd-drive"></a>附加或新增 DVD 光碟機

- 您無法將實體 CD 或 DVD 磁碟機附加至第 2 代虛擬機器。 第 2 代虛擬 DVD 光碟機只支援 ISO 映像檔。 若要建立 Windows 環境的 ISO 映像檔，可以使用 Oscdimg 命令列工具。 如需詳細資訊，請參閱＜ [Oscdimg 命令列選項](https://msdn.microsoft.com/library/hh824847.aspx)＞。
- 當您使用 NEW-VM Windows PowerShell cmdlet 建立新的虛擬機器時，第 2 代虛擬機器沒有 DVD 光碟機。 虛擬機器執行時，您可以新增 DVD 光碟機。

### <a name="use-uefi-firmware"></a>使用 UEFI 韌體

- 安全開機或 UEFI 韌體不需要對實體 HYPER-V 主機。 HYPER-V 提供給虛擬機器都是獨立的 HYPER-V 主機上的虛擬韌體。
- 第 2 代虛擬機器中的 UEFI 韌體不支援安全開機的安裝模式。
- 我們不支援第 2 代虛擬機器中執行 UEFI 殼層或其他 UEFI 應用程式。 如果非 Microsoft UEFI 殼層或 UEFI 應用程式是直接從來源編譯的，則技術上而言可以使用它們。 如果這些應用程式未經正確數位簽署，您必須停用安全開機的虛擬機器。

### <a name="work-with-vhdx-files"></a>使用的 VHDX 檔案

- 您可以調整大小的虛擬機器執行時，包含第 2 代虛擬機器的開機磁碟的 VHDX 檔案。
- 我們不支援也建議您建立可開機第 1 代和第 2 代虛擬機器的 VHDX 檔案。  
- 虛擬機器世代是虛擬機器的屬性，而不是虛擬硬碟的屬性。 因此您無法分辨的 VHDX 檔案已由第 1 代或第 2 代虛擬機器。  
- 建立與層代 2 的虛擬機器可以連接到 IDE 控制器或 SCSI 控制器第 1 代虛擬機器的 VHDX 檔案。 不過，如果這是可開機的 VHDX 檔案，將不會開機第 1 代虛擬機器。

### <a name="use-ipv6-instead-of-ipv4"></a>使用 IPv6 而不是 IPv4

第 2 代虛擬機器預設使用 IPv4。 若要改為使用 IPv6，執行[Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx) Windows PowerShell cmdlet。 比方說，下列命令會針對名為 TestVM 的虛擬機器的 ipv6 設定 慣用的通訊協定：  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>新增的 COM 連接埠的核心偵錯

在新增之前，COM 連接埠無法使用第 2 代虛擬機器。 您可以使用 Windows PowerShell 或 Windows Management Instrumentation (WMI) 來這樣做。 這些步驟會示範如何使用 Windows PowerShell。

若要新增的 COM 連接埠：  

1. 停用安全開機。 核心偵錯不相容於安全開機。 請確定虛擬機器處於關閉狀態，然後使用[Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet。 例如，下列命令停用安全開機，虛擬機器 TestVM 上：  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. 將 COM 連接埠。 使用[Set-vmcomport](https://technet.microsoft.com/library/hh848616.aspx) cmdlet 來執行這項操作。 比方說，下列命令會在虛擬機器 TestVM，連接到具名管道 TestPipe 本機電腦上設定的第一個 COM 連接埠：  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> 設定的 COM 連接埠未列在 HYPER-V 管理員中的虛擬機器的設定。

## <a name="see-also"></a>另請參閱  

- [Linux 和 FreeBSD 虛擬機器之 HYPER-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [搭配 VMConnect 的 HYPER-V 虛擬機器上使用本機資源](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [規劃 Windows Server 2016 中的 HYPER-V 延展性](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
