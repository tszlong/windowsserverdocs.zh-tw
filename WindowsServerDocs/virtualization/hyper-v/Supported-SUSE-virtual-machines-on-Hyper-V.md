---
title: 在 HYPER-V 上支援的 SUSE 虛擬機器
description: 列出的 Linux integration services 和每個版本中所包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 3506c00651951aa2a62637cae6cc4989f9edf1fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818999"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>在 HYPER-V 上支援的 SUSE 虛擬機器

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

以下是功能發佈地圖，指出每個版本的功能。 表格後面列出已知的問題和因應措施的每個散發。

SUSE 認證適用於 HYPER-V 的內建 SUSE Linux Enterprise Service 驅動程式。 本公告中可檢視的範例組態：[SUSE 是憑證公告](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176)。

## <a name="table-legend"></a>表格圖例

* **內建**-LIS 是此 Linux 散發套件的一部分。Microsoft 所提供的 LIS 下載套件不適用於此發佈，所以不會安裝它。核心模組的版本號碼的內建的 LIS (如所示**lsmod**，例如) 上的 Microsoft 所提供的 LIS 下載套件的版本號碼不同。 不相符並不表示內建的 LIS 已過期。

* &#10004;-功能

* (*空白*)-功能無法使用

SLES12 + 只有 64 位元。

|**功能**|**Windows Server 作業系統版本**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|內建|內建|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 正確時間|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||||
|大型訊框|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和主幹連線|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2019、 2016、 2012 R2，2012年|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|TCP 分割和總和檢查碼卸載|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||||||
|VHDX 調整大小|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|虛擬光纖通道|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時虛擬機器備份|2019、 2016、 2012 R2|&#10004;請注意 2、 3、 8|&#10004;請注意 2、 3、 8|&#10004;請注意 2、 3、 8|&#10004;請注意 2、 3、 8|&#10004;請注意 2、 3、 8|&#10004;請注意 2、 3、 8|
|修剪支援|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||||
|PAE 核心支援|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|N/A|N/A|N/A|N/A|&#10004;|&#10004;|
|MMIO 間距的組態|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2019、 2016、 2012 R2，2012年|&#10004;請注意 5、 6|&#10004;請注意 5、 6|&#10004;請注意 5、 6|&#10004;請注意 5、 6|&#10004;請注意 4、 5、 6|&#10004;請注意 4、 5、 6|
|動態記憶體-佔用|2019、 2016、 2012 R2，2012年|&#10004;請注意 5、 6|&#10004;請注意 5、 6|&#10004;請注意 5、 6|&#10004;請注意 5、 6|&#10004;請注意 4、 5、 6|&#10004;請注意 4、 5、 6|
|執行階段記憶體大小調整|2019, 2016|&#10004;請注意 5、 6|&#10004;請注意 5、 6|&#10004;請注意 5、 6||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||||
|Hyper-v 特定視訊裝置|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||||
|索引鍵/值組|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;請注意 7|&#10004;請注意 7|
|非遮罩式插斷|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到客體檔案複製|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;||||
|HYPER-V 通訊端|2019, 2016|&#10004;|&#10004;|||||
|PCI 通過/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||||
|使用 UEFI 開機|2019、 2016、 2012 R2|&#10004;請注意 9|&#10004;請注意 9|&#10004;請注意 9|&#10004;請注意 9|&#10004;請注意 9||
|安全開機|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>附註

1. 如果靜態 IP 資料隱碼攻擊可能無法運作**網路管理員**已針對指定的 Hyper-v 特定網路介面卡，虛擬機器上設定。 若要確保能夠順利運作的靜態 IP 資料隱碼請確定網路管理員完全關閉，或已關閉特定的網路介面卡透過其**ifcfg ethX**檔案。

2. 如果有開啟檔案控制代碼在上線的虛擬機器備份作業時，則某些邊角案例中，備份 Vhd 可能必須進行還原的檔案系統一致性檢查 (fsck)。

3. 如果虛擬機器有附加的 iSCSI 裝置或直接附加儲存體 （也稱為傳遞磁碟），即時的備份作業可能會以無訊息模式失敗。

4. 如果客體作業系統上的記憶體執行太低，動態記憶體作業可能會失敗。 以下是一些最佳作法：

   * 啟動記憶體和最少的記憶體應該等於或大於發佈廠商建議的記憶體數量。

   * 應用程式，會消耗整個系統上可用的記憶體僅限於耗用最多 80%的可用的 RAM。

5. 只有在 64 位元虛擬機器上使用動態記憶體支援。

6. 如果您使用動態記憶體在 Windows Server 2016 或 Windows Server 2012 作業系統上，指定**啟動記憶體**，**最小記憶體**，並**最大記憶體**128 mb 的倍數中的參數。 熱新增失敗，可能會導致失敗，若要這樣做，您可能不會看到任何客體作業系統中增加的記憶體。

7. 在 Windows Server 2016 或 Windows Server 2012 R2 中，索引鍵/值組基礎結構可能會無法正確運作而不需要 Linux 軟體更新。 請連絡您的發佈廠商，以取得軟體更新，萬一您看到這項功能的問題。

8. 如果單一分割區會掛接多次，則 VSS 備份會失敗。

9. 在 Windows Server 2012 R2，除非已停用安全開機選項，不會開機第 2 代虛擬機器有預設值而且層代 2 的 Linux 虛擬機器已啟用安全開機。 您可以在 [Hyper-V 管理員] 虛擬機器設定的 [韌體]  區段停用安全開機，或是使用 Powershell 停用安全開機：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>另請參閱

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
