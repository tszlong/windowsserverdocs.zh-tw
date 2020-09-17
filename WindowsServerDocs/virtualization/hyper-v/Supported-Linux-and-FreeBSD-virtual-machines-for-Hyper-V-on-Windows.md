---
title: Windows 上適用于 Hyper-v 的支援 Linux 和 FreeBSD 虛擬機器
description: 列出每個版本中所包含的 Linux integration services 和功能
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/03/2016
ms.openlocfilehash: 891ad97d8ae5ef01c6dbfd0d59f7be6316c6e687
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746743"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Windows 上適用于 Hyper-v 的支援 Linux 和 FreeBSD 虛擬機器

>適用于： Windows Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

Hyper-v 支援適用于 Linux 和 FreeBSD 虛擬機器的模擬和 Hyper-v 專用裝置。 使用模擬裝置執行時，不需要安裝其他軟體。 不過，模擬裝置不提供高效能，而且無法利用 Hyper-v 技術所提供的豐富虛擬機器管理基礎結構。 為了充分利用 Hyper-v 提供的所有優點，最好使用適用于 Linux 和 FreeBSD 的 Hyper-v 專用裝置。 執行 Hyper-v 專用裝置所需的驅動程式集合稱為 Linux Integration Services (.LIS) 或 FreeBSD Integration Services (BIS) 。

已將 .LIS 版新增至 Linux 核心，並已更新為新版本。 但以較舊的核心為基礎的 Linux 發行版本可能沒有最新的增強功能或修正。 Microsoft 針對以這些較舊的核心為基礎的部分 Linux 安裝提供可安裝的 .LIS 驅動程式下載。 由於散發廠商包含 Linux Integration Services 版本，因此最好安裝適用于您安裝的最新可下載版本（如果適用）。

針對其他 Linux 散發套件，您可以定期將這些變更整合到作業系統核心和應用程式中，因此不需要個別的下載或安裝。

針對在 10.0) 之前 (舊版 FreeBSD，Microsoft 提供的埠包含可安裝的 BIS 驅動程式，以及適用于 FreeBSD 虛擬機器的對應守護程式。 針對較新的 FreeBSD 版本，BIS 內建于 FreeBSD 作業系統，而且除了 FreeBSD 10.0 所需的 KVP 埠下載之外，不需要個別的下載或安裝。

> [!TIP]
> - 從評估中心下載 [Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) 。

此內容的目標是要提供可協助您在 Hyper-v 上部署 Linux 或 FreeBSD 的資訊。 特定詳細資料包含：

* 需要下載和安裝 .LIS 或 BIS 驅動程式的 Linux 發行版本或 FreeBSD 版本。

* Linux 發行版本或 FreeBSD 版本，包含內建的 .LIS 或 BIS 驅動程式。

* 代表主要 Linux 發行版本或 FreeBSD 版中功能的功能發佈對應。

* 每個散發或發行的已知問題和因應措施。

* 每個 .LIS 或 BIS 功能的功能描述。

**想要提出有關特性和功能的建議嗎？** 有什麼東西可以做得更好嗎？ 您可以使用 [Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support) 網站來建議 hyper-v 上的 Linux 和 FreeBSD 虛擬機器的新特性和功能，並查看其他人所說的內容。

## <a name="in-this-section"></a>本節內容

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虛擬機器的功能描述](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳作法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [在 Hyper-v 上執行 FreeBSD 的最佳作法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
