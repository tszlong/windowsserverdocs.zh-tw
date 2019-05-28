---
title: 在 HYPER-V 上支援的 FreeBSD 虛擬機器
description: 列出的 Linux integration services 和每個版本中所包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 08/30/2017
ms.openlocfilehash: a398334700f7c292732207919b73a33145a6aae9
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222697"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>在 HYPER-V 上支援的 FreeBSD 虛擬機器

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

下列功能發佈對應會指出每個版本的功能。 表格後面列出已知的問題和因應措施的每個散發。

## <a name="table-legend"></a>表格圖例

* **內建**-BIS （FreeBSD 整合服務） 是此 FreeBSD 版本的一部分。

* &#10004;-功能

* (*空白*)-功能無法使用

|**功能**|**Windows Server 作業系統版本**|**11.1/11.2**|**11.0**|**10.3**|**10.2**|**10.0 - 10.1**|**9.1 - 9.3, 8.4**|
|-|-|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|內建|[連接埠](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Windows Server 2016 正確時間|2016|&#10004;||||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|大型訊框|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|
|VLAN 標記和主幹連線|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2016、 2012 R2，2012年|&#10004;附註 4|&#10004;附註 4|&#10004;附註 4|&#10004;附註 4|&#10004;附註 4|&#10004;|
|vRSS|2016、 2012 R2|&#10004;|&#10004;|||||
|TCP 分割和總和檢查碼卸載|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|大型接收卸載 (LRO)|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2016|||||||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||附註 1|附註 1|附註 1|附註 1|附註 1 2|附註 1 2|
|VHDX 調整大小|2016、 2012 R2|&#10004;請注意 7|&#10004;請注意 7|||||
|虛擬光纖通道|2016、 2012 R2|||||||
|即時虛擬機器備份|2016、 2012 R2|&#10004;||||||
|修剪支援|2016、 2012 R2|&#10004;||||||
|SCSI WWN|2016、 2012 R2|||||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|PAE 核心支援|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|||||||
|MMIO 間距的組態|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2016、 2012 R2，2012年|||||||
|動態記憶體-佔用|2016、 2012 R2，2012年|||||||
|執行階段記憶體大小調整|2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|HYPER-V 特定視訊裝置|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|||||||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|索引鍵/值組|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;請注意 6|&#10004;請注意 5、 6|&#10004;請注意 6|
|非遮罩式插斷|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到客體檔案複製|2016、 2012 R2|||||||
|lsvmbus 命令|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|||||||
|HYPER-V 通訊端|2016|||||||
|PCI 通過/DDA|2016|&#10004;||||||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|使用 UEFI 開機|2016、 2012 R2|&#10004;||||||
|安全開機|2016|||||||

## <a name="BKMK_notes"></a>附註

1. 若要建議[標籤的磁碟裝置]( https://www.freebsd.org/doc/handbook/geom-glabel.html)以避免在啟動期間的根目錄的掛接錯誤。

2. BIS 驅動程式以 FreeBSD 在載入時，可能無法辨識的虛擬 DVD 光碟機 8.x 和 9.x 除非啟用舊版的 ATA 驅動程式，透過下列命令。
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 是支援的 MTU 大小的上限。

4. 在容錯移轉案例中，您無法設定複本伺服器中的靜態 IPv6 位址。 改為使用 IPv4 位址。

5. KVP 係由 FreeBSD 10.0 上的連接埠。 請參閱[FreeBSD 10.0 的連接埠](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/)上 FreeBSD.org 如需詳細資訊。

6. KVP 可能無法在 Windows Server 2008 R2 上。

7. 若要 VHDX 線上調整大小的運作正常的 FreeBSD 11.0 中，特殊的手動步驟才可解決 GEOM 錯誤，且主機會調整大小的 VHDX 磁碟之後，在 11.0 + 中已修正-開啟磁碟寫入，然後執行"gpart recover"如下所示。
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
**其他注意事項**:10 穩定且 11 穩定的功能矩陣是 FreeBSD 11.1 版本相同。 此外，FreeBSD 10.2 與舊版 (10.1、 10.0，9.x、 8.x) 是生命週期結束。 請參閱[此處](https://security.freebsd.org/)的最新支援的版本與最新的資訊安全摘要報告的清單。

**其他注意事項**:10 穩定且 11 穩定的功能矩陣是 FreeBSD 11.1 版本相同。 此外，FreeBSD 10.2 與舊版 (10.1、 10.0，9.x、 8.x) 是生命週期結束。 請參閱[此處](https://security.freebsd.org/)的最新支援的版本與最新的資訊安全摘要報告的清單。

## <a name="see-also"></a>另請參閱

* [在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [在 HYPER-V 上執行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
