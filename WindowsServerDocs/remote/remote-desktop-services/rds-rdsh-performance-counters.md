---
title: 在遠端桌面工作階段主機上使用效能計數器來診斷應用程式回應性問題
description: 您的應用程式在 RDS 上的執行速度很慢嗎？ 了解可用來在 RDSH 上診斷應用程式效能問題的效能計數器
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/11/2019
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 3eb1e4b6da971d788383b8facbf8bbcbe00a5953
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870914"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>在遠端桌面工作階段主機上使用效能計數器來診斷應用程式效能問題

> 適用於：Windows Server 2019、Windows 10

最難診斷的問題之一是應用程式效能低落：應用程式執行速度很慢或沒有回應。 傳統上，您會在診斷一開始收集 CPU、記憶體、磁碟輸入/輸出及其他計量，然後使用 Windows Performance Analyzer 等工具嘗試找出導致問題的原因。 遺憾的是，在大部分的情況下，此資料無法協助您識別根本原因，因為資源耗用量計數器具有頻繁且巨大的變化。 這讓您難以讀取資料並將它與所回報的問題相互關聯。 為了協助您快速地解決應用程式效能問題，我們新增了一些新的效能計數器 (可透過 [Windows 測試人員計畫](https://insider.windows.com) (英文) [下載](#download-windows-server-insider-software)) 來測量使用者輸入流程。

>[!NOTE]
>使用者輸入延遲計數器只適用於：
> - Windows Server 2019 或更新版本
> - Windows 10，版本 1809 或更新版本

使用者輸入延遲計數器可協助您快速找出造成終端使用者 RDP 體驗不佳的根本原因。 此計數器會測量任何使用者輸入 (例如滑鼠或鍵盤使用方式) 要先在佇列中保留多久時間，然後處理序才會挑選它，而此計數器適用於本機和遠端工作階段。

下圖顯示從用戶端到應用程式之使用者輸入流程的粗略表示法。

![遠端桌面：從使用者遠端桌面用戶端到應用程式的使用者輸入流程](./media/rds-user-input.png)

使用者輸入延遲計數器會在[傳統訊息迴圈](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop) \(英文\) 中，測量要排入佇列的輸入與在應用程式挑選該輸入時之間的最大差異 (在一段時間間隔內)，如以下流程圖所示：

![遠端桌面：使用者輸入延遲效能計數器流程](./media/rds-user-input-delay.png)

此計數器的重要詳細資料之一是，它會在可設定的間隔內回報使用者輸入延遲上限。 這是輸入到達應用程式所花費的最長時間，其可能會影響重要和可見動作 (例如輸入) 的速度。

例如，在下表中，使用者輸入延遲會在此間隔內回報為 1,000 毫秒。 計數器會以該間隔回報最慢的使用者輸入延遲，因為使用者對於「緩慢」的認知取決於他們所體驗到的最慢輸入時間 (上限)，而非所有輸入總數的平均速度。

|Number| 0 | 1 | 2 |
|------|---|---|---|
|延遲 |16 毫秒| 20 毫秒| 1,000 毫秒|

## <a name="enable-and-use-the-new-performance-counters"></a>啟用並使用新的效能計數器

若要使用這些新的效能計數器，您必須先執行下列命令來啟用登錄機碼：

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> 如果您使用的是 Windows 10 1809 版或更新版本，或是 Windows Server 2019 或更新版本，則不需啟用登錄機碼。

接下來，重新啟動伺服器。 然後，開啟效能監視器並選取加號 (+)，如下列螢幕擷取畫面所示。

![遠端桌面：顯示如何新增使用者輸入延遲效能計數器的螢幕擷取畫面](./media/rds-add-user-input-counter-screen.png)

執行此操作之後，您應該會看到 [新增計數器] 對話方塊，讓您可在其中選取 [每個處理序的使用者輸入延遲]  或 [每個工作階段的使用者輸入延遲]  。

![遠端桌面：顯示如何針對每個工作階段新增使用者輸入延遲的螢幕擷取畫面](./media/rds-user-delay-per-session.png)

![遠端桌面：顯示如何針對每個處理序新增使用者輸入延遲的螢幕擷取畫面](./media/rds-user-delay-per-process.png)

如果您選取 [每個處理序的使用者輸入延遲]  ，您將會看到格式為 ```SessionID:ProcessID <Process Image>``` 的 [所選物件的執行個體]  (亦即處理序)。

例如，如果 [小算盤] 應用程式正在[工作階段識別碼 1](https://msdn.microsoft.com/library/ms524326.aspx) \(英文\) 中執行，您將會看到 ```1:4232 <Calculator.exe>```。

> [!NOTE]
> 並非所有處理序都會包含在內。 您將不會看到以 SYSTEM 身分執行的任何處理序。

一旦您新增它之後，計數器就會立即開始回報使用者輸入延遲。 請注意，預設會將調整上限設定為 100 (毫秒)。 

![遠端桌面：效能監視器中針對每個處理序所使用之使用者輸入延遲的活動範例](./media/rds-sample-user-input-delay-perfmon.png)

接下來，讓我們看看 [每個工作階段的使用者輸入延遲]  。 每個工作階段識別碼都有執行個體，而其計數器會顯示指定的工作階段內任何處理序的使用者輸入延遲。 此外，有兩個執行個體分別名為「上限」(所有工作階段上的使用者輸入延遲上限) 和「平均」(所有工作階段上的平均)。

下表顯示這些執行個體的視覺範例 (您可以在 Perfmon 中藉由切換至 [報表] 圖表類型來取得相同資訊)。

|計數器類型|執行個體名稱|回報的延遲 (毫秒)|
|---------------|-------------|-------------------|
|每個處理序的使用者輸入延遲|1:4232 <Calculator.exe>|  200|
|每個處理序的使用者輸入延遲|2:1000 <Calculator.exe>|  16|
|每個處理序的使用者輸入延遲|1:2000 <Calculator.exe>|  32|
|每個工作階段的使用者輸入延遲|1|    200|
|每個工作階段的使用者輸入延遲|2|    16|
|每個工作階段的使用者輸入延遲|平均|  108|
|每個工作階段的使用者輸入延遲|最大值|  200|

## <a name="counters-used-in-an-overloaded-system"></a>已超載之系統中所使用的計數器

現在讓我們看看如果應用程式的效能已降級，您將會在報表中看見哪些內容。 下圖顯示從遠端在 Microsoft Word 中工作的使用者讀數。 在此案例中，RDSH 伺服器效能會因為有更多使用者登入，隨著時間而降級。

![遠端桌面：執行 Microsoft Word 的 RDSH 伺服器範例效能圖表](./media/rds-user-input-perf-graph.png)

以下是閱讀圖表線條的方式：

- 粉紅色線條顯示伺服器上已登入的工作階段數目。
- 紅色線條是 CPU 使用量。
- 綠色線條是所有工作階段上的使用者輸入延遲上限。
- 藍色線條 (在此圖表中顯示為黑色) 表示所有工作階段上的平均使用者輸入延遲。

您將注意到 CPU 尖峰和使用者輸入延遲之間會相互關聯：隨著 CPU 的使用量增加，使用者輸入延遲會增加。 此外，當更多使用者加入至系統時，CPU 使用量就會更接近 100%，因而導致更頻繁的使用者輸入延遲尖峰。 雖然此計數器在發生伺服器執行資源不足的情況下非常有用，但您也可以使用它來追蹤與特定應用程式相關的使用者輸入延遲。

## <a name="configuration-options"></a>設定選項

使用此效能計數器時要記住的重點是，它預設會以 1000 毫秒的間隔回報使用者輸入延遲。 如果您將效能計數器範例間隔屬性 (如下列螢幕擷取畫面所示) 設定為任何不同的項目，則所回報的值將不正確。

![遠端桌面：效能監視器的屬性](./media/rds-user-input-perfmon-properties.png)

若要修正此問題，您可以設定下列登錄機碼，以符合您想要使用的間隔 (以毫秒為單位)。 例如，如果將範例每隔 x 秒變更為 5 秒，則需要將這個機碼設為 5000 毫秒。

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>如果您使用 Windows 10 1809 版或更新版本，或是 Windows Server 2019 或更新版本，則不需設定 LagCounterInterval 來修正效能計數器。

我們也在相同的登錄機碼底下新增了數個您可能發現有所助益的機碼：

**LagCounterImageNameFirst**：將此機碼設定為 `DWORD 1` (預設值為 0 或機碼不存在)。 這會將計數器名稱變更為「映像名稱 <SessionID:ProcessId>」。 例如「explorer <1:7964>」。 如果您想要依映像名稱排序，這非常有用。

**LagCounterShowUnknown**：將此機碼設定為 `DWORD 1` (預設值為 0 或機碼不存在)。 這會顯示任何以服務或 SYSTEM 身分執行的處理序。 某些處理序將會和其設定為 "?" 的工作階段一起顯示。

如果您同時開啟這兩個機碼，這就是它看起來的樣子：

![遠端桌面：已開啟這兩個機碼的效能監視器](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>搭配非 Microsoft 工具使用新的計數器

監視工具可以藉由使用 [Perfmon API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx) \(英文\) 來取用此計數器。

## <a name="download-windows-server-insider-software"></a>下載 Windows Server 測試人員軟體

已註冊的測試人員可以直接巡覽至 [Windows Server Insider Preview 下載頁面](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) \(英文\)，來取得最新的測試人員軟體下載。  若要了解如何註冊為測試人員，請參閱[開始使用 Server](https://insider.windows.com/en-us/for-business-getting-started-server/) \(英文\)。

## <a name="share-your-feedback"></a>分享您的意見反應

您可以透過意見反應中樞來提交此功能的意見反應。 選取 [應用程式] > [所有其他應用程式]  ，並在自己的文章標題中包含「RDS 效能計數器 - 效能監視器」。

如需一般功能的構想，請瀏覽 [RDS UserVoice 頁面](https://aka.ms/uservoice-rds) \(英文\)。
