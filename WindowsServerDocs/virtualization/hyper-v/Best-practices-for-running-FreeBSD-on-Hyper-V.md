---
title: 在 Hyper-v 上執行 FreeBSD 的最佳做法
description: 提供在虛擬機器上執行 FreeBSD 的建議
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 1d284b38e1bdb642aa40ecbb8e82caa7712f7aad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365626"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>在 Hyper-v 上執行 FreeBSD 的最佳做法

>適用於：Windows Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

本主題包含在 Hyper-v 虛擬機器上執行 FreeBSD 做為客體作業系統的建議清單。

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>在 Hyper-v 上啟用 FreeBSD 10.2 中的 CARP

通用位址冗余通訊協定（CARP）可讓多部主機共用相同的 IP 位址和虛擬主機識別碼（VHID），以協助為一或多個服務提供高可用性。 如果有一或多部主機失敗，其他主機會明確地接管，讓使用者不會注意到服務失敗。若要在 FreeBSD 10.2 中使用 CARP，請遵循[FreeBSD 手冊](https://www.freebsd.org/doc/en/books/handbook/carp.html)中的指示，並在 hyper-v 管理員中執行下列動作。

* 確認虛擬機器具有網路介面卡，並已指派虛擬交換器。 選取虛擬機器，然後選取 [**動作**] [ > **設定**]。

![已選取網路介面卡之虛擬機器設定的螢幕擷取畫面](media/Hyper-V_Settings_NetworkAdapter.png)

* 啟用 MAC 位址詐騙。 若要這麼做，

   1. 選取虛擬機器，然後選取 [**動作**] [ > **設定**]。

   2. 展開 [**網路介面卡**]，然後選取 [ **Advanced Features**]。

   3. 選取 [**啟用 MAC 位址**詐騙]。

## <a name="create-labels-for-disk-devices"></a>建立磁片裝置標籤

在啟動期間，系統會在探索到新裝置時建立裝置節點。 這可能表示新增裝置時，裝置名稱可能會變更。 如果您在啟動時收到根載入錯誤，您應該為每個 IDE 分割區建立標籤，以避免發生衝突和變更。 若要瞭解作法，請參閱為[磁片裝置加上標籤](https://www.freebsd.org/doc/handbook/geom-glabel.html)。 以下是範例。 

> [!IMPORTANT]
> 在進行任何變更之前，請先建立 fstab 的備份複本。

1. 將系統重新開機為單一使用者模式。 若要完成這項作業，請選取 [FreeBSD 10.3 +] 的 [開機功能表選項 2] （FreeBSD 8.x 的選項4），或從開機提示執行 [開機-s]。

2. 在 [單一使用者模式] 中，為 fstab 中所列的每個 IDE 磁碟分割建立幾何標籤（[根] 和 [交換]）。 以下是 FreeBSD 10.3 的範例。

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   您可以在下列位置找到幾何標籤的其他資訊：[標籤磁片裝置](https://www.freebsd.org/doc/handbook/geom-glabel.html)。

3. 系統會繼續進行多使用者開機。 開機完成後，請編輯/etc/fstab，並將傳統的裝置名稱取代為其各自的標籤。 最終的/etc/fstab 看起來會像這樣：

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. 系統現在可以重新開機。 如果一切順利，它會正常運作，而且掛接會顯示：

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>使用無線網路介面卡作為虛擬交換器

如果主機上的虛擬交換器是以無線網路介面卡為基礎，請使用下列命令，將 ARP 到期時間縮減為60秒。 否則，VM 的網路功能可能會在一段時間後停止運作。


```
   # sysctl net.link.ether.inet.max_age=60
```


另請參閱

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
