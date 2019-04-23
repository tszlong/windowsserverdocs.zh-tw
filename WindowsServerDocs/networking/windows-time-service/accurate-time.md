---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: 適用於 Windows Server 2016 的精確時間
description: Windows Server 2016 中的時間同步處理精確度具有已持續改善，同時維持完整回溯 NTP 與較舊的 Windows 版本的相容性。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ea4d957ee68f14f4568d3cefe664736585e50cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864009"
---
# <a name="accurate-time-for-windows-server-2016"></a>適用於 Windows Server 2016 的精確時間

>適用於：Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 中，Windows 10 或更新版本

Windows 時間服務是使用用戶端和伺服器的時間同步處理提供者的外掛程式模型的元件。  在 Windows 中，有兩個內建的用戶端提供者，而且有可用的協力廠商外掛程式。 一個提供者會使用[NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305)或是[MS NTP](https://msdn.microsoft.com/library/cc246877.aspx)同步處理至 NTP 和/或 MS NTP 相容的參考伺服器本機系統時間。 其他提供者是適用於 HYPER-V，並同步處理至 HYPER-V 主機的虛擬機器 (VM)。  當多個提供者存在時，Windows 會挑選最佳的提供者，首先，使用 stratum 層級後面根延遲，根離散，最後時間位移。

>[!NOTE]
>Windows 時間服務的快速概觀，請查看本[高層級概觀影片](https://aka.ms/WS2016TimeVideo)。

<!-- Not sure what to do with the following -->
在本主題中，我們會討論...這些主題與啟用正確的時間： 

- 增強功能
- 度量單位
- 最佳做法

>[!IMPORTANT]
>您可以下載 Windows 2016 正確時間發行項所參考的增補[此處](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)。  這份文件提供我們的測試和測量方法更多詳細資料。



>[!NOTE] 
>Windows 時間提供者外掛程式模型可以[記載於 TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

## <a name="domain-hierarchy"></a>網域階層
網域與獨立設定的運作方式。

- 網域成員使用安全的 NTP 通訊協定，這會使用驗證來確保安全性與時間參考的真確性。  網域成員主要取決於網域階層和評分系統時鐘同步處理。  在網域中，沒有時間 stratums，藉此讓每個 DC 指向父網域控制站，以更精確的時間 stratum 階層式層級。  階層會解析為 PDC 或根樹系中的 DC 或 DC，並代表良好的時間伺服器網域的 GTIMESERV 網域旗標。  請參閱[指定本機可靠時間服務使用 GTIMESERV](#GTIMESERV)下一節。

- 根據預設，獨立機器會設定為使用 time.windows.com。  您應該會指向 Microsoft 所擁有的資源的 DNS 伺服器解析此名稱。  類似所有遠端位於的時間參考，而網路中斷，可能會導致同步處理。  網路流量負載，並不對稱的網路路徑，可能會降低精確度的時間同步處理。  針對 1 毫秒精確度，您不能取決於遠端的時間來源。

HYPER-V 來賓會有至少兩個 Windows 時間提供者，可從中選擇，因為主機時間與 NTP，您可能會看到不同的行為，與網域或獨立以來賓身分執行時。

> [!NOTE] 
> 如需有關計分方法與網域階層的詳細資訊，請參閱[「 什麼是 Windows Time 服務 」？](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 部落格文章。

> [!NOTE]
> Stratum 是 NTP 和 HYPER-V 提供者中所使用的概念和其值指出在階層中的時鐘位置。  Stratum 1 保留給最高層級的時鐘，和 stratum 0 是保留供假設為正確的硬體，而且有很小或與它相關聯的任何延遲。  Stratum 2 stratum 1 與伺服器通訊，以 stratum 2 stratum 3，依此類推。  而較低的 stratum 通常表示更精確的時鐘，就可以找到不一致的地方。  此外，W32time 只接受從 stratum 15 或以下的時間。  若要查看用戶端 stratum，使用*w32tm /query /status*。

## <a name="critical-factors-for-accurate-time"></a>時間為準的關鍵因素
在每個時間為準的情況下，有三個關鍵因素：

1. **Solid 來源時鐘**-在您網域的需求是穩定且正確的來源時鐘。 這通常表示安裝的 GPS 裝置，或指向 Stratum 1 來源納入考量的 #3。 會，如果您有兩個船上水，而且您想要測量的高度，相較於另一個比喻，您的精確度是最佳來源船隻是否非常穩定且不移動。 同樣的時間，然後如果您的來源系統時鐘不穩定，整個鏈結中的同步處理時鐘是受影響的放大每個階段。 它也必須能夠因為中斷連線會干擾時間同步處理。 且最後，它必須是安全。 如果參考不正確的時間維護，或由可能是惡意的合作對象，您可能會公開您的網域，以時間為基礎的攻擊。
2. **穩定的用戶端時鐘**-穩定的用戶端時鐘可確保擺盪自然漂移是 containable。  NTP 會從可能的多部 NTP 伺服器的多個範例使用的狀況，並訓練您的本機電腦時鐘。  它不會逐步時間的變更，但而變慢或加速，讓您快速著手準確的時間，並保持正確 NTP 要求之間的本機時鐘。  不過，如果用戶端電腦時鐘的擺盪不穩定，然後在調整之間的多個變動可能會發生，而且 Windows 會使用條件時鐘的演算法無法正確運作。  在某些情況下，您可能需要韌體更新，時間為準。
3. **對稱的 NTP 通訊**-很重要的 NTP 通訊的連接是對稱。  NTP 會使用計算來調整假設網路修補程式是對稱的時間。  如果路徑 NTP 封包會採用伺服器要花費的時間才會傳回不同長，精確度會受到影響。  比方說，路徑可能因網路拓撲中或透過具有不同的介面速度裝置路由傳送的封包中的變更而變更。


電池供電裝置，行動裝置和可攜性，您必須考慮不同的策略。  根據我們的建議，讓正確的時間需要時鐘指 secure 一次的每秒時鐘更新頻率與相互關聯。 這些設定會消耗更多的電池電力，比預期，而且可能會干擾省電模式可用在 Windows 中，對於這類裝置。 電池供電裝置也已停止執行，W32time 的能力來訓練時鐘和維護正確的時間會干擾的所有應用程式特定電源模式。 此外，在行動裝置的時鐘不可能非常精確開始。  環境的環境條件影響時鐘的正確性，並在行動裝置可以將一個環境的條件移到下這可能會干擾能夠準確地記錄的時間。  因此，Microsoft 不建議您在使用高精確度設定設定電池供電的可攜式裝置。 

## <a name="why-is-time-important"></a>時間為何如此重要？  
有許多不同的原因，您可能需要精確的時間。  Windows 的一般案例是精確度的 Kerberos，需要 5 分鐘的用戶端與伺服器之間。  不過，有許多可能會受到時間精確度包括其他領域：


- 例如政府法規：
    - 位於美國的 FINRA 的 50 毫秒精確度
    - 1 ms ESMA (MiFID II) 在歐盟境內。
- 密碼編譯演算法
- 叢集/SQL/交換和文件 Db 等分散式的系統
- 比特幣交易的區塊鏈架構
- 分散式記錄檔和威脅分析 
- AD 複寫
- PCI （支付卡產業），目前 1 的第二個精確度



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
