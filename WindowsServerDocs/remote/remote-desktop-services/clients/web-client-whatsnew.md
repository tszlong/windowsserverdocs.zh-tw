---
title: 什麼是新的遠端桌面 web 用戶端？
description: 深入了解遠端桌面 web 用戶端的最新變更
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 15218af2f084e9c998d89250aace1d763d03b42a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976332"
---
# <a name="whats-new-for-the-remote-desktop-web-client"></a>什麼是新的遠端桌面 web 用戶端？

我們會定期更新[遠端桌面 web 用戶端](remote-desktop-web-client.md)、 新增功能和修正問題。 查看最新的更新。

   >[!NOTE]
    >我們已變更版本控制系統的 web 用戶端。 從 1.0.18.0 版開始，所有的 web 用戶端版本將包含數字 （以 「 W.X.Y.Z"的格式）。 遠端桌面 web 用戶端的版本號碼的最後一定會以 0 (比方說，W.X.Y.0)。 每個 Windows 虛擬桌面 web 用戶端版本將會變更的最後一位數，直到下一步 的遠端桌面 web 用戶端版本 (例如 1.0.18.1)。

## <a name="updates-for-version-10180"></a>更新版本，1.0.18.0
*發行的日期：5/14/2019*

- 新增的資源啟動方法設定 索引標籤，讓使用者在瀏覽器中開啟資源，或下載要處理的另一個用戶端的.rdp 檔案中的組態。 此設定可能會設定由您的管理員。這項功能可在系統管理員設定的相關詳細資料[web 用戶端安裝程式文件](remote-desktop-web-client-admin.md)。
- 遠端工作階段中的色彩轉譯問題，啟用就越逼真的固定的色彩。
- 摘要的遠端資源的錯誤相關的修改過的錯誤訊息。 
- 已新增的支援更多 office 快速鍵，例如特殊的貼上 (Ctrl + Alt + V)。
- 新增的鍵盤快速鍵，讓使用者叫用 Windows 鍵，在遠端工作階段 (alt+f3)
- 嘗試使用過期的密碼進行驗證的使用者更新的錯誤訊息。
- 在 [所有資源] 頁面上的重新整理的摘要 UI。
- 解析重疊的對話工作階段期間發生的重新連線。
- 修正資源工作列中的遠端資源圖示大小。 

## <a name="updates-for-version-1011"></a>如需版本 1.0.11 的更新
*發行的日期：2/22/2019*

- 啟用 RD 代理人，不需要在 Windows Server 2019 RD 閘道連線。
- 依字母順序排序摘要 (亦即，Remoteapp 第一個桌面第二個)。
- 修正多個網頁可及性錯誤改善螢幕讀取器相容性。
- 更新我們的建置工具。
- 各種 bug 修正。

## <a name="updates-for-version-107"></a>更新版本，version:1.0.7
*發行的日期：1/24/2019*

- 現在支援在內部網路上的離線使用。
- 在非 Microsoft Edge 瀏覽器上呈現改善。
- 實作的限制摘要的擷取重試會嘗試防止 DoS。
- 固定的協助工具的錯誤，讓視障使用者使用 web 用戶端。
- 改進的摘要錯誤對使用者顯示的錯誤訊息。
- 新增的 Ctrl + Alt + End (Windows) 和 fn + 控制項 + 選項 + delete (Mac) 的捷徑來叫用遠端電腦中的 Ctrl + Alt + Del。
- 改善的遙測的當機事件。 
- 改善我們的組建管線和建置工具。
- 各種 bug 修正。

## <a name="updates-for-version-101"></a>1.0.1 版的更新
*發行的日期：10/29/2018*

- 新增的選項**擷取支援資訊**來診斷問題的 [關於] 頁面上。
- 現在支援 inPrivate 模式。
- 非英文鍵盤上的改善的支援。
- 已修正的問題，非英文字元的工具提示顯示不正確。
- 修正圖形會受影響的 Chrome 使用者的轉譯問題。
- 更新完整的 DST 支援時區重新導向。
- 改善的記憶體不足錯誤的錯誤訊息。
- 各種 bug 修正。

## <a name="updates-for-version-100"></a>1.0.0 版的更新
*發行的日期：07/16/2018*

- 現在已可使用遠端桌面 web 用戶端。
- 系統管理員可以全域關閉遙測 web 用戶端。
- 各種 bug 修正。

## <a name="updates-for-version-090"></a>0.9.0 版的更新
*發行的日期：07/05/2018*

- 新的登入 web 用戶端中的體驗。
- 當您啟動的桌面或應用程式的連線 （單一登入） 時，不會再提示輸入認證。
- 移至新的 URL 的 web 用戶端： **https://server_FQDN/RDWeb/webclient/index.html**
- 已新增的時區重新導向。
- 各種 bug 修正。

## <a name="updates-for-version-081"></a>0.8.1 版的更新
*發行的日期：05/17/2018*

- 若要解決 CredSSP 加密 oracle 補救 CVE-2018年-0886年中所述的更新。
- 啟用列印時，請修正某些語言中的連線失敗。
- 閘道不是部署的一部分時，就會改進的錯誤訊息。
- **幫助**並**意見反應**已新增選項。

## <a name="updates-for-version-080"></a>0.8.0 版的更新
*發行的日期：03/28/2018*

- Web 用戶端的初始公開預覽版本。
- 複製/貼上文字到剪貼簿**CTRL + C**並**CTRL + V**。
- 列印至 PDF 檔案。
- 18 種語言進行當地語系化。
 
