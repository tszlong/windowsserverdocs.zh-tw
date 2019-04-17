---
title: 改善效能的檔案伺服器與 SMB 直接傳輸
description: 說明 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的 SMB 直接傳輸功能。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ed8fd5b4114fc9fd9c7dc278a98cea8cc67a8749
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239225"
---
# SMB 直接傳輸

>適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 包含稱為 「 SMB 直接傳輸，可支援使用的遠端直接記憶體存取 (RDMA) 功能的網路介面卡的功能。 以最快速且延遲很低，RDMA 的網路介面卡可以使用非常少 CPU 時運作。 對於例如 HYPER-V 或 Microsoft SQL Server 的工作負載，這可讓遠端檔案伺服器以類似於本機存放區。 SMB 直接傳輸包含：

- 提高的輸送量： 運用高速網路的網路介面卡位置座標的大量的資料列的速度傳輸的完整輸送量。
- 低延遲： 提供非常快速網路要求的回應，並且如此一來，可讓遠端檔案儲存空間，感覺如同它是直接附加的區塊存放裝置。
- 低 CPU 使用率： 使用較少 CPU 循環時傳輸資料透過網路，離開更多的電源伺服器應用程式可以使用。

SMB 直接傳輸是自動設定的 Windows Server 2012 R2 和 Windows Server 2012。

## SMB 多重通道和 SMB 直接傳輸

SMB 多重通道是負責偵測網路介面卡的 RDMA 功能，以啟用 SMB 直接傳輸的功能。 沒有 SMB 多重通道，SMB，請使用支援 RDMA 的網路介面卡 （所有網路介面卡所都提供的 TCP/IP 堆疊，以及在新的 RDMA 堆疊） 一般 TCP/IP。

SMB 多重通道，與 SMB 偵測是否網路介面卡 RDMA 功能，然後建立多個 RDMA 連線該單一工作階段 （每個介面二）。 這可讓 SMB 使用高輸送量、 低延遲與低支援 RDMA 的網路介面卡所提供的 CPU 使用率。 如果您使用多個 RDMA 介面也可以提供容錯。

>[!NOTE]
>如果您想要使用的網路介面卡的 RDMA 功能，您不應該小組支援 RDMA 的網路介面卡。 當組合在一起，網路介面卡將不支援 RDMA。
>至少一個 RDMA 網路連線建立之後，將不再使用用於原始的通訊協定交涉的 TCP/IP 連線。 不過，以防 RDMA 網路連線失敗，會保留 TCP/IP 連線。

## 需求

SMB 直接傳輸需要下列項目：

- 至少兩個電腦執行 Windows Server 2012 R2 或 Windows Server 2012
- 一或多個網路介面卡具有 RDMA 功能。

### 使用 SMB 直接傳輸時的考量

- 您可以使用 SMB 直接傳輸中容錯移轉叢集。不過，您需要確定使用用戶端存取的叢集網路有足夠的 SMB 直接傳輸。 容錯移轉叢集支援使用多個網路的用戶端存取，以及網路介面卡，是 RSS （接收端調整）-能夠和 RDMA 功能。
- 您可以 SMB 直接傳輸上使用 HYPER-V 管理作業系統來支援使用 HYPER-V 透過 SMB，並提供使用 HYPER-V 儲存堆疊的虛擬機器的存放裝置。 不過，支援 RDMA 的網路介面卡不是直接公開給 HYPER-V 用戶端。 如果您支援 RDMA 的網路介面卡連接到虛擬交換器時，不會支援 RDMA 的虛擬網路介面卡，從切換。
- 如果您停用 SMB 多重通道，SMB 直接傳輸也會停用。 SMB 多重通道偵測網路介面卡功能，並判斷是否支援 RDMA 的網路介面卡，因為 SMB 直接傳輸如果無法使用用戶端 SMB 多重通道已停用。
- SMB 直接傳輸上不支援 Windows RT SMB 直接傳輸需要支援支援 RDMA 的網路介面卡，這是僅適用於 Windows Server 2012 R2 和 Windows Server 2012。
- SMB 直接傳輸不支援向下層級版本的 Windows Server 上。 它只能在 Windows Server 2012 R2 和 Windows Server 2012 上支援。

## 啟用和停用 SMB 直接傳輸

SMB 直接傳輸預設為啟用時已安裝 Windows Server 2012 R2 或 Windows Server 2012。 SMB 用戶端會自動偵測，並使用多個網路連線，如果找出有適當的設定。

### 停用 SMB Direct

一般而言，您不需要停用 SMB 直接傳輸，不過，您可以停用它執行其中一項下列 Windows PowerShell 指令碼。

若要停用 RDMA，特定的介面，請輸入：

```PowerShell
Disable-NetAdapterRdma <name>
```

若要停用 RDMA，所有介面，請輸入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

當您要在用戶端或伺服器停用 RDMA 時，系統不能使用它。 *網路直接存取*是 Windows Server 2012 R2 和 Windows Server 2012 基本網路支援 RDMA 介面的內部名稱。

### 重新啟用 SMB 直接存取

停用 RDMA 之後, 您可以重新啟用它藉由執行下列 Windows PowerShell 指令碼之一。

若要重新啟用 RDMA 的特定的介面，請輸入：

```PowerShell
Enable-NetAdapterRDMA <name>
```

若要重新啟用 RDMA 所有介面，請輸入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

您需要啟用 RDMA 用戶端和伺服器開始再次使用它。

## 測試的 SMB 直接傳輸的效能

您可以測試效能使用其中一個下列程序的運作方式。

### 比較檔案複本，而不需使用 SMB 直接傳輸

以下是如何測量 SMB 直接傳輸的提高的輸送量：

1. 設定 SMB 直接存取
2. 測量執行使用 SMB 直接傳輸大型檔案複本的時間的量。
3. 停用 RDMA 網路介面卡上，請參閱[啟用和停用 SMB 直接傳輸](#enabling-and-disabling-smb-direct)。
4. 測量執行而不需使用 SMB 直接傳輸大型檔案複本的時間的量。
5. 重新啟用 RDMA 的網路介面卡，並再比較兩個結果。
6. 若要避免快取的影響，您應該執行下列動作：
    1. 複製大量的資料 （比記憶體能夠處理的詳細資料）。
    2. 做為做法，並再計時第二個複本的第一個複本兩次，複製的資料。
    3. 重新啟動伺服器和用戶端之前每個測試以確定它們類似的情況下運作。

### 在檔案複本與 SMB 直接傳輸期間失敗的多個網路介面卡的其中一個

以下是如何確認 SMB 直接傳輸的容錯移轉功能：

1. 請確定該 SMB 直接傳輸運作多個網路介面卡設定中。
2. 執行大型的檔案複本。 雖然執行複製時，模擬失敗時的網路路徑的其中一個中斷纜線的其中一個 （或停用其中一個網路介面卡）。
3. 確認該檔案複製會繼續使用其中一個剩餘的網路介面卡，並沒有檔案複製錯誤。

>[!NOTE]
>若要避免工作負載，在不使用 SMB 直接傳輸的失敗，請確定沒有任何其他使用中斷連線的網路路徑的工作負載。

## 更多資訊

- [伺服器訊息區概觀](file-server-smb-overview.md)
- [增加伺服器、 儲存和網路可用性： 案例概觀](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署透過 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
