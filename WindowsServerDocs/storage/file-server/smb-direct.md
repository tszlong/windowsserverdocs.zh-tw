---
title: 使用 SMB 直接傳輸改善檔案伺服器的效能
description: 說明 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 直接傳輸功能。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 41126aa0d054607449d57928c1777679e5087e73
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394464"
---
# <a name="smb-direct"></a>SMB 直接傳輸

>適用於：Windows Server 2012 R2、Windows Server 2012、Windows Server 2016

Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 都包含一項叫做 SMB 直接傳輸的功能，這項功能支援使用具備遠端直接記憶體存取 (RDMA) 功能的網路介面卡。 具備 RDMA 功能的網路介面卡可以在使用非常少量 CPU 資源的情況下，以非常低的延遲全速運作。 對於像是 Hyper-V 或 Microsoft SQL Server 的工作負載，這讓遠端檔案伺服器能夠類似本機存放裝置。 SMB 直接傳輸包括：

- 增加輸送量：利用高速網路的全部輸送量，其中網路介面卡會以線路速度協調大量資料的傳輸。
- 低延遲：對網路要求提供非常快速的回應，因此讓遠端檔案存放裝置感覺好像是直接連接的區塊存放裝置一樣。
- 低 CPU 使用率：透過網路傳輸資料時，使用較少的 CPU 週期，這會將較多的運算能力留給伺服器應用程式使用。

SMB 直接傳輸是由 Windows Server 2012 R2 和 Windows Server 2012 自動設定。

## <a name="smb-multichannel-and-smb-direct"></a>SMB 多重通道和 SMB 直接傳輸

SMB 多重通道的功能是負責偵測網路介面卡的 RDMA 功能，以啟用 SMB 直接傳輸功能。 在沒有 SMB 多重通道的情況下，SMB 會使用一般 TCP/IP 搭配具備 RDMA 功能的網路介面卡 (所有網路介面卡皆會隨著新的 RDMA 堆疊提供一個 TCP/IP 堆疊)。

在有 SMB 多重通道的情況下，SMB 會偵測網路介面卡是否具備 RDMA 功能，然後針對該單一工作階段建立多個 RDMA 連線 (每一個介面兩個連線)。 這會讓 SMB 能夠享有具備 RDMA 功能之網路介面卡提供的高輸送量、低延遲及低 CPU 使用率。 如果您使用多個 RDMA 介面，則它也提供容錯的功能。

>[!NOTE]
>如果您打算使用網路介面卡的 RDMA 功能，請勿將具備 RDMA 功能的網路介面卡組成小組。 網路介面卡在組成小組時，將不支援 RDMA。
>至少建立一個 RDMA 網路連線之後，就不會再使用原本通訊協定交涉的 TCP/IP 連線。 不過，還是會保留 TCP/IP 連線，以便應付發生 RDMA 網路連線失敗的狀況。

## <a name="requirements"></a>需求

SMB 直接傳輸具有下列需求：

- 至少有兩部執行 Windows Server 2012 R2 或 Windows Server 2012 的電腦
- 一或多個具備 RDMA 功能的網路介面卡。

### <a name="considerations-when-using-smb-direct"></a>使用 SMB 直接傳輸時的注意事項

- 您可以在容錯移轉叢集中使用 SMB 直接傳輸；不過，您必須確定用戶端存取使用的叢集網路適用 SMB 直接傳輸。 容錯移轉叢集支援使用多個網路搭配具備 RSS (接收端調整) 和 RDMA 功能的網路介面卡進行用戶端存取。
- 您可以在 Hyper-V 管理作業系統上使用 SMB 直接傳輸來支援使用透過 SMB 的 Hyper-V，以及為使用 Hyper-V 儲存堆疊的虛擬機器提供存放裝置。 不過，系統並不會對 Hyper-V 用戶端直接顯示具備 RDMA 功能的網路介面卡 。 如果您將具備 RDMA 功能的網路介面卡連線到虛擬交換器，則來自交換器的虛擬網路介面卡將不具備 RDMA 功能。
- 如果您停用 SMB 多重通道，則 SMB 直接傳輸也會一併停用。 由於 SMB 多重通道會偵測網路介面卡功能並判斷網路介面卡是否具備 RDMA 功能，因此，如果將 SMB 多重通道停用，用戶端便無法使用 SMB 直接傳輸。
- Windows RT 不支援 SMB 直接傳輸。 SMB 直接傳輸需要能夠支援具備 RDMA 功能的網路介面卡，而只有 Windows Server 2012 R2 和 Windows Server 2012 才可以使用這種介面卡。
- 舊版的 Windows Server 不支援 SMB 直接傳輸。 僅在 Windows Server 2012 R2 和 Windows Server 2012 上提供支援。

