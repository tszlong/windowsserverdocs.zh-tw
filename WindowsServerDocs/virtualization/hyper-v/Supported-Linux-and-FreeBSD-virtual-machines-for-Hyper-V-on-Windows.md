---
title: Windows 上 HYPER-V 的受支援的 Linux 和 FreeBSD 虛擬機器
description: 列出的 Linux integration services 和每個版本中所包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 9df495bdc67b06a675fec050fb4c2960337ce8ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832899"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Windows 上 HYPER-V 的受支援的 Linux 和 FreeBSD 虛擬機器

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

HYPER-V 支援的 Linux 和 FreeBSD 虛擬機器的模擬和 Hyper-v 特定裝置。 執行模擬的裝置，沒有其他的軟體時，需要安裝。 不過模擬的裝置不會提供高效能，而且無法利用 HYPER-V 技術提供豐富的虛擬機器管理基礎結構。 若要充分利用 HYPER-V 提供的所有優點，最好是使用適用於 Linux 和 FreeBSD 的 Hyper-v 特定裝置。 執行 Hyper-v 特定裝置所需的驅動程式的集合稱為 Linux Integration Services (LIS) 或 FreeBSD Integration Services (BIS)。

LIS 已新增至 Linux 核心，而且會更新為新版本。 但較舊核心為基礎的 Linux 散發套件可能沒有最新的增強功能或修正。 Microsoft 提供下載，其中包含的某些 Linux 安裝，這些較舊的核心為基礎的可安裝 LIS 驅動程式。 由於發佈廠商包括新版的 Linux Integration Services，最好是安裝最新的 LIS，可下載版本，如果適用的話，您的安裝。

針對其他 Linux 散發套件 LIS 變更會定期整合到應用程式與作業系統核心因此不需要任何個別下載或安裝。

針對較舊的 FreeBSD 版本 （之前 10.0)，Microsoft 會提供包含可安裝 BIS 驅動程式和對應的精靈，FreeBSD 虛擬機器的連接埠。 針對較新的 FreeBSD 版本，BIS 內建 FreeBSD 作業系統中，因此不需要個別下載或安裝需要 KVP 連接埠下載所需的 FreeBSD 10.0 除外。

> [!TIP]
> - 下載[Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)從評估中心。
> - 下載[Microsoft HYPER-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016)從評估中心。

此內容的目標是提供可協助加速您的 Linux 或 FreeBSD 部署在 HYPER-V 上的資訊。 特定的詳細資料包括：

* Linux 散發套件或需要的下載和安裝 LIS 或 BIS 驅動程式的 FreeBSD 版本。

* Linux 散發套件或包含內建的 LIS 或 BIS 驅動程式的 FreeBSD 版本。

* 表示主要 Linux 散發套件或 FreeBSD 版本中的功能的功能散發對應。

* 已知的問題和因應措施為每個散發或發行。

* 針對每個 LIS 或 BIS 功能的功能描述。

**想要提出建議的相關特性與功能嗎？** 有任何需要我們如何改進？ 您可以使用[Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support)網站建議適用於 Linux 和 FreeBSD 虛擬機器在 HYPER-V 上的新特色與功能，並看看其他人的意見。

## <a name="in-this-section"></a>本節內容

* [支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [在 HYPER-V 上執行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
