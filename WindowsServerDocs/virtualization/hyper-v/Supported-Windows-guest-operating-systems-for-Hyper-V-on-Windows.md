---
title: Windows Server 上的 Hyper-v 支援的 Windows 客體作業系統
description: 列出支援在虛擬機器中做為來賓使用的 Windows 作業系統。 也提供舊版 Hyper-v 的類似文章連結。
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/08/2019
ms.openlocfilehash: 2e5cf6c94d0127a283c640a48aaa2fced5472e9d
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746723"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-v 支援的 Windows 客體作業系統

>適用於：Windows Server 2016、Windows Server 2019

Hyper-v 支援以虛擬機器形式在虛擬機器中執行的數種 Windows Server、Windows 和 Linux 發行版本，作為客體作業系統。 本文涵蓋支援的 Windows Server 和 Windows 客體作業系統。 若為 Linux 和 FreeBSD 散發套件，請參閱 [Windows 上適用于 hyper-v 的支援 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。

某些作業系統有內建的整合服務。 當您在虛擬機器中設定作業系統之後，其他人需要您以個別的步驟安裝或升級 integration services。 如需詳細資訊，請參閱下列各節，並  [Integration Services](/virtualization/hyper-v-on-windows/reference/integration-services)。

## <a name="supported-windows-server-guest-operating-systems"></a>支援的 Windows Server 客體作業系統

以下是 Windows server 2016 和 Windows Server 2019 中支援作為 Hyper-v 之客體作業系統的 Windows Server 版本。

|客體作業系統 (伺服器)|虛擬處理器數量的上限|Integration Services|備註|
|-------------------------------------|----------------------------------------|------------------------|---------|
|Windows Server 版本 1909 |適用于層代2的 240;<br>適用于層代1的64|內建|大於240的虛擬處理器支援需要 Windows Server、1903版或更新版本的客體作業系統。|
|Windows Server 版本 1903 |適用于層代2的 240;<br>適用于層代1的64|內建||
|Windows Server，版本 1809 |適用于層代2的 240;<br>適用于層代1的64|內建||
|Windows Server 2019 |適用于層代2的 240;<br>適用于層代1的64|內建||
|Windows Server 1803 版 |適用于層代2的 240;<br>適用于層代1的64|內建||
|Windows Server 2016 |適用于層代2的 240;<br>適用于層代1的64|內建||
|Windows Server 2012 R2 |64|內建||
|Windows Server 2012 |64|內建||
|Windows Server 2008 R2 含 Service Pack 1 (SP 1)|64|設定客體作業系統之後，請安裝所有重要的 Windows 更新。|Datercenter、Enterprise、Standard 以及 Web 版本。|
|Windows Server 2008 Service Pack 2 (SP2)|8|設定客體作業系統之後，請安裝所有重要的 Windows 更新。|Datacenter、Enterprise、Standard 以及 Web 版本 (32 位元與 64 位元)。|

## <a name="supported-windows-client-guest-operating-systems"></a>支援的 Windows 用戶端客體作業系統

以下是 Windows Server 2016 和 Windows Server 2019 中支援作為 Hyper-v 之客體作業系統的 Windows 用戶端版本。

|客體作業系統 (用戶端)|虛擬處理器數量的上限|Integration Services|備註|
|-------------------------------------|----------------------------------------|------------------------|---------|
|Windows 10|32|內建||
|Windows 8。1|32|內建||
|Windows 7 含 Service Pack 1 (SP 1)|4|在設定客體作業系統之後，請升級 integration services。|旗艦版、企業版和專業版版本 (32 位元與 64 位元)。|

## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>其他 Windows 版本上的客體作業系統支援

下表提供在其他 Windows 版本上支援 Hyper-v 之客體作業系統的相關資訊連結。

|主機作業系統|主題|
|-------------------------|---------|
|Windows 10|[Windows 10 中的用戶端 Hyper-v 支援的客體作業系統](/virtualization/hyper-v-on-windows/about/supported-guest-os)|
|Windows Server 2012 R2 和 Windows 8.1|-   [Windows Server 2012 R2 和 Windows 8.1 中 Hyper-v 支援的 Windows 客體作業系統](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Hyper-v 上的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|
|Windows Server 2012 和 Windows 8|[Windows Server 2012 與 Windows 8 中 Hyper-V 所支援的 Windows 客體作業系統](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|
|Windows Server 2008 和 Windows Server 2008 R2|[關於虛擬機器和客體作業系統](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|

## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Microsoft 如何提供客體作業系統的支援

Microsoft 透過下列方式提供客體作業系統的支援：

-   Microsoft 作業系統和整合服務中發現的問題可獲得 Microsoft 支援服務的支援。

-   經過作業系統廠商認證可在 Hyper-V 執行的其他作業系統上發現的問題，由廠商提供支援。

-   若為在其他作業系統中發現的問題，Microsoft 會將問題提交給 [TSANet](https://www.tsanet.org/)，其為多個廠商支援的社群。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 上的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

-   [Windows 10 中的用戶端 Hyper-v 支援的客體作業系統](/virtualization/hyper-v-on-windows/about/supported-guest-os)