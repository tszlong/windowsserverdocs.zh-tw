---
title: 使用效能計數器來診斷在遠端桌面工作階段主機上的應用程式回應性問題
description: 您的應用程式執行緩慢在 RDS 上嗎？ 了解您可用來診斷 RDSH 上的應用程式效能問題的效能計數器
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 241b2b776a68cf5aec68a4d331201a07f0e5ea53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844649"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>使用效能計數器來診斷應用程式在遠端桌面工作階段主機上的效能問題

診斷最困難的問題之一是應用程式效能低落，應用程式執行速度很慢或沒有回應。 傳統上，開始診斷所收集的 CPU、 記憶體、 磁碟輸入/輸出和其他度量，然後使用 Windows Performance Analyzer 等工具嘗試找出造成問題。 不幸的是在大部分情況下這項資料不會協助您識別根本原因，因為資源耗用量計數器有常用及最大的變化。 這可讓您難以讀取資料，並將它與回報的問題相互關聯。 若要可協助您更快速地解決您的應用程式效能問題，我們增加了一些新的效能計數器 (可用[下載](#download-windows-server-insider-software)透過[Windows 測試人員計畫](https://insider.windows.com)) 該量值使用者輸入流程。

使用者輸入延遲計數器可協助您快速找出不正確的終端使用者體驗 RDP 的根本原因。 此計數器會測量多久任何使用者輸入 （例如滑鼠或鍵盤的使用方式） 會保留在佇列之前的程序中，挑選，而且計數器適用於本機和遠端工作階段。

下圖顯示從用戶端應用程式的粗略的表示法的使用者輸入流程。

![遠端桌面-使用者輸入流程從使用者的遠端桌面用戶端應用程式](.\media\rds-user-input.png)

使用者輸入延遲計數器測量 （在一段時間） 排入佇列的輸入與當它會取用中應用程式之間的最大的差異[傳統的訊息迴圈](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop)，如以下流程圖所示：

![遠端桌面-使用者輸入延遲的效能計數器的流程](.\media\rds-user-input-delay.png)

此計數器的一個重要的詳細資料是它在可設定的間隔內，報告的最大使用者輸入的延遲。 這是到達應用程式，可能會影響速度的重要且可見的動作，例如輸入的輸入所花費的時間最長。

比方說，在下列資料表中，使用者輸入的延遲會回報為 1000 毫秒在此間隔內。 計數器會報告最慢的使用者輸入延遲間隔中因為 「 緩慢 」 的使用者對於取決於最慢的輸入時間 （最多） 他們體驗，不平均總的所有輸入的速度。

|Number| 0 | 1 | 2 |
|------|---|---|---|
|延遲 |16 毫秒| 20 毫秒| 1000 毫秒|

## <a name="enable-and-use-the-new-performance-counters"></a>啟用及使用新的效能計數器

若要使用這些新的效能計數器，您必須先執行此命令啟用登錄機碼：

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> 如果您使用 Windows 10 版 1809年或更新版本或 Windows Server 2019 或更新版本中，您不需要啟用登錄機碼。

接下來，重新啟動伺服器。 然後，開啟效能監視器，並選取加號 （+），如下列螢幕擷取畫面所示。

![遠端桌面-螢幕擷取畫面顯示如何加入使用者輸入延遲效能計數器](.\media\rds-add-user-input-counter-screen.png)

之後，您應該會看到 [新增計數器] 對話方塊中，您可以在其中選取**每個處理序的使用者輸入延遲**或是**每個工作階段的使用者輸入延遲**。

![遠端桌面-螢幕擷取畫面顯示如何加入每個工作階段的使用者輸入的延遲](.\media\rds-user-delay-per-session.png)

![遠端桌面-螢幕擷取畫面顯示如何加入每個處理序的使用者輸入的延遲](.\media\rds-user-delay-per-process.png)

如果您選取**每個處理序的使用者輸入延遲**，您會看到**所選物件的執行個體**（亦即，處理序） 中```SessionID:ProcessID <Process Image>```格式。

例如，如果 [小算盤] 應用程式正在執行[工作階段識別碼 1](https://msdn.microsoft.com/library/ms524326.aspx)，您會看到```1:4232 <Calculator.exe>```。

> [!NOTE]
> 會包含不是所有的處理序。 您不會看到以系統身分執行任何處理程序。

計數器中，便會啟動時將它報告的使用者輸入的延遲。 請注意，最大調整規模設定為 100 （毫秒） 預設。 

![遠端桌面-每個效能監視器中的程序的使用者輸入延遲活動範例](.\media\rds-sample-user-input-delay-perfmon.png)

接下來，讓我們看看**每個工作階段的使用者輸入延遲**。 每個工作階段識別碼的執行個體，其計數器會顯示使用者輸入的延遲之任何處理序內指定的工作階段。 此外，有兩個名為"Max"（最大使用者輸入延遲跨所有工作階段） 和 「 平均 」 (平均 acorss 所有工作階段) 的執行個體。

下表顯示這些執行個體的視覺範例。 （您可以取得相同的資訊在 Perfmon 中藉由切換至 報表圖形類型。）

|計數器型別|執行個體名稱|報告的延遲 （毫秒）|
|---------------|-------------|-------------------|
|每個處理序的使用者輸入延遲|1:4232 <Calculator.exe>|  200|
|每個處理序的使用者輸入延遲|2:1000 <Calculator.exe>|  16|
|每個處理序的使用者輸入延遲|1:2000 <Calculator.exe>|  32|
|每個工作階段的使用者輸入延遲|1|    200|
|每個工作階段的使用者輸入延遲|2|    16|
|每個工作階段的使用者輸入延遲|平均|  108|
|每個工作階段的使用者輸入延遲|最大值|  200|

## <a name="counters-used-in-an-overloaded-system"></a>多載的系統中所使用的計數器

現在讓我們看看您會看到在報表中的應用程式的效能會降低。 下圖顯示使用者從遠端使用 Microsoft Word 中的數據。 在此情況下，RDSH 伺服器的效能會隨著時間降低為更多使用者登入。

![遠端桌面-執行 Microsoft Word 的 RDSH 伺服器的範例效能圖表](.\media\rds-user-input-perf-graph.png)

以下是如何讀取圖形的幾行：

- 粉紅色行顯示登入伺服器上的工作階段數目。
- 紅線是 CPU 使用量。
- 綠色線條是跨所有工作階段的最大使用者輸入的延遲。
- （顯示為黑色，此圖形中） 的藍線表示跨所有工作階段的平均使用者輸入的延遲。

您會注意到沒有 CPU 尖峰的原因和使用者輸入的延遲之間的相互關聯，如 CPU 取得更多使用量，使用者輸入延遲會增加。 此外，當更多使用者取得加入至系統時，CPU 使用量取得接近 100%，導致更頻繁的使用者輸入的延遲尖峰。 雖然此計數器是很有用的資源不足的伺服器執行所在的情況下，您也可以使用它來追蹤與特定的應用程式相關的使用者輸入的延遲。

## <a name="configuration-options"></a>組態選項

使用此效能計數器時記住的重點是，它報告的使用者輸入的延遲預設為 1000 毫秒間隔。 如果設定效能計數器範例間隔屬性 （如下列螢幕擷取畫面所示） 到不同的任何項目，則回報的值會不正確。

![遠端桌面-效能監控的屬性](.\media\rds-user-input-perfmon-properties.png)

若要修正此問題，您可以設定下列登錄機碼，以符合您想要使用的間隔 （以毫秒為單位）。 例如，如果我們變更範例每隔 x 秒到 5 秒，我們需要將這個金鑰設為 5000 毫秒。

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>如果您使用 Windows 10 版 1809年或更新版本或 Windows Server 2019 或更新版本中，您不需要設定 LagCounterInterval，若要修正的效能計數器。

我們也新增了幾個您可能會有所助益相同的登錄機碼底下的機碼：

**LagCounterImageNameFirst** — 若要設定此機碼`DWORD 1`（預設值為 0 或索引鍵不存在）。 這會計數器名稱變更為 「 映像名稱為 < SessionID:ProcessId >。 」 例如，「 總管 < 1:7964 > 」。 這非常有用，如果您想要依影像名稱排序。

**LagCounterShowUnknown** — 若要設定此機碼`DWORD 1`（預設值為 0 或索引鍵不存在）。 這會顯示任何為服務或系統執行的處理序。 某些處理程序將會顯示其工作階段設定為 「？。 」

這是看起來像是如果您開啟這兩個金鑰：

![遠端桌面-效能監視器，在這兩個索引鍵](.\media\rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>使用非 Microsoft 工具使用新的計數器

監視工具可以藉由使用此計數器[Perfmon API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx)。

## <a name="download-windows-server-insider-software"></a>下載 Windows Server Insider 軟體

已註冊的測試人員可以直接瀏覽[Windows Server Insider Preview 下載頁面](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)取得最新的測試人員軟體下載。  若要了解如何註冊為測試人員，請參閱[Server 入門](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## <a name="share-your-feedback"></a>共用您的意見反應

您可以提交意見反應中樞透過這項功能的意見反應。 選取**應用程式 > 所有其他應用程式**並包含"RDS 效能計數器 — 效能監視器 」 在自己的文章標題中。

一般功能的想法，請瀏覽[RDS UserVoice 頁面](https://aka.ms/uservoice-rds)。
