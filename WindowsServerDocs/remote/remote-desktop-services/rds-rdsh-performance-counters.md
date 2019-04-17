---
title: 使用診斷遠端桌面工作階段主機上的應用程式的回應性問題的效能計數器
description: 您的應用程式慢速通道上執行 RDS 嗎？ 深入了解您可用來診斷 rdsh 的應用程式效能問題的效能計數器
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133714"
---
# 使用效能計數器來診斷遠端桌面工作階段主機上的應用程式效能問題

其中一個最難來診斷問題是不佳的應用程式的效能，應用程式執行速度變慢或未回應。 傳統上來說，開始您的診斷所收集的 CPU、 記憶體、 磁碟輸入/輸出，以及其他衡量標準，然後使用工具，例如 Windows Performance Analyzer 嘗試找出造成問題。 遺憾的是在大多數情況下這項資料不會協助您找出根本原因，因為資源耗用量計數器有頻繁和大型的變化。 這可讓您難以讀取資料，並將它與報告問題相互關聯。 為了協助您更快速地解決您的應用程式的效能問題，我們已經新增一些新效能計數器 （可用[來下載](#download-windows-server-insider-software)透過[Windows 測試人員計畫](https://insider.windows.com)），用來計算使用者輸入的流程。

使用者輸入延遲計數器可協助您快速找出根本原因，不好的終端使用者 RDP 體驗。 這個計數器測量多久任何使用者輸入 （例如滑鼠或鍵盤的使用方式） 停留在佇列中處理程序，挑選之前，並在本機和遠端工作階段中計數器。

下圖顯示下使用者輸入流程的概略表示從用戶端應用程式。

![遠端桌面-使用者輸入流程從使用者遠端桌面用戶端應用程式](.\media\rds-user-input.png)

使用者輸入延遲計數器測量最大的差異 （在的時間間隔內） 之間所排入佇列的輸入，並在[傳統的訊息迴圈](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop)中，應用程式所挑選下列流程圖中所示：

![遠端桌面-使用者輸入延遲效能計數器流程](.\media\rds-user-input-delay.png)

此計數器的一個重要的詳細資料是它可設定的間隔內報告的最大的使用者輸入的延遲。 這是最長時間的輸入，以觸達的應用程式可能會影響重要且可見的動作，例如輸入的速度。

例如，在下列表格中，使用者的輸入的延遲會報告為 1,000 ms 這個時間間隔內。 計數器會報告最慢的使用者輸入，延遲間隔中因為使用者的認知的 「 慢速 」 由輸入最慢的時間 （最多） 他們體驗，不平均所有的總輸入的速度。

|數字| 0 | 1 | 2 |
|------|---|---|---|
|延遲 |16 ms| 20 ms| 1,000 ms|

## 啟用並使用新的效能計數器

若要使用這些新的效能計數器，您必須先執行此命令來啟用登錄機碼：

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> 如果您使用 Windows 10，版本 1809年或更新版本或 Windows Server 2019，或更新版本，您不需要啟用登錄機碼。

接下來，重新啟動伺服器。 然後，開啟效能監視器，並選取加號 （+），如下列螢幕擷取畫面所示。

![遠端桌面的螢幕擷取畫面顯示如何新增使用者輸入延遲效能計數器](.\media\rds-add-user-input-counter-screen.png)

之後這樣一來，您應該會看到新增計數器] 對話方塊中，您可以在其中選取**每個處理程序的使用者輸入延遲**或**使用者的輸入延遲，每個工作階段**。

![遠端桌面的螢幕擷取畫面顯示如何新增每個工作階段的使用者輸入的延遲](.\media\rds-user-delay-per-session.png)

![遠端桌面的螢幕擷取畫面顯示如何新增每個處理程序的使用者輸入的延遲](.\media\rds-user-delay-per-process.png)

如果您選取**使用者的輸入延遲，每個處理程序**，您會看到**已選取物件的執行個體**（亦即，處理序），在```SessionID:ProcessID <Process Image>```格式。

