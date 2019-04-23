---
title: 在 HYPER-V 上支援的 Oracle Linux 虛擬機器
description: 列出的 Linux integration services 和每個版本中所包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/01/2017
ms.openlocfilehash: a1569a30c0c5657de14c6df936b3b4596dceb7e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838699"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>在 HYPER-V 上支援的 Oracle Linux 虛擬機器

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

下列功能發佈對應指示會出現在每個版本的功能。 表格後面列出已知的問題和因應措施的每個散發。

本節內容：

* [Red Hat 相容核心系列](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_rhc)

* [Unbreakable Enterprise Kernel 系列](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_uek)

* [附註](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>表格圖例

* **內建**-LIS 是此 Linux 散發套件的一部分。 核心模組的版本號碼的內建的 LIS (如所示**lsmod**，例如) 上的 Microsoft 所提供的 LIS 下載套件的版本號碼不同。 不相符並不表示內建的 LIS 已過期。

* &#10004;-功能

* (*空白*)-功能無法使用

* **UEK R\*x QU\*y** -Unbreakable Enterprise Kernel (UEK) 位置*x*是版本號碼並*y*是每季更新。

## <a name="BKMK_rhc"></a>Red Hat 相容核心系列

32 位元核心 6.x 系列是啟用 PAE。 沒有內建 LIS 支援的 Oracle Linux RHCK 6.0-6.3。 Oracle Linux 7.x 核心可以僅表示 64 位元。

| **功能**                                                                                                              | **Windows server 版本**   | **7.5-7.6**         | **7.4**             | **6.4 6.8 和 7.0 7.3**                                             | **6.4 6.8 和 7.0 7.2**                                             | **RHCK 7.0-7.2**         | **RHCK 6.8**             | **RHCK 6.6、 6.7**        | **RHCK 6.5**              | **RHCK6.4**               |
|--------------------------------------------------------------------------------------------------------------------------|------------------------------|---------------------|---------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------|--------------------------|--------------------------|---------------------------|---------------------------|
| **可用性**                                                                                                         |                              | 內建            | 內建            | [LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612) | 內建                 | 內建                 | 內建                 | 內建                  | 內建                  |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**                          | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Windows Server 2016 正確時間                                                                                        | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**              |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| 大型訊框                                                                                                             | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| VLAN 標記和主幹連線                                                                                                | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 | &#10004;            | &#10004;            | &#10004;（如 6.4 6.8 註 1）                                       | &#10004;（如 6.4 6.8 註 1）                                       | &#10004;                 | &#10004;附註 1          | &#10004;附註 1          | &#10004;附註 1           | &#10004;附註 1           |
| 即時移轉                                                                                                           | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| 靜態 IP 資料隱碼攻擊                                                                                                      | 2016、 2012 R2，2012年          | &#10004;請注意 14    | &#10004;請注意 14    | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| vRSS                                                                                                                     | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| TCP 分割和總和檢查碼卸載                                                                                   | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| SR-IOV                                                                                                                   | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**                    |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| VHDX 調整大小                                                                                                              | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| 虛擬光纖通道                                                                                                    | 2016、 2012 R2                | &#10004;附註 2     | &#10004;附註 2     | &#10004;附註 2                                                     | &#10004;附註 2                                                     | &#10004;附註 2          | &#10004;附註 2          | &#10004;附註 2          | &#10004;附註 2           |                           |
| 即時虛擬機器備份                                                                                              | 2016、 2012 R2                | &#10004;請注意 11,3  | &#10004;請注意第 11 3 | &#10004;附註 3 4                                                  | &#10004;附註 3 4                                                  | &#10004;請注意 3、 4、 11   | &#10004;請注意 3、 4、 11   | &#10004;請注意 3、 4、 11   | &#10004;請注意 3、 4、 5、 11 | &#10004;請注意 3、 4、 5、 11 |
| 修剪支援                                                                                                             | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 |                          |                           |                           |
| SCSI WWN                                                                                                                 | 2016、 2012 R2                | &#10004;            |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| **[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**                      |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| PAE 核心支援                                                                                                       | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 | N/A                 | N/A                 | &#10004;(只有 6.x)                                                 | &#10004;(只有 6.x)                                                 | N/A                      | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| MMIO 間距的組態                                                                                                | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| 動態記憶體的熱新增                                                                                                 | 2016、 2012 R2，2012年          | &#10004;請注意 8 9  | &#10004;請注意 8 9  | &#10004;請注意 7、 8、 9、 10 （請注意 6.4 6.7 6）                      | &#10004;請注意 7、 8、 9、 10 （請注意 6.4 6.7 6）                      | &#10004;請注意 6、 7、 8、 9 | &#10004;請注意 6、 7、 8、 9 | &#10004;請注意 6、 7、 8、 9 | &#10004;請注意 6、 7、 8、 9  |                           |
| 動態記憶體-佔用                                                                                              | 2016、 2012 R2，2012年          | &#10004;請注意 8 9  | &#10004;請注意 8 9  | &#10004;請注意 7、 9、 10 （請注意 6.4 6.7 6）                         | &#10004;請注意 7、 9、 10 （請注意 6.4 6.7 6）                         | &#10004;請注意 6、 8、 9    | &#10004;請注意 6、 8、 9    | &#10004;請注意 6、 8、 9    | &#10004;請注意 6、 8、 9     | &#10004;請注意 6、 8、 9、 10 |
| 執行階段記憶體大小調整                                                                                                    | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**                        |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Hyper-v 特定視訊裝置                                                                                            | 2016,2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2  | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| **[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**                 |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| 索引鍵 / 值組                                                                                                           | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;請注意 12         | &#10004;請注意 12         | &#10004;請注意 12         | &#10004;請注意 12          | &#10004;請注意 12          |
| 非遮罩式插斷                                                                                                   | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| 從主機到客體檔案複製                                                                                             | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            |                          | &#10004;                 |                          |                           |                           |
| lsvmbus 命令                                                                                                          | 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2 |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| HYPER-V 通訊端                                                                                                          | 2016                         |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| PCI 通過/DDA                                                                                                      | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)** |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| 使用 UEFI 開機                                                                                                          | 2016、 2012 R2                | &#10004;請注意 13    | &#10004;請注意 13    | &#10004;請注意 13                                                    | &#10004;請注意 13                                                    | &#10004;請注意 13         | &#10004;請注意 13         |                          |                           |                           |
| 安全開機                                                                                                              | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |


## <a name="BKMK_uek"></a>Unbreakable Enterprise Kernel 系列

Oracle Linux Unbreakable Enterprise Kernel (UEK) 只有 64 位元，並有支援內建的 LIS。

|**功能**|**Windows server 版本**|**UEK R4**|**UEK R3 QU3**|**UEK R3 QU2**|**UEK R3 QU1**|
|-|-|-|-|-|-|
|**可用性**||內建|內建|內建|內建|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 正確時間|2016|||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||
|大型訊框|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和主幹連線|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2016、 2012 R2，2012年|&#10004;|&#10004;|&#10004;||
|vRSS|2016、 2012 R2|&#10004;||||
|TCP 分割和總和檢查碼卸載|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;||||
|SR-IOV|2016|||||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||||
|VHDX 調整大小|2016、 2012 R2|&#10004;|&#10004;|&#10004;||
|虛擬光纖通道|2016、 2012 R2|&#10004;|&#10004;|&#10004;||
|即時虛擬機器備份|2016、 2012 R2|&#10004;請注意 3、 4、 5、 11|&#10004;請注意 3、 4、 5、 11|&#10004;請注意 3、 4、 5、 11||
|修剪支援|2016、 2012 R2|&#10004;||||
|SCSI WWN|2016、 2012 R2|||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|PAE 核心支援|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|N/A|N/A|N/A|N/A|
|MMIO 間距的組態|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2016、 2012 R2，2012年|&#10004;||||
|動態記憶體-佔用|2016、 2012 R2，2012年|&#10004;||||
|執行階段記憶體大小調整|2016|||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||
|Hyper-v 特定視訊裝置|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||
|索引鍵 / 值組|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;請注意 11、 12|&#10004;請注意 11、 12|&#10004;請注意 11、 12|&#10004;請注意 11、 12|
|非遮罩式插斷|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到客體檔案複製|2016、 2012 R2|&#10004;請注意 11|&#10004;請注意 11|&#10004;請注意 11|&#10004;請注意 11|
|lsvmbus 命令|2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|||||
|HYPER-V 通訊端|2016|||||
|PCI 通過/DDA|2016|||||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|使用 UEFI 開機|2016、 2012 R2|&#10004;||||
|安全開機|2016|&#10004;||||

## <a name="BKMK_notes"></a>附註

1. Oracle Linux 版本中，VLAN 標記有作用但 VLAN 主幹連線則否。

2. 在使用虛擬光纖通道裝置時，請確定邏輯單元編號 (LUN 0) 0 已填入。 如果尚未擴展 LUN 0，Linux 虛擬機器可能無法以原生方式掛接光纖通道裝置。

3. 如果有開啟檔案控制代碼在上線的虛擬機器備份作業時，則某些邊角案例中，備份 Vhd 可能必須進行還原的檔案系統一致性檢查 (fsck)。

4. 如果虛擬機器有附加的 iSCSI 裝置或直接附加儲存體 （也稱為傳遞磁碟），即時的備份作業可能會以無訊息模式失敗。

5. 即時備份支援的 Oracle Linux 6.4/6.5/UEKR3 QU2 和 QU3 是可透過[適用於 Linux 的 HYPER-V 備份 Essentials](https://github.com/LIS/backupessentials/tree/1.0)。

6. 只有在 64 位元虛擬機器上使用動態記憶體支援。

7. 根據預設，此分佈在未啟用熱新增的支援。 若要啟用熱新增的支援，您需要新增下 /etc/udev/rules.d/ udev 規則，如下所示：

   1. 建立檔案 **/etc/udev/rules.d/100-balloon.rules**。 您可以使用任何其他所需要的檔案名稱。

   2. 將下列內容加入檔案： `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. 若要啟用熱新增的支援系統重新開機。

   在 Linux Integration Services 下載安裝建立此規則，規則也會移除時解除安裝 LIS，因此必須重新建立規則，如果解除安裝之後需要動態記憶體。

8. 如果客體作業系統上的記憶體執行太低，動態記憶體作業可能會失敗。 以下是一些最佳作法：

   * 啟動記憶體和最少的記憶體應該等於或大於發佈廠商建議的記憶體數量。

   * 應用程式，會消耗整個系統上可用的記憶體僅限於耗用最多 80%的可用的 RAM。

9. 如果您使用動態記憶體在 Windows Server 2016 或 Windows Server 2012 R2 作業系統上，指定**啟動記憶體**，**最小記憶體**，並**最大記憶體**128 mb 的倍數中的參數。 熱新增失敗，可能會導致失敗，若要這樣做，您可能不會看到任何客體作業系統中增加的記憶體。

10. 某些散發套件，包括使用 LIS 3.5 或 LIS 4.0 中，只佔用支援並不會提供熱新增的支援。 在此案例中，將啟動記憶體參數設定為等於最大記憶體參數的值可以使用 「 動態記憶體 」 功能。 這會導致在經常性存取層加入至虛擬機器開機時間，然後再根據主應用程式的記憶體需求的所有必要記憶體，HYPER-V 可以自由地配置或釋出客體使用佔用的記憶體。 請設定**啟動記憶體**並**最小記憶體**等於或高於所發佈的建議值。

11. 根據預設，不會安裝 oracle Linux HYPER-V 精靈。 若要使用這些精靈，安裝 hyperv 精靈封裝。 此封裝 je v konfliktu 下載 Linux Integration Services，並不應該安裝在已下載的 LIS 的系統上。

12. 沒有 Linux 軟體更新，索引鍵/值組 (KVP) 基礎結構可能會無法正確運作。 請連絡您的發佈廠商，以取得軟體更新，萬一您看到這項功能的問題。

13. 自 Windows Server 2012 R2Generation 2 虛擬機器已預設啟用安全開機和某些 Linux 虛擬機器將無法開機，除非已停用安全開機選項。 您可以停用安全開機**韌體**中的虛擬機器的設定 區段**HYPER-V 管理員**也可以使用 Powershell 加以停用：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

    Linux Integration Services 下載可以套用至現有第 2 代 Vm，但未傳達第 2 代功能。

14. 如果指定的綜合網路介面卡的虛擬機器上已設定網路管理員，靜態 IP 資料隱碼攻擊可能無法運作。 針對靜態 IP 能夠順利運作資料隱碼請確定已關閉完全 「 網路管理員，或已關閉的特定網路介面卡透過其 ifcfg ethX 檔案。


另請參閱

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
