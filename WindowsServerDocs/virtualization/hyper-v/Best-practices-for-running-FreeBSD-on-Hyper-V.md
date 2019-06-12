---
title: 在 HYPER-V 上執行 FreeBSD 的最佳做法
description: 提供虛擬機器上執行 FreeBSD 的建議
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 6320ceb86093146592a54ab34b013f334f43ddb4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447770"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>在 HYPER-V 上執行 FreeBSD 的最佳做法

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

本主題包含的建議做為客體作業系統中執行 FreeBSD 的 HYPER-V 虛擬機器上的清單。

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>啟用在 HYPER-V 上的 FreeBSD 10.2 CARP

常見的地址備援通訊協定 (CARP) 可讓多部主機共用相同的 IP 位址和一個或多個服務提供高可用性的虛擬主應用程式識別碼 (VHID)。 如果一個或多部主機失敗，其他主機無障礙地接管讓使用者將不會注意到服務失敗。若要使用 FreeBSD 10.2 CARP，請依照下列中的指示[FreeBSD 手冊](https://www.freebsd.org/doc/en/books/handbook/carp.html)執行 HYPER-V 管理員中的下列一項動作。

* 請確認虛擬機器網路介面卡，而且已指派的虛擬交換器。 選取的虛擬機器，然後選取**動作** > **設定**。

![與選取的網路介面卡的虛擬機器設定的螢幕擷取畫面](media/Hyper-V_Settings_NetworkAdapter.png)

* 啟用 MAC 位址詐騙。 若要這樣做，

   1. 選取的虛擬機器，然後選取**動作** > **設定**。

   2. 依序展開**網路介面卡**，然後選取**進階功能**。

   3. 選取 **啟用 MAC 位址詐騙**。

## <a name="create-labels-for-disk-devices"></a>建立的磁碟裝置的標籤

在啟動期間，在發現新的裝置，會建立裝置節點。 這可能表示，當加入新的裝置時，可以變更裝置名稱。 如果您在啟動期間收到根掛接錯誤，您應該建立每個 IDE 磁碟分割，以避免衝突與變更的標籤。 若要深入了解，請參閱[標記的磁碟裝置](https://www.freebsd.org/doc/handbook/geom-glabel.html)。 以下是範例。 

> [!IMPORTANT]
> 請您 fstab 的備份副本，再進行任何變更。

1. 系統重新開機進入單一使用者模式。 這可藉由選取適用於 FreeBSD 10.3 + 開機功能表選項 2 (適用於 FreeBSD 選項 4 8.x)，或從開機提示字元中執行 '開機-s'。

2. 在單一使用者模式中，請為每個您 fstab （根和交換） 中所列的 IDE 磁碟分割建立 GEOM 標籤。 以下是範例中的 FreeBSD 10.3。

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   在就可以找到 GEOM 標籤上的其他資訊：[標記磁碟裝置](https://www.freebsd.org/doc/handbook/geom-glabel.html)。

3. 系統將會繼續多使用者開機。 開機完成之後，編輯 /etc/fstab，並取代傳統的裝置名稱，其各自的標籤。 最終的 /etc/fstab 看起來像這樣：

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. 此外，系統現在可以重新開機。 如果一切順利，它會啟動通常會顯示掛接：

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>使用無線網路介面卡與虛擬交換器

如果主機上的虛擬交換器為基礎的無線網路介面卡，減少為 60 秒 ARP 到期時間，下列命令。 否則 VM 的網路功能可能會在一段時間之後停止運作。


```
   # sysctl net.link.ether.inet.max_age=60
```


另請參閱

* [在 HYPER-V 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
