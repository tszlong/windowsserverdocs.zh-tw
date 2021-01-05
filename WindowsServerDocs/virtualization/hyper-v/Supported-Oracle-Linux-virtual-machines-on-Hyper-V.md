---
title: Hyper-v 上支援的 Oracle Linux 虛擬機器
description: 列出每個版本中所包含的 Oracle Linux integration services 和功能
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
ms.author: benarm
author: BenjaminArmstrong
ms.date: 06/05/2020
ms.openlocfilehash: ce31296712d23aaf30525eeec9a1e8b2c366d84f
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833843"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>Hyper-v 上支援的 Oracle Linux 虛擬機器

>適用于： Windows Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows 10、Windows 8。1

下列功能發佈對應會指出每個版本中的功能。 每個散發的已知問題和因應措施會列在資料表之後。

本節內容：

* [Oracle Linux 8.x 系列](#oracle-linux-8x-series)
* [Oracle Linux 7.x 系列](#oracle-linux-7x-series)
* [Oracle Linux 6.x 系列](#oracle-linux-6x-series)


## <a name="table-legend"></a>資料表圖例

* **內建的內建** 元件包含在此 Linux 發行版本中。 內建的 .LIS (的核心模組版本號碼如 **lsmod** 所示，例如) 與 Microsoft 提供的 .lis 下載套件上的版本號碼不同。 不相符並不表示內建的 .LIS 已過期。

* &#10004;-可用功能
*  (*空白*) -功能無法使用
* **RHCK** -Red Hat 相容核心
* **UEK** -Unbreakable Enterprise KERNEL (UEK) 
   * UEK4-建置於上游 Linux 核心版本4.1.12
   * UEK5-建置於上游 Linux 核心版本4.14
   * UEK6-建置於上游 Linux 核心版本5。4

## <a name="oracle-linux-8x-series"></a>Oracle Linux 8.x 系列

|       **功能**     |       **Windows Server 版本**      |       **8.0-8.1 (RHCK)** |
|-----------------------|---------------------------------------|-------------------|
|       **可用性**        |   |
|       **[核心版](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019、2016、2012 R2 | &#10004; |
|       Windows Server 2016 精確時間       | 2019、2016 | &#10004; |
|       **[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   |
|       大型訊框        | 2019、2016、2012 R2 | &#10004; |
|       VLAN 標記和中繼       | 2019、2016、2012 R2 | &#10004;  |
|       即時移轉      | 2019、2016、2012 R2 | &#10004; |
|       靜態 IP 插入     |  2019、2016、2012 R2 | &#10004; 附注2 |
|       vRSS     | 2019、2016、2012 R2 | &#10004; |
|       TCP 分割和總和檢查碼卸載 | 2019、2016、2012 R2 | &#10004;|
|       SR-IOV  | 2019、2016 |  &#10004;   |
|       **[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  |
|       VHDX 調整大小  | 2019、2016、2012 R2 | &#10004; |
|       虛擬光纖通道 | 2019、2016、2012 R2 | &#10004; 附注3  |
|       即時虛擬機器備份  | 2019、2016、2012 R2 | &#10004; 附注5 |
|       TRIM 支援 | 2019、2016、2012 R2 | &#10004;  |
|       SCSI WWN | 2019、2016、2012 R2 | &#10004;  |
|       **[記憶](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |
|       PAE 核心支援  | 2019、2016、2012 R2 |  N/A |
|       設定 MMIO 間距  | 2019、2016、2012 R2 | &#10004; |
|       動態記憶體-Hot-Add | 2019、2016、2012 R2  | &#10004; 附注7、8、9 |
|       動態記憶體-佔用 | 2019、2016、2012 R2 | &#10004; 附注7、8、9 |
|       執行時間記憶體大小調整 | 2019、2016  | &#10004;  |
|       **[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | |
|       Hyper-v 特定的影片裝置 | 2019、2016、2012 R2 | &#10004;   |
|       **[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | |
|       Key-Value 配對  | 2019、2016、2012 R2 | &#10004;   |
|       非遮罩式插斷 | 2019、2016、2012 R2 | &#10004;  |
|       從主機到來賓的檔案複製 | 2019、2016、2012 R2 | &#10004;  |
|       lsvmbus 命令 | 2019、2016、2012 R2 | &#10004;  |
|       Hyper-v 通訊端 | 2019、2016 | &#10004;  |
|       PCI 傳遞/DDA | 2019、2016 | &#10004; |
| **[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       使用 UEFI 開機 | 2019、2016、2012 R2 |  &#10004; 附注12  |
|       安全開機 | 2019、2016 |  &#10004; |

## <a name="oracle-linux-7x-series"></a>Oracle Linux 7.x 系列

此系列只有64位核心。

<table width="100%">
<tr height="50px">
<td width="20%" rowspan="2">

功能
</td>
<td width="20%" rowspan="2">

Windows Server 版本
</td>
<td width="30%" colspan="3">

7.5-7。8
</td>
<td width="30%" colspan="3">

7.3-7。4
</td>
</tr>
<tr>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK5
</td>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK4
</td>
</tr>
<tr>
<td width="20%">

可用性
</td>
<td width="20%">


</td>
<td width="10%">

.LIS 4。3
</td>
<td width="10%">

內建
</td>
<td width="10%">

內建
</td>
<td width="10%">

.LIS 4。3
</td>
<td width="10%">

內建
</td>
<td width="10%">

內建
</td>
</tr>
<tr height="50px">
<td width="20%">

**[核心版](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Windows Server 2016 精確時間
</td>
<td width="20%">

2019、2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

 **[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

大型訊框
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">
VLAN 標記和中繼
</td>
<td width="20%">
2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

即時移轉
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

靜態 IP 插入
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004; 附注2
</td>
<td width="10%">

&#10004; 附注2
</td>
<td width="10%">

&#10004; 附注2
</td>
<td width="10%">

&#10004; 附注2
</td>
<td width="10%">

&#10004; 附注2
</td>
<td width="10%">

&#10004; 附注2
</td>
</tr>
<tr height="50px">
<td width="20%">

vRSS
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

TCP 分割和總和檢查碼卸載
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

SR-IOV
</td>
<td width="20%">

2019、2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

VHDX 調整大小
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

虛擬光纖通道
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004; 附注3
</td>
<td width="10%">

&#10004; 附注3
</td>
<td width="10%">

&#10004; 附注3
</td>
<td width="10%">

&#10004; 附注3
</td>
<td width="10%">

&#10004; 附注3
</td>
<td width="10%">

&#10004; 附注3
</td>
</tr>
<tr height="50px">
<td width="20%">

即時虛擬機器備份
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004; 附注5
</td>
<td width="10%">

&#10004; 附注4，5
</td>
<td width="10%">

&#10004; 附注5
</td>
<td width="10%">

&#10004; 附注5
</td>
<td width="10%">

&#10004; 附注4，5
</td>
<td width="10%">

&#10004; 附注5
</td>
</tr>
<tr height="50px">
<td width="20%">

TRIM 支援
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

SCSI WWN
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[記憶](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**
</td>
<td width="20%">


</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

PAE 核心支援
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

N/A
</td>
<td width="10%">

N/A
</td>
<td width="10%">

N/A
</td>
<td width="10%">

N/A
</td>
<td width="10%">

N/A
</td>
<td width="10%">

N/A
</td>
</tr>
<tr height="50px">
<td width="20%">

設定 MMIO 間距
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

動態記憶體 Hot-Add
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004; 附注7、8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
</tr>
<tr height="50px">
<td width="20%">

動態記憶體佔用
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004; 附注7、8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
<td width="10%">

&#10004; 附注8、9
</td>
</tr>
<tr height="50px">
<td width="20%">

執行時間記憶體大小調整
</td>
<td width="20%">

2019、2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Hyper-v 特定影片
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

索引鍵/值組
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

非遮罩式插斷
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

從主機到來賓的檔案複製
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

lsvmbus 命令
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Hyper-v 通訊端
</td>
<td width="20%">

2019、2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

PCI 傳遞/DDA
</td>
<td width="20%">

2019、2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

使用 UEFI 開機
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004; 附注12
</td>
<td width="10%">

&#10004; 附注12
</td>
<td width="10%">

&#10004; 附注12
</td>
<td width="10%">

&#10004; 附注12
</td>
<td width="10%">

&#10004; 附注12
</td>
<td width="10%">

&#10004; 附注12
</td>
</tr>
<tr height="50px">
<td width="20%">

安全開機
</td>
<td width="20%">

2019、2016、2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
</table>


## <a name="oracle-linux-6x-series"></a>Oracle Linux 6.x 系列

此系列只有64位核心。

|       **功能**     |       **Windows Server 版本**      |       **6.8-6.10 (RHCK)** |       **6.8-6.10 (UEK4)**     |
|-----------------------|---------------------------------------|-------------------|-------------------|
|       **可用性**     |   | .LIS 4。3  | 內建  |
|       **[核心版](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019、2016、2012 R2 | &#10004; | &#10004;
|       Windows Server 2016 精確時間       | 2019、2016 | |
|       **[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   |  |
|       大型訊框        | 2019、2016、2012 R2 | &#10004; | &#10004;|
|       VLAN 標記和中繼       | 2019、2016、2012 R2 | &#10004; 附注1 | &#10004; 附注1 |
|       即時移轉      | 2019、2016、2012 R2 | &#10004; | &#10004;|
|       靜態 IP 插入     |  2019、2016、2012 R2 | &#10004; 附注2 | &#10004;|
|       vRSS     | 2019、2016、2012 R2 | &#10004; | &#10004;|
|       TCP 分割和總和檢查碼卸載 | 2019、2016、2012 R2 | &#10004;|  &#10004; |
|       SR-IOV  | 2019、2016 |    |  |
|       **[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  |  |
|       VHDX 調整大小  | 2019、2016、2012 R2 | &#10004; | &#10004; |
|       虛擬光纖通道 | 2019、2016、2012 R2 | &#10004; 附注3  | &#10004; 附注3 |
|       即時虛擬機器備份  | 2019、2016、2012 R2 | &#10004; 附注5 | &#10004; 附注5|
|       TRIM 支援 | 2019、2016、2012 R2 | &#10004;  | &#10004; |
|       SCSI WWN | 2019、2016、2012 R2 | &#10004;  | &#10004; |
|       **[記憶](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |  |
|       PAE 核心支援  | 2019、2016、2012 R2 |  N/A | N/A
|       設定 MMIO 間距  | 2019、2016、2012 R2 | &#10004; | &#10004;  |
|       動態記憶體-Hot-Add | 2019、2016、2012 R2  | &#10004; 附注6、8、9 | &#10004; 附注6、8、9 |
|       動態記憶體-佔用 | 2019、2016、2012 R2 | &#10004; 附注6、8、9 | &#10004; 附注6、8、9 |
|       執行時間記憶體大小調整 | 2019、2016  |  | |
|       **[影片](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | | |
|       Hyper-v 特定的影片裝置 | 2019、2016、2012 R2 | &#10004;   | &#10004; |
|       **[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | | |
|       Key-Value 配對  | 2019、2016、2012 R2 | &#10004; 附注10，11   | &#10004; 附注10，11  |
|       非遮罩式插斷 | 2019、2016、2012 R2 | &#10004;  | &#10004; |
|       從主機到來賓的檔案複製 | 2019、2016、2012 R2 | &#10004;  | &#10004; |
|       lsvmbus 命令 | 2019、2016、2012 R2 | &#10004;  | &#10004; |
|       Hyper-v 通訊端 | 2019、2016 | &#10004;  | &#10004; |
|       PCI 傳遞/DDA | 2019、2016 | &#10004; | &#10004; |
| **[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       使用 UEFI 開機 | 2019、2016、2012 R2 |  &#10004; 附注12  | &#10004; 附注12
|       安全開機 | 2019、2016 |  |  |



## <a name="notes"></a><a name="BKMK_notes"></a>附註

1. 在此 Oracle Linux 版本中，VLAN 標記可正常運作，但 VLAN 中繼則否。

2. 如果已針對虛擬機器上指定的綜合網路介面卡設定網路系統管理員，則靜態 IP 插入可能無法運作。 若要讓靜態 IP 插入順利運作，請確定網路系統管理員已完全關閉，或已透過其 >ifcfg-eth0 ethX 檔關閉特定網路介面卡的功能。

3.  在使用虛擬光纖通道裝置的 Windows Server 2012 R2 上，請確定已填入邏輯單元編號 0 (LUN 0) 。 如果 LUN 0 尚未填入，Linux 虛擬機器可能無法以原生方式掛接光纖通道裝置。

4. 針對內建的 .LIS，必須安裝 "hyperv-守護程式" 套件才能進行這項功能。

5.  如果在即時虛擬機器備份作業期間有開啟的檔案控制代碼，則在某些角落的情況下，備份的 Vhd 可能必須在還原時 (fsck) 進行檔案系統一致性檢查。 如果虛擬機器具有連結的 iSCSI 裝置或直接連接的存放裝置 (也稱為傳遞磁片) ，即時備份作業可能會以無訊息方式失敗。

6. 動態記憶體支援僅適用于64位的虛擬機器。

7. 在此散發套件中，預設不會啟用 Hot-Add 支援。 若要啟用 Hot-Add 支援，您需要在/etc/udev/rules.d/底下新增 udev 規則，如下所示：

   1. 建立檔案 **/etc/udev/rules.d/100-balloon.rules**。 您可以針對檔案使用任何其他想要的名稱。

   2. 將下列內容新增至檔案： `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. 重新開機系統以啟用 Hot-Add 支援。

   雖然 Linux Integration Services 下載會在安裝時建立此規則，但卸載 IIS 時也會移除規則，因此，如果在卸載後需要動態記憶體，就必須重新建立規則。

8. 如果客體作業系統的記憶體不足，則動態記憶體作業可能會失敗。 以下是一些最佳作法：

   * 啟動記憶體和最小記憶體應等於或大於散發廠商建議的記憶體數量。

   * 通常會耗用系統上的整個可用記憶體的應用程式，只能耗用高達80% 的可用 RAM。

9. 如果您在 Windows Server 2016 或 Windows Server 2012 R2 作業系統上使用動態記憶體，請指定 **啟動記憶體**、 **最小記憶體**，以及 **最大記憶體** 參數（以 128 mb (MB) 的倍數表示）。 若未這麼做，可能會導致熱新增失敗，而且您可能不會在客體作業系統中看到任何記憶體增加。

10. 若要啟用機碼/值組 (KVP) 基礎結構，請從 Oracle Linux ISO 安裝 hypervkvpd 或 hyperv-守護程式 rpm 套件。 或者，您可以直接從 Oracle Linux Yum 存放庫安裝套件。

11. KVP) 基礎結構的索引鍵/值組 (可能無法在沒有 Linux 軟體更新的情況下正常運作。 如果您看到此功能有問題，請洽詢您的散發廠商以取得軟體更新。

12. 在 Windows Server 2012 R2 第2代虛擬機器上，預設會啟用安全開機，除非停用安全開機選項，否則部分 Linux 虛擬機器將不會開機。 您可以在 **Hyper-v 管理員** 的虛擬機器設定的 [**固件**] 區段中停用安全開機，也可以使用 Powershell 來停用它：

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

    Linux Integration Services 下載可套用至現有的第2代 Vm，但不會傳授第2代的功能。


另請參閱

* [設定-Get-vmfirmware](/powershell/module/hyper-v/set-vmfirmware)

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳作法](Best-Practices-for-running-Linux-on-Hyper-V.md)
