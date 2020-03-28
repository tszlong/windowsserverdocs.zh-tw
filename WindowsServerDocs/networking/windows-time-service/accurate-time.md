---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Windows Server 2016 準確時間
description: Windows Server 2016 中的時間同步準確度已大幅改善，同時維持與舊版 Windows 的完整 NTP 回溯相容性。
author: eross-msft
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 3320c67d52978f0e9abaae7d5bec9b4fcb727fd6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315086"
---
# <a name="accurate-time-for-windows-server-2016"></a>Windows Server 2016 準確時間

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

Windows Time 服務是一個元件，其使用用戶端和伺服器時間同步提供者都適用的外掛程式模型。  Windows 上有兩個內建用戶端提供者，還有第三方外掛程式可用。 其中一個提供者使用 [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) 或 [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx)，將本機系統時間同步處理至 NTP 和/或 MS NTP 相容參考伺服器。 另一個提供者適用於 Hyper-V，可將虛擬機器 (VM) 同步處理至 Hyper-V 主機。  若有多個提供者存在，Windows 會先使用層次層級，接著使用根延遲、根離散，最後使用時間位移來挑選最佳提供者。

> [!NOTE]
> 如需 Windows Time 服務的快速概觀，請參閱此[整體概觀影片](https://aka.ms/WS2016TimeVideo)。

在本主題中，我們將討論下列與達成準確時間相關的主題： 

- 改善
- 量測
- 最佳做法

> [!IMPORTANT]
> 在[這裡](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)可以下載「Windows 2016 準確時間」一文所參考的附錄。  本文件提供更多有關測試和測量方法的詳細資訊。

> [!NOTE] 
> Windows 時間提供者外掛程式模型已[記載於 TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx) 上。

## <a name="domain-hierarchy"></a>網域階層
網域和單機設定的運作方式不同。

- 網域成員會使用安全的 NTP 通訊協定，其使用驗證來確保時間參考的安全性和真實性。  網域成員會與網域階層和計分系統所決定的主要時鐘同步處理。  在網域中，有一個階層式時間層次，藉以讓每個 DC 指向具有更準確時間層次的父系 DC。  階層會解析為根樹系中的 PDC 或 DC，或具有 GTIMESERV 網域旗標的 DC，這代表網域的「良好時間伺服器」。  請參閱下面的 [使用 GTIMESERV 指定本機可靠時間服務] 一節。

- 預設會將單機設為使用 time.windows.com。  此名稱是由您的 DNS 伺服器解析，應指向 Microsoft 擁有的資源。  就如同所有位於遠端的時間參考，網路中斷可能會阻止同步處理。  網路流量負載和非對稱網路路徑可能會降低時間同步的準確度。  對於 1 毫秒的準確度，您不能依賴遠端時間來源。

由於 Hyper-V 來賓至少會有兩個 Windows Time 提供者可供選擇 (主機時間和 NTP)，因此以來賓身分執行時，您可能會看到網域或單機有不同的行為。

> [!NOTE] 
> 如需有關網域階層和計分系統的詳細資訊，請參閱[「Windows Time 服務是什麼？」](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 部落格文章。

> [!NOTE]
> 層次是在 NTP 和 Hyper-V 提供者中使用的概念，其值表示階層中的時鐘位置。  層次 1 會保留給最高層級的時鐘，而層次 0 則會保留給假定準確且幾乎沒有相關延遲的硬體。  層次 2 會與層次 1 伺服器交談，而層次 3 會與層次 2 交談，依此類推。  雖然較低的層次通常表示較準確的時鐘，但可能會發現不一致的情況。  此外，W32time 只接受來自層次 15 或以下的時間。  若要查看用戶端的層次，請使用 *w32tm /query /status*。

## <a name="critical-factors-for-accurate-time"></a>準確時間的關鍵因子
在每個準確時間案例中，都有三個重要因子：

1. **穩固的來源時鐘** - 您網域中的來源時鐘必須穩定且準確。 這通常表示安裝 GPS 裝置或指向層次 1 來源，並將 #3 納入考量。 依此類推，如果您在水上有兩艘船，且正嘗試測量其中一艘船相較於另一艘船的海拔高度，那麼如果來源船隻非常穩定且不移動，您的準確度就是最佳的。 時間也一樣，如果您的來源時鐘不穩定，則整個已同步的時鐘鏈會受到影響，並在每個階段擴大。 此外，來源時鐘也必須可存取，因為連線中斷會妨礙時間同步作業。 最後，其必須是安全的。 如果時間參考並未適當地維護，或由潛在的惡意方所操作，則可能會讓您的網域暴露於時間型攻擊。
2. **穩定的用戶端時鐘** - 穩定的用戶端時鐘可確保振盪器的自然漂移是可控制的。  NTP 會使用可能來自多部 NTP 伺服器的多個樣本，訓練及約束您的本機電腦時鐘。  其不會逐步進行時間變更，而是會使本機時鐘變慢或加快，讓您能快速靠近準確時間並在 NTP 要求間保持準確。  不過，如果用戶端電腦時鐘的振盪器不穩定，則調整間可能會發生更多波動，且 Windows 用於訓練時鐘的演算法無法準確地運作。  在某些情況下，可能需要進行韌體更新才能取得準確時間。
3. **對稱 NTP 通訊** - NTP 通訊的連線是對稱的很重要。  NTP 會使用計算作業來調整假定網路修補程式對稱的時間。  如果 NTP 封包前往伺服器時所採用的路徑需要不同的返回時間量，則準確度會受到影響。  例如，路徑可能會因為網路拓撲中的變更，或透過具有不同介面速度的裝置所路由傳送的封包而變更。

對於以電池供電的裝置 (行動和可攜式)，您必須考慮不同的策略。  依照我們的建議，若要保持準確時間，時鐘需要每秒約束一次，這與時鐘更新頻率相互關聯。 這些設定會耗用超過預期的電池電力，而且可能會妨礙 Windows 中適用於這類裝置的省電模式。 以電池供電的裝置也有特定的電源模式可讓所有應用程式停止執行，這會妨礙 W32time 約束時鐘及維持準確時間的能力。 此外，行動裝置中的時鐘一開始可能不是非常準確。  周圍環境條件會影響時鐘準確度，而行動裝置可以從一個周圍條件移到下一個，這可能會妨礙其保持準確時間的能力。  因此，Microsoft 不建議您以高準確度設定來設定以電池供電的可攜式裝置。 

## <a name="why-is-time-important"></a>為什麼時間很重要？  
您需要準確時間的原因有很多。  Windows 的一般情況是 Kerberos，其在用戶端與伺服器之間需要 5 分鐘的準確度。  不過，有許多其他領域可能會受時間準確度影響，包括：


- 政府法規，例如：
    - 美國 FINRA 的 50 毫秒準確度
    - 歐盟的 1 毫秒 ESMA (MiFID II)。
- 密碼編譯演算法
- 分散式系統，例如叢集/SQL/Exchange 和文件 DB
- 適用於比特幣交易的區塊鏈架構
- 分散式記錄和威脅分析 
- AD 複寫
- PCI (支付卡產業)，目前為 1 秒的準確度



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
