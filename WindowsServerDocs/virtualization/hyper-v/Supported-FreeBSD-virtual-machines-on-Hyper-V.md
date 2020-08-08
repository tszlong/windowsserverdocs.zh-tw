---
title: Hyper-v 上支援的 FreeBSD 虛擬機器
description: 列出每個版本中包含的 Linux 整合服務和功能
manager: dongill
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 04/07/2020
ms.openlocfilehash: 3767c56640dd4e4e07e2cdd4a578ec0c3db2f470
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965635"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Hyper-v 上支援的 FreeBSD 虛擬機器

>適用于： Windows Server 2019、Hyper-v Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows 10、Windows 8。1

下列功能發佈對應會指出每個版本的功能。 每個散發套件的已知問題和因應措施會列在表格之後。

## <a name="table-legend"></a>資料表圖例

* 此 FreeBSD 版本包含**內建**的 BIS (FreeBSD 整合服務) 。

* &#10004; 功能可供使用

*  (*空白*) -不提供功能

|**功能**|**Windows Server 作業系統版本**|**12-12。1**|**11.1-11。3**|**11.0**|**10.3**|**10.2**|**10.0-10。1**|**9.1-9.3、8。4**|
|-|-|-|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|內建|內建|[連接埠](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[核心](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 精確時間|2019、2016|&#10004;|&#10004;||||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|大型訊框|2019、2016、2012 R2|&#10004; 附注3|&#10004; 附注3|&#10004; 附注3|&#10004; 附注3|&#10004; 附注3|&#10004; 附注3|&#10004; 附注3|
|VLAN 標記和中繼|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 插入|2019、2016、2012 R2|&#10004; 附注4|&#10004; 附注4|&#10004; 附注4|&#10004; 附注4|&#10004; 附注4|&#10004; 附注4|&#10004;|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|||||
|TCP 分割和總和檢查碼卸載|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|大型接收卸載 (LRO) |2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;|||||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||Note1|附註 1|附註 1|附註 1|附註 1|附注1、2|附注1、2|
|VHDX 調整大小|2019、2016、2012 R2|&#10004; 附注6|&#10004; 附注6|&#10004; 附注6|||||
|虛擬光纖通道|2019、2016、2012 R2||||||||
|即時虛擬機器備份|2019、2016、2012 R2|&#10004;|&#10004;||||||
|修剪支援|2019、2016、2012 R2|&#10004;|&#10004;||||||
|SCSI WWN|2019、2016、2012 R2||||||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||||||
|PAE 核心支援|2019、2016、2012 R2||||||||
|設定 MMIO 間隙|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體-熱新增|2019、2016、2012 R2||||||||
|動態記憶體-佔用|2019、2016、2012 R2||||||||
|執行時間記憶體大小調整|2019、2016||||||||
|**[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||||
|Hyper-v 特定的影片裝置|2019、2016、2012 R2||||||||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||||||
|索引鍵/值組|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; 附注5|&#10004;|
|非遮罩式插斷|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到來賓的檔案複製|2019、2016、2012 R2||||||||
|lsvmbus 命令|2019、2016、2012 R2||||||||
|Hyper-v 通訊端|2019、2016||||||||
|PCI 通過/DDA|2019、2016|&#10004;|&#10004;||||||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||||||
|使用 UEFI 開機|2019、2016、2012 R2|&#10004;|&#10004;||||||
|安全開機|2019、2016||||||||

## <a name="notes"></a><a name="BKMK_notes"></a>附註

1. 建議您為[磁片裝置加上標籤]( https://www.freebsd.org/doc/handbook/geom-glabel.html)，以避免在啟動期間發生根掛接錯誤。

2. 在 FreeBSD 8.x 和2.x 上載入 BIS 驅動程式時，除非您透過下列命令啟用舊版 ATA 驅動程式，否則可能無法辨識虛擬 DVD 光碟機。
    ```sh
    # echo ‘hw.ata.disk_enable=1' >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126是支援的最大 MTU 大小。

4. 在容錯移轉案例中，您無法在複本伺服器中設定靜態 IPv6 位址。 請改用 IPv4 位址。

5. KVP 是由 FreeBSD 10.0 上的埠提供。 如需詳細資訊，請參閱 FreeBSD.org 上的[FreeBSD 10.0 埠](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/)。

6. 若要讓 VHDX 線上調整大小在 FreeBSD 11.0 中正常運作，需要特殊的手動步驟來解決在 11.0 + 中已修正的幾何錯誤：在主機調整 VHDX 磁片大小時，請開啟磁片進行寫入，然後執行如下所示的「gpart 復原」。
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```

**其他注意事項**：10穩定和11穩定的功能對照表與 FreeBSD 11.1 版本相同。 此外，FreeBSD 10.2 和舊版 (10.1、10.0、9. x、8.x) 都是生命週期結束。 如需最新的支援版本清單和最新的資訊安全摘要報告，請參閱[這裡](https://security.freebsd.org/)。

## <a name="see-also"></a>另請參閱

* [Hyper-v 上 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [在 Hyper-v 上執行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