## <a name="enabling-and-disabling-smb-direct"></a>啟用和停用 SMB 直接傳輸

安裝 Windows Server 2012 R2 或 Windows Server 2012 時，預設會啟用 SMB 直接傳輸。 如果 SMB 用戶端有識別出適當設定的話，會自動偵測及使用多個網路連線。

### <a name="disable-smb-direct"></a>停用 SMB 直接傳輸

一般而言，您不需要停用 SMB 直接傳輸，不過，您可以執行下列其中一個 Windows PowerShell 指令碼予以停用。

若要停用特定介面的 RDMA，請輸入：

```PowerShell
Disable-NetAdapterRdma <name>
```

若要停用所有介面的 RDMA，請輸入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

當您停用用戶端或伺服器的 RDMA 時，系統便無法使用它。 Network Direct  是 Windows Server 2012 R2 和 Windows Server 2012 對於 RDMA 介面的基本網路支援內部名稱。

### <a name="re-enable-smb-direct"></a>重新啟用 SMB 直接傳輸

停用 RDMA 之後，您可以執行下列其中一個 Windows PowerShell 指令碼來重新予以啟用。

若要重新啟用特定介面的 RDMA，請輸入：

```PowerShell
Enable-NetAdapterRDMA <name>
```

若要重新啟用所有介面的 RDMA ，請輸入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

您必須同時啟用用戶端和伺服器的 RDMA，才能再次開始使用它。

## <a name="test-performance-of-smb-direct"></a>測試 SMB 直接傳輸的效能

您可以使用下列其中一個程序來測試效能的運作狀況。

### <a name="compare-a-file-copy-with-and-without-using-smb-direct"></a>比較使用和不使用 SMB 直接傳輸時的檔案複製

以下說明如何測量增加的 SMB 直接傳輸輸送量：

1. 設定 SMB 直接傳輸
2. 測量使用 SMB 直接傳輸時執行大型檔案複製所花的時間。
3. 停用網路介面卡上的 RDMA，請參閱 [Enabling and disabling SMB Direct](#enabling-and-disabling-smb-direct)。
4. 測量不使用 SMB 直接傳輸時執行大型檔案複製所花的時間。
5. 重新啟用網路介面卡上的 RDMA，然後比較兩個結果。
6. 若要避免快取的影響，請執行下列動作：
    1. 複製大量的資料 (大到記憶體無法處理的資料量)。
    2. 將資料複製兩次，以第一次複製做為練習，然後對第二次複製計時。
    3. 在每次測試之前將伺服器和用戶端都重新啟動，以確保它們都在相似的條件下運作。

### <a name="fail-one-of-multiple-network-adapters-during-a-file-copy-with-smb-direct"></a>使用 SMB 直接傳輸進行檔案複製時，讓多個網路介面卡的其中一個失效。

以下說明如何確認 SMB 直接傳輸的容錯移轉功能：

1. 確定 SMB 直接傳輸是在多網路介面卡設定下運作。
2. 執行大型檔案複製。 在執行複製時，藉由將其中一條纜線中斷連線 (或停用其中一個網路介面卡)，模擬其中一個網路路徑失敗的狀況。
3. 確認檔案複製作業在使用其中一個剩餘網路介面卡的情況下繼續執行，而未發生檔案複製錯誤。

>[!NOTE]
>若要避免未使用 SMB 直接傳輸的工作負載發生錯誤，請確定沒有其他工作負載使用中斷連線的網路路徑。

## <a name="more-information"></a>其他資訊

- [伺服器訊息區概觀](file-server-smb-overview.md)
- [提高伺服器、存放裝置及網路可用性：案例概觀](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署透過 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
