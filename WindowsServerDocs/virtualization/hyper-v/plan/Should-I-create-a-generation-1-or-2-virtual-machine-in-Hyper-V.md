---
title: 我應在 Hyper-V 建立第 1 或 2 代的虛擬機器嗎？
description: 提供如支援的開機方法和其他功能差異等考慮，協助您選擇哪一種世代符合您的需求。
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/05/2016
ms.openlocfilehash: f9cdb144e7edacf8a1be0f2d98509517adf5c87e
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746593"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>我應在 Hyper-V 建立第 1 或 2 代的虛擬機器嗎？

>適用于： Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

> [!NOTE]
> 如果您打算將 Windows 虛擬機器 (Vm) 從內部部署環境上傳至 Microsoft Azure、第1代和第2代 Vm （VHD 檔案格式），並支援固定大小的磁片。 請參閱 [azure 上的第2代 vm](/azure/virtual-machines/windows/generation-2) ，以深入瞭解 azure 上所支援的層代2功能。 如需上傳 Windows VHD 或 VHDX 的詳細資訊，請參閱 [準備 WINDOWS vhd 或 vhdx 以上傳至 Azure](/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。

建立第1代或第2代虛擬機器的選擇取決於您想要安裝的客體作業系統，以及您想要用來部署虛擬機器的開機方法。 建議您建立第2代虛擬機器，以利用安全開機等功能，除非下列其中一個陳述成立：

- 您要開機的 VHD 與 [UEFI 不相容](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824898(v=win.10))。
- 層代2不支援您想要在虛擬機器上執行的作業系統。
- 層代2不支援您想要使用的開機方法。

如需第2代虛擬機器可用功能的詳細資訊，請參閱 [hyper-v 功能與產生和來賓的功能相容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。

建立虛擬機器之後，您就無法變更虛擬機器的世代。 因此，我們建議您複習這裡的考慮，以及選擇您要在選擇世代之前使用的作業系統、開機方法和功能。

## <a name="which-guest-operating-systems-are-supported"></a>受支援的客體作業系統有哪些？

第1代虛擬機器支援大部分的客體作業系統。 第2代虛擬機器支援最多64位版本的 Windows，以及更多目前的 Linux 和 FreeBSD 作業系統版本。 請使用以下各節來查看哪一代的虛擬機器支援您要安裝的客體作業系統。

- [Windows 客體作業系統支援](#windows-guest-operating-system-support)

- [CentOS 和 Red Hat Enterprise Linux 客體作業系統支援](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)

- [Debian 客體作業系統支援](#debian-guest-operating-system-support)

- [FreeBSD 客體作業系統支援](#freebsd-guest-operating-system-support)

- [Oracle Linux 客體作業系統支援](#oracle-linux-guest-operating-system-support)

- [SUSE 客體作業系統支援](#suse-guest-operating-system-support)

- [Ubuntu 客體作業系統支援](#ubuntu-guest-operating-system-support)

### <a name="windows-guest-operating-system-support"></a>Windows 客體作業系統支援

下表顯示您可以在第1代和第2代虛擬機器中，將哪些64位版本的 Windows 用作客體作業系統。

|64位版本的 Windows|第 1 代|第 2 代|
|-------------------------------|----------------|----------------|
| Windows Server 2019 |&#10004;|&#10004;|
| Windows Server 2016 |&#10004;|&#10004;|
| Windows Server 2012 R2 |&#10004;|&#10004;|
| Windows Server 2012 |&#10004;|&#10004;|
|Windows Server 2008 R2|&#10004;| &#10006;|
|Windows Server 2008|&#10004;| &#10006;|
|Windows 10|&#10004;|&#10004;|
|Windows 8。1|&#10004;|&#10004;|
|Windows 8|&#10004;|&#10004;|
|Windows 7|&#10004;| &#10006;|

下表顯示您可以在第1代和第2代虛擬機器中，將哪些32位版本的 Windows 用作客體作業系統。

|32位版本的 Windows|第 1 代|第 2 代|
|-------------------------------|----------------|----------------|
|Windows 10|&#10004;| &#10006;|
|Windows 8。1|&#10004;| &#10006;|
|Windows 8|&#10004;| &#10006;|
|Windows 7|&#10004;| &#10006;|

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>CentOS 和 Red Hat Enterprise Linux 客體作業系統支援

下表顯示哪些版本的 Red Hat Enterprise Linux \( RHEL \) 和 CentOS 可作為第1代和第2代虛擬機器的客體作業系統使用。

|作業系統版本|第 1 代|第 2 代|
|-----------------------------|----------------|----------------|
|RHEL/CentOS 7.x 系列|&#10004;|&#10004;|
|RHEL/CentOS 6.x 系列|&#10004;|&#10004;<br />**注意：** 只有在 Windows Server 2016 和更新版本上才支援。|
|RHEL/CentOS 5.x 系列|&#10004;| &#10006;|

如需詳細資訊，請參閱 [CentOS 和 Red Hat Enterprise Linux hyper-v 上的虛擬機器](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)。

### <a name="debian-guest-operating-system-support"></a>Debian 客體作業系統支援

下表顯示您可以用來做為第1代和第2代虛擬機器之客體作業系統的 Debian 版本。

|作業系統版本|第 1 代|第 2 代|
|-----------------------------|----------------|----------------|
|Debian 7.x 系列|&#10004;| &#10006;|
|Debian 8.x 系列|&#10004;|&#10004;|

如需詳細資訊，請參閱 [hyper-v 上的 Debian 虛擬機器](../Supported-Debian-virtual-machines-on-Hyper-V.md)。

### <a name="freebsd-guest-operating-system-support"></a>FreeBSD 客體作業系統支援

下表顯示您可以用來做為第1代和第2代虛擬機器之客體作業系統的 FreeBSD 版本。

|作業系統版本|第 1 代|第 2 代|
|-----------------------------|----------------|----------------|
|FreeBSD 10 和10。1|&#10004;| &#10006;|
|FreeBSD 9.1 和9。3|&#10004;| &#10006;|
|FreeBSD 8。4|&#10004;| &#10006;|

如需詳細資訊，請參閱 [hyper-v 上的 FreeBSD 虛擬機器](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md)。

### <a name="oracle-linux-guest-operating-system-support"></a>Oracle Linux 客體作業系統支援

下表顯示哪些版本的 Red Hat 相容核心系列可用來作為第1代和第2代虛擬機器的客體作業系統。

|Red Hat 相容核心系列版本|第 1 代|第 2 代|
|---------------------------------------------|----------------|----------------|
|Oracle Linux 7.x 系列|&#10004;|&#10004;|
|Oracle Linux 6.x 系列|&#10004;| &#10006;|

下表說明您可以使用哪些 Unbreakable Enterprise 核心版本作為第1代和第2代虛擬機器的客體作業系統。

|Unbreakable Enterprise Kernel (UEK) 版本|第 1 代|第 2 代|
|--------------------------------------------------|----------------|----------------|
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|

如需詳細資訊，請參閱 [hyper-v 上的 Oracle Linux 虛擬機器](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)。

### <a name="suse-guest-operating-system-support"></a>SUSE 客體作業系統支援

下表說明您可以使用哪些 SUSE 版本作為第1代和第2代虛擬機器的客體作業系統。

|作業系統版本|第 1 代|第 2 代|
|-----------------------------|----------------|----------------|
|SUSE Linux Enterprise Server 12 系列|&#10004;|&#10004;|
|SUSE Linux Enterprise Server 11 系列|&#10004;| &#10006;|
|開啟 SUSE 12。3|&#10004;| &#10006;|

如需詳細資訊，請參閱 [hyper-v 上的 SUSE 虛擬機器](../Supported-SUSE-virtual-machines-on-Hyper-V.md)。

### <a name="ubuntu-guest-operating-system-support"></a>Ubuntu 客體作業系統支援

下表顯示哪些版本的 Ubuntu 可作為第1代和第2代虛擬機器的客體作業系統使用。

|作業系統版本|第 1 代|第 2 代|
|-----------------------------|----------------|----------------|
|Ubuntu 14.04 和更新版本|&#10004;|&#10004;|
|Ubuntu 12.04|&#10004;| &#10006;|

如需詳細資訊，請參閱 [hyper-v 上的 Ubuntu 虛擬機器](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md)。

## <a name="how-can-i-boot-the-virtual-machine"></a>如何啟動虛擬機器？

下表顯示第1代和第2代虛擬機器支援的開機方法。

|開機方法|第 1 代|第 2 代|
|---------------|----------------|----------------|
|使用標準網路介面卡進行 PXE 開機| &#10006;|&#10004;|
|使用傳統網路介面卡進行 PXE 開機|&#10004;| &#10006;|
|從 SCSI 虛擬硬碟開機 (。VHDX) 或虛擬 DVD (。ISO) | &#10006;|&#10004;|
|從 IDE 控制器虛擬硬碟 ( 開機。VHD) 或虛擬 DVD (。ISO) |&#10004;| &#10006;|
|從軟碟開機 (。.VFD) |&#10004;| &#10006;|

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>使用第2代虛擬機器有哪些優點？

以下是您使用第2代虛擬機器時取得的一些優點：
- **安全開機** 這項功能會確認開機載入器是否由 UEFI 資料庫中的受信任授權單位簽署，以協助防止未經授權的固件、作業系統或 UEFI 驅動程式在開機時執行。 第 2 代虛擬機器預設會啟用安全開機。 如果您需要執行安全開機不支援的客體作業系統，您可以在建立虛擬機器之後停用該作業系統。  如需詳細資訊，請參閱[安全開機](/previous-versions/windows/it-pro/windows-8.1-and-8/dn486875(v=ws.11))。

    若要保護啟動第2代 Linux 虛擬機器，您必須在建立虛擬機器時選擇 UEFI CA 安全開機範本。

- **較大的開機磁碟區** 第2代虛擬機器的開機磁碟區上限為 64 TB。 這是所支援的最大磁片大小。VHDX. 針對第1代虛擬機器，開機磁碟區的最大值為 2 TB。的 VHDX 和2040GB。VHD. 如需詳細資訊，請參閱 [Hyper-v 虛擬硬碟格式總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831446(v=ws.11))。

  您也可能會看到虛擬機器開機和安裝時間在第2代虛擬機器上稍微改進。

## <a name="whats-the-difference-in-device-support"></a>裝置支援有何不同？

下表將比較第1代和第2代虛擬機器之間的可用裝置。

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

以下是有關使用第2代虛擬機器的一些其他秘訣。

### <a name="attach-or-add-a-dvd-drive"></a>連接或新增 DVD 光碟機

- 您無法將實體 CD 或 DVD 光碟機連接到第2代虛擬機器。 第 2 代虛擬 DVD 光碟機只支援 ISO 映像檔。 若要建立 Windows 環境的 ISO 映像檔，可以使用 Oscdimg 命令列工具。 如需詳細資訊，請參閱＜[Oscdimg 命令列選項](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824847(v=win.10))＞。
- 當您使用新的-VM Windows PowerShell Cmdlet 來建立新的虛擬機器時，第2代虛擬機器沒有 DVD 磁片磁碟機。 當虛擬機器正在執行時，您可以新增 DVD 光碟機。

### <a name="use-uefi-firmware"></a>使用 UEFI 固件

- 實體 Hyper-v 主機上不需要安全開機或 UEFI 固件。 Hyper-v 會提供虛擬機器給獨立于 Hyper-v 主機上的虛擬機器。
- 第2代虛擬機器中的 UEFI 固件不支援安全開機的安裝模式。
- 我們不支援在第2代虛擬機器中執行 UEFI shell 或其他 UEFI 應用程式。 如果非 Microsoft UEFI 殼層或 UEFI 應用程式是直接從來源編譯的，則技術上而言可以使用它們。 如果這些應用程式未經過數位簽署，您必須停用虛擬機器的安全開機。

### <a name="work-with-vhdx-files"></a>使用 VHDX 檔案

- 當虛擬機器正在執行時，您可以調整包含第2代虛擬機器之開機磁碟區的 VHDX 檔案大小。
- 我們不支援或建議您建立可從第1代和第2代虛擬機器開機的 VHDX 檔案。
- 虛擬機器世代是虛擬機器的屬性，而不是虛擬硬碟的屬性。 因此，您無法分辨是由第1代或第2代虛擬機器所建立的 VHDX 檔案。
- 使用第2代虛擬機器所建立的 VHDX 檔案可以附加至 IDE 控制器或第1代虛擬機器的 SCSI 控制器。 但是，如果這是可開機的 VHDX 檔案，第1代虛擬機器將不會開機。

### <a name="use-ipv6-instead-of-ipv4"></a>使用 IPv6 而不是 IPv4

第 2 代虛擬機器預設使用 IPv4。 若要改為使用 IPv6，請執行 [get-vmfirmware](/powershell/module/hyper-v/set-vmfirmware?view=win10-ps) Windows PowerShell Cmdlet。 例如，下列命令會針對名為 TestVM 的虛擬機器，將慣用的通訊協定設定為 IPv6：

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6
```

## <a name="add-a-com-port-for-kernel-debugging"></a>新增內核偵錯工具的 COM 埠

第2代虛擬機器無法使用 COM 埠，除非您將其新增。 您可以使用 Windows PowerShell 或 Windows Management Instrumentation (WMI) 來完成此動作。 這些步驟會示範如何使用 Windows PowerShell 來執行此作業。

若要新增 COM 埠：

1. 停用安全開機。 內核調試與安全開機不相容。 請確定虛擬機器處於關閉狀態，然後使用 [get-vmfirmware](/powershell/module/hyper-v/set-vmfirmware?view=win10-ps) Cmdlet。 例如，下列命令會停用虛擬機器 TestVM 上的安全開機：

    ```powershell
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off
    ```

2. 新增 COM 埠。 使用 Get-vmcomport 指令 Cmdlet 來執行這 [一](/powershell/module/hyper-v/set-vmcomport?view=win10-ps) 作業。 例如，下列命令會將虛擬機器 TestVM 上的第一個 COM 埠設定為連接到本機電腦上的具名管道（TestPipe）：

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe
    ```

> [!NOTE]
> 設定的 COM 埠未列在 [Hyper-v 管理員] 中的虛擬機器設定中。

## <a name="see-also"></a>另請參閱

- [Hyper-V 上的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [使用 VMConnect 在 Hyper-V 虛擬機器上使用本機資源](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [規劃 Windows Server 2016 中的 Hyper-v 擴充性](./plan-hyper-v-scalability-in-windows-server.md)