---
title: 我應該在 Hyper-v 中建立第1代或第2代虛擬機器嗎？
description: 提供支援的開機方法和其他功能差異等考慮，以協助您選擇符合您需求的世代。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: kbdazure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 0e8b8dbaa937229b5a87560f3993bb07d7cd7ff8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860771"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>我應該在 Hyper-v 中建立第1代或第2代虛擬機器嗎？

>適用于： Windows 10、Windows Server 2016、Microsoft Hyper-v Server 2016、Windows Server 2019、Microsoft Hyper-v Server 2019

> [!NOTE]
> 如果您打算從內部部署將 Windows 虛擬機器（Vm）上傳至 Microsoft Azure，第1代和第2代 Vm 會使用 VHD 檔案格式，並可支援固定大小的磁片。 若要深入瞭解 Azure 上支援的第2代功能，請參閱[azure 上的第2代 vm](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) 。 如需上傳 Windows VHD 或 VHDX 的詳細資訊，請參閱[準備要上傳至 Azure 的 WINDOWS vhd 或 vhdx](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。

您選擇建立第1代或第2代虛擬機器的方式，取決於您想要安裝的客體作業系統，以及您要用來部署虛擬機器的開機方法。 我們建議您建立第2代虛擬機器，以利用安全開機之類的功能，除非下列其中一個陳述為 true：  

- 您要從中開機的 VHD 與 UEFI 不[相容](https://technet.microsoft.com/library/hh824898.aspx)。  
- 第2代不支援您想要在虛擬機器上執行的作業系統。  
- 第2代不支援您想要使用的開機方法。  

如需第2代虛擬機器可用功能的詳細資訊，請參閱[世代和來賓的 hyper-v 功能相容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。

建立虛擬機器之後，您就無法變更它的世代。 因此，我們建議您在選擇世代之前，先參閱此處的考慮，以及選擇要使用的作業系統、開機方法和功能。  

## <a name="which-guest-operating-systems-are-supported"></a>支援哪些客體作業系統？

第1代虛擬機器支援大部分的客體作業系統。 第2代虛擬機器支援大部分64位版本的 Windows，以及更多目前的 Linux 和 FreeBSD 作業系統版本。 使用下列各節來查看哪一代的虛擬機器支援您想要安裝的客體作業系統。  

- [Windows 客體作業系統支援](#windows-guest-operating-system-support)  

- [CentOS 和 Red Hat Enterprise Linux 客體作業系統支援](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Debian 客體作業系統支援](#debian-guest-operating-system-support)  

- [FreeBSD 客體作業系統支援](#freebsd-guest-operating-system-support)  

- [Oracle Linux 客體作業系統支援](#oracle-linux-guest-operating-system-support)  

- [SUSE 客體作業系統支援](#suse-guest-operating-system-support)  

- [Ubuntu 客體作業系統支援](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Windows 客體作業系統支援

下表顯示您可以使用哪些64位版本的 Windows，做為第1代和第2代虛擬機器的客體作業系統。  

|64位版本的 Windows|第 1 代|第 2 代|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

下表顯示您可以使用哪些32位版本的 Windows，做為第1代和第2代虛擬機器的客體作業系統。

|32位版本的 Windows|第 1 代|第 2 代|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>CentOS 和 Red Hat Enterprise Linux 客體作業系統支援

下表顯示您可以使用哪些版本的 Red Hat Enterprise Linux \(RHEL\) 和 CentOS，做為第1代和第2代虛擬機器的客體作業系統。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|RHEL/CentOS 7.x 系列|&#10004;|&#10004;|  
|RHEL/CentOS 6.x 系列|&#10004;|&#10004;<br />**注意：** 僅在 Windows Server 2016 和更新版本上支援。|  
|RHEL/CentOS 5.x 系列|&#10004;| &#10006;|  

如需詳細資訊，請參閱[CentOS 和 Red Hat Enterprise Linux hyper-v 上的虛擬機器](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="debian-guest-operating-system-support"></a>Debian 客體作業系統支援  

下表顯示您可以使用哪些版本的 Debian 作為第1代和第2代虛擬機器的客體作業系統。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Debian 7.x 系列|&#10004;| &#10006;|  
|Debian 8.x 系列|&#10004;|&#10004;|  

如需詳細資訊，請參閱[Debian virtual 機器 On hyper-v](../Supported-Debian-virtual-machines-on-Hyper-V.md)。  

### <a name="freebsd-guest-operating-system-support"></a>FreeBSD 客體作業系統支援

下表顯示您可以使用哪些版本的 FreeBSD 作為第1代和第2代虛擬機器的客體作業系統。  

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 和10。1|&#10004;| &#10006;|  
|FreeBSD 9.1 和9。3|&#10004;| &#10006;|  
|FreeBSD 8。4|&#10004;| &#10006;|  

如需詳細資訊，請參閱[FreeBSD virtual 機器 On hyper-v](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md)。  

### <a name="oracle-linux-guest-operating-system-support"></a>Oracle Linux 客體作業系統支援  

下表顯示您可以用來作為第1代和第2代虛擬機器之客體作業系統的 Red Hat 相容核心系列版本。  

|Red Hat 相容核心系列版本|第 1 代|第 2 代|  
|---------------------------------------------|----------------|----------------|  
|Oracle Linux 7.x 系列|&#10004;|&#10004;|
|Oracle Linux 6.x 系列|&#10004;| &#10006;|  

下表顯示您可以用來做為第1代和第2代虛擬機器之客體作業系統的 Unbreakable Enterprise 核心版本。

|Unbreakable Enterprise Kernel （UEK）版本|第 1 代|第 2 代|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

如需詳細資訊，請參閱[hyper-v 上的 Oracle Linux 虛擬機器](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)。  

### <a name="suse-guest-operating-system-support"></a>SUSE 客體作業系統支援

下表顯示您可以使用哪些 SUSE 版本做為第1代和第2代虛擬機器的客體作業系統。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|SUSE Linux Enterprise Server 12 系列|&#10004;|&#10004;|  
|SUSE Linux Enterprise Server 11 系列|&#10004;| &#10006;|  
|開啟 SUSE 12。3|&#10004;| &#10006;|  

如需詳細資訊，請參閱[hyper-v 上的 SUSE 虛擬機器](../Supported-SUSE-virtual-machines-on-Hyper-V.md)。  

### <a name="ubuntu-guest-operating-system-support"></a>Ubuntu 客體作業系統支援

下表顯示您可以使用哪些 Ubuntu 版本做為第1代和第2代虛擬機器的客體作業系統。

|作業系統版本|第 1 代|第 2 代|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 和更新版本|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

如需詳細資訊，請參閱[hyper-v 上的 Ubuntu 虛擬機器](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md)。  

## <a name="how-can-i-boot-the-virtual-machine"></a>如何啟動虛擬機器？

下表顯示第1代和第2代虛擬機器支援哪些開機方法。  

|開機方法|第 1 代|第 2 代|  
|---------------|----------------|----------------|  
|使用標準網路介面卡進行 PXE 開機| &#10006;|&#10004;|  
|使用傳統網路介面卡進行 PXE 開機|&#10004;| &#10006;|  
|從 SCSI 虛擬硬碟（）開機。VHDX）或虛擬 DVD （。ISO| &#10006;|&#10004;|  
|從 IDE 控制器虛擬硬碟（）開機。VHD）或虛擬 DVD （）。ISO|&#10004;| &#10006;|  
|從軟碟開機（.VFD|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>使用第2代虛擬機器的優點為何？

以下是您在使用第2代虛擬機器時取得的一些優點：  
- **安全開機**這項功能可確認開機載入器是由 UEFI 資料庫中的受信任授權單位所簽署，以協助防止未經授權的固件、作業系統或 UEFI 驅動程式在開機時執行。 第 2 代虛擬機器預設會啟用安全開機。 如果您需要執行安全開機不支援的客體作業系統，可以在建立虛擬機器之後將它停用。  如需詳細資訊，請參閱[安全開機](https://technet.microsoft.com/library/dn486875.aspx)。  

    若要保護開機第2代 Linux 虛擬機器，您必須在建立虛擬機器時選擇 UEFI CA 安全開機範本。  

- **較大的開機磁碟區**第2代虛擬機器的開機磁碟區上限為 64 TB。 這是所支援的最大磁片大小。VHDX. 針對第1代虛擬機器，開機磁碟區的最大值為2TB。的 VHDX 和2040GB。VHD. 如需詳細資訊，請參閱[Hyper-v 虛擬硬碟格式總覽](https://technet.microsoft.com/library/hh831446.aspx)。  

  您也可能會看到第2代虛擬機器的虛擬機器開機和安裝時間稍微改善。

## <a name="whats-the-difference-in-device-support"></a>裝置支援的差異為何？

下表比較第1代與第2代虛擬機器之間可用的裝置。  

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

## <a name="more-about-generation-2-virtual-machines"></a>深入瞭解第2代虛擬機器

以下是有關使用第2代虛擬機器的一些額外秘訣。

### <a name="attach-or-add-a-dvd-drive"></a>連接或新增 DVD 光碟機

- 您無法將實體 CD 或 DVD 光碟機連接到第2代虛擬機器。 第 2 代虛擬 DVD 光碟機只支援 ISO 映像檔。 若要建立 Windows 環境的 ISO 映像檔，可以使用 Oscdimg 命令列工具。 如需詳細資訊，請參閱＜ [Oscdimg 命令列選項](https://msdn.microsoft.com/library/hh824847.aspx)＞。
- 當您使用新的-VM Windows PowerShell Cmdlet 建立新的虛擬機器時，第2代虛擬機器沒有 DVD 光碟機。 當虛擬機器正在執行時，您可以新增 DVD 光碟機。

### <a name="use-uefi-firmware"></a>使用 UEFI 固件

- 實體 Hyper-v 主機不需要安全開機或 UEFI 固件。 Hyper-v 提供虛擬機器給獨立于 Hyper-v 主機上的虛擬機器。
- 第2代虛擬機器中的 UEFI 固件不支援安全開機的設定模式。
- 我們不支援在第2代虛擬機器中執行 UEFI shell 或其他 UEFI 應用程式。 如果非 Microsoft UEFI 殼層或 UEFI 應用程式是直接從來源編譯的，則技術上而言可以使用它們。 如果這些應用程式未經過適當的數位簽署，您必須停用虛擬機器的安全開機。

### <a name="work-with-vhdx-files"></a>使用 VHDX 檔案

- 當虛擬機器正在執行時，您可以調整包含第2代虛擬機器開機磁碟區的 VHDX 檔案大小。
- 我們不支援或建議您建立可開機至第1代和第2代虛擬機器的 VHDX 檔案。  
- 虛擬機器世代是虛擬機器的屬性，而不是虛擬硬碟的屬性。 因此，您無法判斷 VHDX 檔案是由第1代或第2代虛擬機器所建立。  
- 使用第2代虛擬機器建立的 VHDX 檔案可以連接到 IDE 控制器或第1代虛擬機器的 SCSI 控制器。 不過，如果這是可開機的 VHDX 檔案，第1代虛擬機器將無法開機。

### <a name="use-ipv6-instead-of-ipv4"></a>使用 IPv6，而不是 IPv4

第 2 代虛擬機器預設使用 IPv4。 若要改為使用 IPv6，請執行[Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx) Windows PowerShell Cmdlet。 例如，下列命令會將名為 TestVM 之虛擬機器的慣用通訊協定設定為 IPv6：  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>新增用於內核偵錯工具的 COM 埠

在第2代虛擬機器中，COM 埠無法使用，直到您新增它們為止。 您可以使用 Windows PowerShell 或 Windows Management Instrumentation （WMI）來執行此動作。 這些步驟說明如何使用 Windows PowerShell 來執行此動作。

若要新增 COM 埠：  

1. 停用安全開機。 內核調試與安全開機不相容。 請確定虛擬機器處於關閉狀態，然後使用[set-vmfirmware 指令程式](https://technet.microsoft.com/library/dn464287.aspx)。 例如，下列命令會在虛擬機器 TestVM 上停用安全開機：  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. 新增 COM 通訊埠。 使用[set-vmcomport](https://technet.microsoft.com/library/hh848616.aspx) Cmdlet 來執行此動作。 例如，下列命令會將虛擬機器（TestVM）上的第一個 COM 埠設定為連接到本機電腦上的具名管道（TestPipe）：  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> 設定的 COM 埠不會列在 [Hyper-v 管理員] 中的虛擬機器設定中。

## <a name="see-also"></a>另請參閱  

- [Hyper-V 上的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [使用 VMConnect 在 Hyper-V 虛擬機器上使用本機資源](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [規劃 Windows Server 2016 中的 Hyper-v 擴充性](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
