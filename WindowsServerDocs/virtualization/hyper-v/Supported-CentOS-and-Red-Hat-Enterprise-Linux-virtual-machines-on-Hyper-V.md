---
title: 支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器
description: 列出的新版 Linux integration services 支援的 CentOS 和 Red Hat Enterprise 散發套件
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4bf8783d-dee5-4b3e-8cce-2b11b117c189
author: danihalfin
ms.author: daniha
ms.date: 12/20/2017
ms.openlocfilehash: 6c9ed85c2249a4671e52eb7d512298a75f53b309
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222677"
---
# <a name="supported-centos-and-red-hat-enterprise-linux-virtual-machines-on-hyper-v"></a>支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

下列功能發佈對應會指出有內建和可下載的版本，Linux Integration services 中的功能。 資料表之後，會列出已知的問題和因應措施的每個散發。

HYPER-V （可自 Red Hat Enterprise Linux 6.4） 的內建 Red Hat Enterprise Linux 整合服務驅動程式會使用高效能綜合裝置在 HYPER-V 主機上執行 Red Hat Enterprise Linux 來賓足夠。Red Hat 認證這些內建的驅動程式為此用途。 經認證的設定可檢視此 Red Hat web 網頁上：[Red Hat 認證目錄](https://access.redhat.com/ecosystem/search/#/ecosystem/Red%20Hat%20Enterprise%20Linux?sort=sortTitle%20asc&vendors=Microsoft&category=Server)。 不需要下載並安裝 Linux Integration Services 封裝，從 Microsoft 下載中心取得，並執行因此可能會限制您的 Red Hat 支援 Red Hat 知識庫文件 1067年中所述：[Red Hat 知識庫 1067年](https://access.redhat.com/articles/1067)。

因為內建的 LIS 支援和可下載的 LIS 支援之間的潛在衝突而當您升級核心時，停用自動更新、 解除安裝 LIS 可下載的套件、 更新核心、 重新開機，然後再安裝發行最新的 LIS，並重新啟動一次。

>[!NOTE]
>正式的 Red Hat Enterprise Linux 認證資訊是透過[Red Hat 客戶入口網站](https://access.redhat.com/ecosystem/search/#/category/Server?sort=sortTitle%20asc&query=windows%20server&ecosystem=Red%20Hat%20Enterprise%20Linux)。

本節內容：

* [RHEL/CentOS 7.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_7x)

* [RHEL/CentOS 6.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_6x)

* [RHEL/CentOS 5.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_5x)

* [附註](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>表格圖例

* **內建**-LIS 是此 Linux 散發套件的一部分。 核心模組的版本號碼的內建的 LIS (如所示**lsmod**，例如) 上的 Microsoft 所提供的 LIS 下載套件的版本號碼不同。 不相符並不代表內建的 LIS 已過期。

* &#10004;-功能

* (*空白*)-功能無法使用

## <a name="BKMK_7x"></a>RHEL/CentOS 7.x Series

這一系列只有 64 位元核心。

|**功能**|**Windows Server 版本**|**7.5-7.6**|**7.3-7.4**|**7.0-7.2**|**7.5-7.6**|**7.4**|**7.3**|**7.2**|**7.1**|**7.0**|
|-|-|-|-|-|-|-|-|-|-|-|
|**可用性**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|內建|內建|內建|內建|內建|內建||
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 正確時間|2019, 2016|&#10004;|&#10004;|||||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|大型訊框|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和主幹連線|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|即時移轉|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2019、 2016、 2012 R2，2012年|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|TCP 分割和總和檢查碼卸載|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;|||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||
|VHDX 調整大小|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|虛擬光纖通道|2019、 2016、 2012 R2|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|
|即時虛擬機器備份|2019、 2016、 2012 R2|&#10004;請注意 5|&#10004;請注意 5|&#10004;請注意 5|&#10004;附註 4 5|&#10004;附註 4 5|&#10004;附註 4 5|&#10004;附註 4 5|&#10004;附註 4 5|&#10004;附註 4 5|
|修剪支援|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||
|PAE 核心支援|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|MMIO 間距的組態|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2019、 2016、 2012 R2，2012年|&#10004;請注意 8、 9、 10|&#10004;請注意 8、 9、 10|&#10004;請注意 8、 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 8、 9、 10|
|動態記憶體-佔用|2019、 2016、 2012 R2，2012年|&#10004;請注意 8、 9、 10|&#10004;請注意 8、 9、 10|&#10004;請注意 8、 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 9、 10|&#10004;請注意 8、 9、 10|
|執行階段記憶體大小調整|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||
|Hyper-v 特定視訊裝置|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||
|索引鍵 / 值組|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|非遮罩式插斷|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到客體檔案複製|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|||||||
|HYPER-V 通訊端|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|PCI 通過/DDA|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;||||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||
|使用 UEFI 開機|2019、 2016、 2012 R2|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14|
|安全開機|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="BKMK_6x"></a>RHEL/CentOS 6.x Series

這一系列的 32 位元核心是啟用 PAE。 沒有內建 LIS 支援 RHEL/centos 6.0-6.3。

|**功能**|**Windows Server 版本**|**6.4-6.10**|**6.0-6.3**|**6.10, 6.9, 6.8**|**6.6, 6.7**|**6.5**|**6.4**|
|-|-|-|-|-|-|-|-|
|**可用性**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|內建|內建|內建|內建|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 正確時間|2019, 2016||||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|大型訊框|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 標記和主幹連線|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|
|即時移轉|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2019、 2016、 2012 R2，2012年|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|TCP 分割和總和檢查碼卸載|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|SR-IOV|2019, 2016|||||||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|VHDX 調整大小|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|虛擬光纖通道|2019、 2016、 2012 R2|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3|&#10004;附註 3||
|即時虛擬機器備份|2019、 2016、 2012 R2|&#10004;請注意 5|&#10004;請注意 5|&#10004;附註 4 5|&#10004;附註 4 5|&#10004;請注意 4、 5、 6|&#10004;請注意 4、 5、 6|
|修剪支援|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;||||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|PAE 核心支援|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|MMIO 間距的組態|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2019、 2016、 2012 R2，2012年|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 8、 9、 10|&#10004;請注意 7、 8、 9、 10||
|動態記憶體-佔用|2019、 2016、 2012 R2，2012年|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 9、 10|&#10004;請注意 7、 9、 10、 11|
|執行階段記憶體大小調整|2019, 2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Hyper-v 特定視訊裝置|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|索引鍵 / 值組|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;請注意 12|&#10004;請注意 12|&#10004;請注意 12、 13|&#10004;請注意 12、 13|
|非遮罩式插斷|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|從主機到客體檔案複製|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|lsvmbus 命令|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;||||||
|HYPER-V 通訊端|2019, 2016|&#10004;|&#10004;|||||
|PCI 通過/DDA|2019, 2016|||||||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|使用 UEFI 開機|2012 R2|||||||
||2019, 2016|&#10004;請注意 14|&#10004;請注意 14|&#10004;請注意 14||||
|安全開機|2019, 2016||||||

## <a name="BKMK_5x"></a>RHEL/CentOS 5.x Series

這一系列有可支援的 32 位元 PAE 核心。 5.9 之前沒有 RHEL/CentOS 的 LIS 內建支援。

|**功能**|**Windows Server 版本**|5.2 -5.11|**5.2-5.11**|**5.9 - 5.11**|
|-|-|-|-|-|
|**可用性**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612)|內建|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 正確時間|2019, 2016||||
|**[網路功能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|大型訊框|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|VLAN 標記和主幹連線|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;附註 1|&#10004;附註 1|&#10004;附註 1|
|即時移轉|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|靜態 IP 資料隱碼攻擊|2019、 2016、 2012 R2，2012年|&#10004;附註 2|&#10004;附註 2|&#10004;附註 2|
|vRSS|2019、 2016、 2012 R2||||
|TCP 分割和總和檢查碼卸載|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;||
|SR-IOV|2019, 2016||||||
|**[儲存體](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|VHDX 調整大小|2019、 2016、 2012 R2|&#10004;|&#10004;||
|虛擬光纖通道|2019、 2016、 2012 R2|&#10004;附註 3|&#10004;附註 3||
|即時虛擬機器備份|2019、 2016、 2012 R2|&#10004;請注意 5、 15|&#10004;請注意 5|&#10004;請注意 4、 5、 6|
|修剪支援|2019、 2016、 2012 R2||||
|SCSI WWN|2019、 2016、 2012 R2||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|PAE 核心支援|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;|&#10004;|
|MMIO 間距的組態|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|
|動態記憶體的熱新增|2019、 2016、 2012 R2，2012年||||
|動態記憶體-佔用|2019、 2016、 2012 R2，2012年|&#10004;請注意 7、 9、 10、 11|&#10004;請注意 7、 9、 10、 11||
|執行階段記憶體大小調整|2019, 2016||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Hyper-v 特定視訊裝置|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;||
|**[其他](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|索引鍵 / 值組|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2|&#10004;|&#10004;||
|非遮罩式插斷|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|
|從主機到客體檔案複製|2019、 2016、 2012 R2|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、WINDOWS SERVER 2012、WINDOWS 2008 R2||||
|HYPER-V 通訊端|2019, 2016||||
|PCI 通過/DDA|2019, 2016||||||
|**[第 2 代虛擬機器](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|使用 UEFI 開機|2019、 2016、 2012 R2||||
|安全開機|2019, 2016||||

## <a name="BKMK_notes"></a>附註

1. 這個 RHEL/CentOS 發行，VLAN 標記運作，但 VLAN 主幹連線則否。

2. 如果指定的綜合網路介面卡的虛擬機器上已設定網路管理員，靜態 IP 資料隱碼攻擊可能無法運作。 針對靜態 IP 能夠順利運作資料隱碼請確定已關閉完全 「 網路管理員，或已關閉的特定網路介面卡透過其 ifcfg ethX 檔案。

3. 在 Windows Server 2012 R2 使用虛擬光纖通道裝置時，請確定邏輯單元編號 (LUN 0) 0 已填入。 如果尚未擴展 LUN 0，Linux 虛擬機器可能無法以原生方式掛接光纖通道裝置。

4. 內建的 LIS，"hyperv 精靈 」 的套件必須安裝這項功能。

5. 如果有開啟檔案控制代碼在上線的虛擬機器備份作業時，則某些邊角案例中，備份 Vhd 可能必須進行還原的檔案系統一致性檢查 (fsck)。 如果虛擬機器有附加的 iSCSI 裝置或直接附加儲存體 （也稱為傳遞磁碟），即時的備份作業可能會以無訊息模式失敗。

6. 慣用的 Linux Integration Services 下載時，即時備份支援的 RHEL/CentOS 5.9-5.11/6.4/6.5 也是可透過[適用於 Linux 的 HYPER-V 備份 Essentials](https://github.com/LIS/backupessentials/tree/1.0)。

7. 只有在 64 位元虛擬機器上使用動態記憶體支援。

8. 根據預設，此分佈在未啟用熱新增的支援。 若要啟用熱新增的支援，您需要新增下 /etc/udev/rules.d/ udev 規則，如下所示：

   1. 建立檔案 **/etc/udev/rules.d/100-balloon.rules**。 您可以使用任何其他所需要的檔案名稱。

   2. 將下列內容加入檔案： `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. 若要啟用熱新增的支援系統重新開機。

   在 Linux Integration Services 下載安裝建立此規則，規則也會移除時解除安裝 LIS，因此必須重新建立規則，如果解除安裝之後需要動態記憶體。

9. 如果客體作業系統上的記憶體執行太低，動態記憶體作業可能會失敗。 以下是一些最佳作法：

   * 啟動記憶體和最少的記憶體應該等於或大於發佈廠商建議的記憶體數量。

   * 應用程式，會消耗整個系統上可用的記憶體僅限於耗用最多 80%的可用的 RAM。

10. 如果您使用動態記憶體在 Windows Server 2016 或 Windows Server 2012 R2 作業系統上，指定**啟動記憶體**，**最小記憶體**，並**最大記憶體**128 mb 的倍數中的參數。 熱新增失敗，可能會導致失敗，若要這樣做，您可能不會看到任何客體作業系統中增加的記憶體。

11. 某些散發套件，包括那些使用 LIS 4.0 和 4.1 只佔用支援並不會提供熱新增的支援。 在此案例中，將啟動記憶體參數設定為等於最大記憶體參數的值可以使用 「 動態記憶體 」 功能。 這會導致在經常性存取層加入至虛擬機器開機時間，然後再根據主應用程式的記憶體需求的所有必要記憶體，HYPER-V 可以自由地配置或釋出客體使用佔用的記憶體。 請設定**啟動記憶體**並**最小記憶體**等於或高於所發佈的建議值。

12. 若要啟用索引鍵/值組 (KVP) 基礎結構，請從 RHEL ISO 安裝 hypervkvpd 或 hyperv 精靈 rpm 封裝。 或者可以直接從 RHEL 儲存機制安裝封裝。

13. 沒有 Linux 軟體更新，索引鍵/值組 (KVP) 基礎結構可能會無法正確運作。 請連絡您的發佈廠商，以取得軟體更新，萬一您看到這項功能的問題。

14. 在 Windows Server 2012 R2 第 2 代虛擬機器已預設啟用安全開機，某些 Linux 虛擬機器將無法開機，除非已停用安全開機選項。 您可以停用安全開機**韌體**中的虛擬機器的設定 區段**HYPER-V 管理員**也可以使用 Powershell 加以停用：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

   Linux Integration Services 下載可以套用至現有第 2 代 Vm，但未傳達第 2 代功能。

15. 在 Red Hat Enterprise Linux 或 CentOS 5.2、 5.3，和 5.4 檔案系統凍結功能不適，因此即時虛擬機器備份也無法使用。

另請參閱

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Red Hat Hardware 認證](https://hardware.redhat.com/&quicksearch=Microsoft)
