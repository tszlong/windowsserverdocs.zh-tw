---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Windows Server 2016 的準確時間
description: Windows Server 2016 中的時間同步處理精確度已大幅改善，同時維持與舊版 Windows 的完整回溯 NTP 相容性。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 1399ed6a50085baa37f06c09b8c3e18ca8bca98b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395712"
---
# <a name="accurate-time-for-windows-server-2016"></a>Windows Server 2016 的準確時間

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

Windows 時間服務是一個元件，它會使用用戶端和伺服器時間同步處理提供者的外掛程式模型。  Windows 上有兩個內建的用戶端提供者，並提供協力廠商外掛程式。 一個提供者會使用[ntp （RFC 1305）](https://tools.ietf.org/html/rfc1305)或[ms-ntp](https://msdn.microsoft.com/library/cc246877.aspx) ，將本機系統時間與 NTP 和/或 ms ntp 相容參照伺服器同步。 另一個提供者適用于 Hyper-v，並會將虛擬機器（VM）同步處理至 Hyper-v 主機。  當有多個提供者存在時，Windows 會先使用階層層級來挑選最佳提供者，後面接著根延遲、根散佈，最後是時間位移。

> [!NOTE]
> 如需 Windows Time 服務的快速總覽，請參閱此[高階總覽影片](https://aka.ms/WS2016TimeVideo)。

在本主題中，我們將討論 .。。這些主題與啟用正確時間相關： 

- 措施
- 測評
- 最佳做法

> [!IMPORTANT]
> 您可以在[這裡](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)下載 Windows 2016 精確時間文章所參考的增補。  本檔提供有關測試和測量方法的更多詳細資料。

> [!NOTE] 
> Windows 時間提供者外掛程式模型已[記載于 TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

## <a name="domain-hierarchy"></a>網域階層
網域和獨立設定的工作方式不同。

- 網域成員使用安全的 NTP 通訊協定，其使用驗證來確保時間參考的安全性和真實性。  網域成員會與網域階層和評分系統所決定的主要時鐘進行同步處理。  在網域中，有一個階層式的時間 stratums，讓每個 DC 指向具有更精確時間層次的父系 DC。  階層會解析為根樹系中的 PDC 或 DC，或具有 GTIMESERV 網域旗標的 DC，這代表網域的良好時間伺服器。  請參閱下面的[使用 GTIMESERV 指定本機可靠時間服務](#GTIMESERV)一節。

- 預設會將獨立電腦設定為使用 time.windows.com。  此名稱是由您的 DNS 伺服器解析，這應指向 Microsoft 擁有的資源。  就像所有遠端找出時間參照，網路中斷可能會阻止同步處理。  網路流量負載和非對稱的網路路徑可能會降低時間同步處理的準確性。  對於1毫秒的精確度，您不能依賴遠端時間來源。

由於 Hyper-v 來賓至少會有兩個 Windows 時間提供者可供選擇（主機時間和 NTP），因此以來賓身分執行時，您可能會看到網域或獨立的不同行為。

> [!NOTE] 
> 如需有關網域階層和計分系統的詳細資訊，請參閱「[什麼是 Windows 時間服務？](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 」 blog 文章。

> [!NOTE]
> 階層是在 NTP 和 Hyper-v 提供者中使用的概念，其值表示階層中的時鐘位置。  階層1是保留給最高層級的時鐘，而階層0則是保留給硬體使用，而不會有任何相關聯的延遲。  層次2與第1部伺服器交談，第3層次到層次2等等。  雖然較低的層次通常會指出較精確的時鐘，但可能會發現不一致的情況。  此外，W32time 只接受來自層次15或以下的時間。  若要查看用戶端的層次，請使用*w32tm/query/status*。

## <a name="critical-factors-for-accurate-time"></a>精確時間的關鍵因素
在每個案例中，都有三個重要因素：

1. **穩固的來源時鐘**-您網域中的來源時鐘必須穩定且精確。 這通常表示安裝 GPS 裝置或指向第1層次來源，並將 #3 納入考慮。 相較之下，如果您在水上有兩個 boats，而您想要測量一個與另一個的高度，那麼如果來源船的穩定性很穩定而不移動，您的精確度就會最佳。 時間也相同，如果您的來源時鐘不穩定，則整個同步處理的時鐘鏈會受到影響，並在每個階段進行相應放大。 它也必須是可存取的，因為連接中斷會干擾時間同步處理。 最後，它必須是安全的。 如果未正確地維護時間參考，或由潛在的惡意物件操作，您可以將網域暴露于時間型攻擊中。
2. **穩定的用戶端時鐘**-穩定的用戶端時鐘可確保 oscillator 的自然漂移是 containable。  NTP 使用多個範例，從可能有多部 NTP 伺服器到條件，並將您的本機電腦的頻率設為專業  它不會逐步進行時間變更，而是會減緩或加速本機時鐘，讓您在 NTP 要求之間快速且保持正確的時間。  不過，如果用戶端電腦時鐘的 oscillator 不穩定，則可能會發生調整之間的波動，而 Windows 用於條件的演算法則無法精確地處理。  在某些情況下，可能需要進行固件更新以取得精確的時間。
3. **對稱式 ntp 通訊**-ntp 通訊的連線非常重要。  NTP 會使用計算來調整假設網路修補程式已對稱的時間。  如果 NTP 封包傳送至伺服器的路徑需要不同的時間來傳回，則精確度會受到影響。  例如，路徑可能會因為網路拓撲中的變更，或透過具有不同介面速度的裝置所路由的封包而變更。

針對備有電池的裝置（行動和便攜），您必須考慮不同的策略。  根據我們的建議，保持準確的時間需要每秒有一次的頻率，這會與時鐘更新頻率相互關聯。 這些設定會耗用超過預期的電池計量，而且可能會干擾 Windows 中適用于這類裝置的省電模式。 電池供電的裝置也有特定的電源模式，可讓所有應用程式停止執行，這會干擾 W32time's 的頻率並維持準確的時間。 此外，行動裝置中的時鐘開始可能不太精確。  環境環境條件會影響時鐘精確度，而行動裝置可以從一個環境條件移到下一個，這可能會干擾其能夠正確地持續時間。  因此，Microsoft 不建議您以高準確度設定來設定電池供電的可攜式裝置。 

## <a name="why-is-time-important"></a>為什麼時間很重要？  
有許多不同的原因，您可能需要精確的時間。  Windows 的一般情況是 Kerberos，這需要用戶端與伺服器之間5分鐘的精確度。  不過，有許多其他可能會受到時間精確度影響的區域，包括：


- 政府法規，例如：
    - 美國 FINRA 的50毫秒精確度
    - 歐盟的1毫秒 ESMA （MiFID II）。
- 密碼編譯演算法
- 分散式系統，例如 Cluster/SQL/Exchange 和檔 Db
- 適用于比特幣交易的區塊鏈架構
- 分散式記錄和威脅分析 
- AD 複寫
- PCI （付款卡產業），目前1秒的精確度



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
