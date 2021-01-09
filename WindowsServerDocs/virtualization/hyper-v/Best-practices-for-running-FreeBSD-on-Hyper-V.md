---
title: 在 Hyper-v 上執行 FreeBSD 的最佳作法
description: 提供在虛擬機器上執行 FreeBSD 的建議
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/08/2021
ms.openlocfilehash: 396366e72dcfda60131267299ac7427ad83b05df
ms.sourcegitcommit: 209b0995a11c89bb9ece3db0d48a35d7ba5bbd9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2021
ms.locfileid: "98053651"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>在 Hyper-v 上執行 FreeBSD 的最佳作法

>適用于： Azure Stack HCI、版本 20H2;Windows Server 2019、Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

本主題包含在 Hyper-v 虛擬機器上執行 FreeBSD 做為客體作業系統的建議清單。

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>在 Hyper-v 上啟用 FreeBSD 10.2 中的 CARP

常見的位址冗余通訊協定 (CARP) 可讓多部主機共用相同的 IP 位址和虛擬主機識別碼 (VHID) ，以協助提供一或多個服務的高可用性。 如果有一或多部主機失敗，其他主機會明確地接管，讓使用者不會注意到服務失敗。若要使用 FreeBSD 10.2 中的 CARP，請遵循 [FreeBSD 手冊](https://www.freebsd.org/doc/en/books/handbook/carp.html) 中的指示，並在 hyper-v 管理員中執行下列操作。

* 確認虛擬機器具有網路介面卡，且已獲指派虛擬交換器。 選取虛擬機器，然後選取 [**動作**  >  **設定**]。

![已選取網路介面卡之虛擬機器設定的螢幕擷取畫面](media/Hyper-V_Settings_NetworkAdapter.png)

* 啟用 MAC 位址詐騙。 若要這樣做：

   1. 選取虛擬機器，然後選取 [**動作**  >  **設定**]。

   2. 展開 [ **網路介面卡** ]，然後選取 [ **Advanced Features**]。

   3. 選取 [ **啟用 MAC 位址** 詐騙]。

## <a name="create-labels-for-disk-devices"></a>建立磁片裝置的標籤

在啟動期間，系統會在探索到新裝置時建立裝置節點。 這可能表示新增裝置時，裝置名稱可能會變更。 如果您在啟動時遇到根掛接錯誤，則應該為每個 IDE 磁碟分割建立標籤，以避免衝突和變更。 若要深入瞭解，請參閱 [標記磁片裝置](https://www.freebsd.org/doc/handbook/geom-glabel.html)。 以下是範例。

> [!IMPORTANT]
> 進行任何變更之前，請先製作 fstab 的備份副本。

1. 將系統重新開機為單一使用者模式。 若要完成此動作，請選取 [開機功能表選項 2] （適用于 FreeBSD 10.3 + (選項 4 FreeBSD 8.x) ，或從開機提示字元執行 ' boot-s '）。

2. 在單一使用者模式中，為 fstab 中所列的每個 IDE 磁碟分割建立幾何標籤 (根和交換) 。 以下是 FreeBSD 10.3 的範例。

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   幾何標籤的其他資訊可在下列位置找到： [標記磁片裝置](https://www.freebsd.org/doc/handbook/geom-glabel.html)。

3. 系統將繼續進行多使用者的開機。 開機完成後，請編輯/etc/fstab，並將傳統裝置名稱取代為其各自的標籤。 最終的/etc/fstab 看起來會像這樣：

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. 系統現在可以重新開機。 如果一切順利，它就會正常運作，而且掛接將會顯示：

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>使用無線網路介面卡作為虛擬交換器

如果主機上的虛擬交換器是以無線網路介面卡為基礎，請使用下列命令將 ARP 到期時間縮短為60秒。 否則，VM 的網路可能會在一段時間後停止運作。


```
   # sysctl net.link.ether.inet.max_age=60
```


另請參閱

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
