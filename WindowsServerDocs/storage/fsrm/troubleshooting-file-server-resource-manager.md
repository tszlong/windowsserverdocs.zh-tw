---
title: 檔案伺服器資源管理員疑難排解
description: 本文說明使用檔案伺服器資源管理員時如何對常見問題進行疑難排解。
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: fbd44938f57cc27576ba3d3bfa39b3e87e4989ed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950442"
---
# <a name="troubleshooting-file-server-resource-manager"></a>檔案伺服器資源管理員疑難排解

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server (半年通道) 、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

本節列出使用檔案伺服器資源管理員時，可能會遇到的常見問題。

> [!Note]
> 有一個不錯的疑難排解選擇，就是尋找檔案伺服器資源管理員所產生的事件記錄檔。 檔案伺服器資源管理員的所有事件記錄項目都可以在來源 **SRMSVC** 底下的 **\[應用程式\]** 事件記錄檔中找到。

## <a name="i-am-not-receiving-e-mail-notifications"></a>我沒有收到電子郵件通知。

-   **原因**：電子郵件選項未設定，或設定不正確。

-   **解決方案**：在 **\[電子郵件通知\]** 索引標籤的 **\[檔案伺服器資源管理員選項\]** 對話方塊中，確認 SMTP 伺服器及預設電子郵件收件者已指定且為有效。 傳送測試電子郵件以確認資訊正確且傳送通知所用 SMTP 伺服器運作正常。 如需詳細資訊，請參閱[設定電子郵件通知](configure-email-notifications.md)。


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>我只會收到一封電子郵件通知，即使觸發該通知的事件連續發生數次，也是如此。

-   **原因**：當使用者嘗試儲存封鎖的檔案或儲存超過配額閾值的檔案數次，而且電子郵件通知已針對該檔案檢測或配額事件進行設定時，預設在 60 分鐘期間只傳送一封電子郵件給系統管理員。 這樣可避免系統管理員的電子郵件帳戶中出現大量重複的訊息。

-   **解決方案**：在 **\[通知限制\]** 索引標籤的 **\[檔案伺服器資源管理員選項\]** 對話方塊中，您可以設定每一個通知類型的時間限制：電子郵件、事件記錄檔、命令和報告。 每項限制都會指定針對同樣問題產生另一相同類型通知之前必須經過的一段時間。 如需詳細資訊，請參閱[設定通知限制](configure-notification-limits.md)。


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>我的存放裝置報告持續失敗，事件記錄檔卻很少或幾乎沒有提供失敗原因的相關資訊。

-   **原因**：儲存報告所在的磁碟區可能已損毀。
-   **解決方案**：對磁碟區執行 **chkdsk**，再重新嘗試產生報告。

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>我的 [檔案檢測稽核] 報告沒有包含任何資訊。

-   **原因**：可能是下列其中一個或多個原因所造成︰
    -   稽核資料庫未設定成記錄檔案檢測活動。
    -   稽核資料庫是空的 (也就是，沒有記錄任何檔案檢測活動)。
    -   [檔案檢測稽核] 報告的參數沒有從稽核資料庫選取資料。

-   **解決方案**：在 **\[檔案檢測稽核\]** 索引標籤的 **\[檔案伺服器資源管理員選項\]** 對話方塊中，確認已選取 **\[在稽核資料庫中記錄檔案檢測活動\]** 核取方塊。
    -   如需有關記錄檔案檢測活動的詳細資訊，請參閱[設定檔檢測稽核](configure-file-screen-audit.md)。
    -   若要設定 [檔案檢測稽核] 報告的預設參數，請參閱[設定存放裝置報告](configure-storage-reports.md)。
    -   若要編輯已排程報告工作或隨選報告的報告參數，請參閱[存放裝置報告管理](storage-reports-management.md)。

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>我所建立的某些配額，其 [已使用] 值及 [可用] 值與實際的 [限制] 設定不相符。

-   **原因**：您的配額可能是巢狀配額，其中子資料夾的配額由其上層資料夾配額衍生了更嚴格的限制。 例如，您可能已將 100 MB的配額限制套用至上層資料夾，又分別將 200 MB 的配額限制套用至其每個子資料夾。 如果上層資料夾中總共儲存了 50 MB 的資料 (其子資料夾中儲存的資料總和)，則每一個子資料夾將會列出只有 50 MB 的可用空間。

-   **解決方案**：在 **\[配額管理\]** 中，按一下 **\[配額\]**。 在 **\[結果\]** 窗格中，選取您要進行疑難排解的配額項目。 在 **\[動作\]** 窗格中，按一下 **\[檢視影響資料夾的配額\]**，然後尋找套用至上層資料夾的配額。 這樣可讓您找出哪些上層資料夾配額的存放限制設定比您所選取的配額還要低。