例如，如果小算盤應用程式正在執行[工作階段識別碼 1](https://msdn.microsoft.com/library/ms524326.aspx)中，您會看到```1:4232 <Calculator.exe>```。

> [!NOTE]
> 並非所有處理程序會包含在內。 您將不會看到 SYSTEM 身分執行任何處理程序。

計數器會開始報告使用者輸入的延遲，因為您將它新增。 請注意，最大的縮放比例預設設定為 100 （毫秒）。 

![遠端桌面-活動的使用者輸入延遲，每個效能監視器 」 中的處理序的範例](.\media\rds-sample-user-input-delay-perfmon.png)

接下來，我們來看看**每個工作階段的使用者輸入延遲**。 針對每個工作階段識別碼、 執行個體和計數器它們顯示在指定的工作階段的任何處理程序的使用者輸入的延遲。 此外，有兩個執行個體，稱為 「 最大值 」 （最大的使用者輸入延遲橫跨所有工作階段） 及 「 平均 」 (平均 acorss 所有工作階段)。

下表顯示這些執行個體的視覺化範例。 （您可以取得相同的資訊在效能切換到報告圖形型別。）

|計數器的類型|執行個體的名稱|回報的延遲 （毫秒）|
|---------------|-------------|-------------------|
|每個處理程序的使用者輸入延遲|1:4232 < Calculator.exe >|  200|
|每個處理程序的使用者輸入延遲|2:1000 < Calculator.exe >|  16|
|每個處理程序的使用者輸入延遲|1:2000 < Calculator.exe >|  32|
|每個工作階段的使用者輸入延遲|1|    200|
|每個工作階段的使用者輸入延遲|2|    16|
|每個工作階段的使用者輸入延遲|平均|  108|
|每個工作階段的使用者輸入延遲|最大值|  200|

## 多載的系統中所用的計數器

現在我們來看看您會看到報告中如果應用程式的效能降低。 下圖顯示 Microsoft Word 中從遠端工作的使用者的讀數。 在此情況下，RDSH 伺服器的效能降低是經過一段時間因為更多使用者登入。

![遠端桌面-RDSH 伺服器執行 Microsoft Word 的範例效能圖形](.\media\rds-user-input-perf-graph.png)

以下是如何讀取圖形的行：

- 粉紅色行顯示在伺服器上登入工作階段數目。
- 紅線是 CPU 使用量。
- 綠色列是最大的使用者輸入的延遲橫跨所有的工作階段。
- （顯示為黑色，此圖表中） 的藍色一行代表平均使用者輸入的延遲橫跨所有工作階段。

您會注意到沒有 CPU 尖峰並使用者的輸入的延遲之間的相互關聯 — CPU 取得更多的使用方式，因為使用者輸入延遲增加。 此外，更多使用者會新增至系統，取得接近 100%，進而造成更頻繁的使用者輸入的延遲尖峰 CPU 使用量。 雖然這個計數器是在資源不足的伺服器執行所在的情況下非常實用，您也可以使用它來追蹤相關特定的應用程式的使用者輸入的延遲。

## 設定選項

若要使用這個效能計數器時請記住重點是，它會報告使用者輸入的延遲的 1,000 毫秒間隔預設。 如果您將效能計數器範例間隔屬性 （如下列螢幕擷取畫面所示） 不同的任何項目，回報的值將會不正確。

![遠端桌面-的屬性，以您的效能監視器](.\media\rds-user-input-perfmon-properties.png)

若要修正這個問題，您可以設定下列登錄機碼，以符合您想要使用的時間間隔 （以毫秒為單位）。 例如，如果我們變更範例每隔 x 秒到 5 秒，我們需要 5000 ms 設定此機碼。

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>如果您使用 Windows 10，版本 1809年或更新版本或 Windows Server 2019，或更新版本，您不需要設定 LagCounterInterval，若要修正的效能計數器。

我們也已新增幾個您可能會發現很有幫助相同的登錄機碼下方的機碼：

**LagCounterImageNameFirst** — 設為此索引鍵`DWORD 1`（預設值為 0 或機碼不存在）。 這樣會計數器名稱變更為 「 映像名稱為 < SessionID:ProcessId >。 」 例如，「 檔案總管 < 1:7964 >。 」 這非常有用，如果您想要排序的影像名稱。

**LagCounterShowUnknown** — 設為此索引鍵`DWORD 1`（預設值為 0 或機碼不存在）。 這會顯示為服務或系統執行任何處理程序。 有些處理程序將會顯示與他們的工作階段設定為 「？。 」

這是看起來像是如果您在開啟這兩個金鑰：

![遠端桌面-這兩個金鑰效能監視器](.\media\rds-user-input-delay-with-two-counters.png)

## 非 Microsoft 工具搭配使用的新計數器

監視工具可以取用此計數器使用[效能 API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx)。

## 下載 Windows Server 測試人員軟體

已註冊的測試人員可以直接瀏覽到[Windows Server Insider Preview 下載頁面](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)以取得最新的測試人員軟體下載項目。  若要深入了解如何註冊為測試人員，請參閱[開始使用伺服器](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## 分享您的意見反應

您可以提交意見反應中樞透過此功能的意見反應。 選取**應用程式 > 所有其他應用程式**且包含 「 RDS 效能計數器 — 效能監視器 」 中張貼的標題。

如一般的功能，請瀏覽[RDS UserVoice 頁面](https://aka.ms/uservoice-rds)。
