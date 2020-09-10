---
title: Web 用戶端中的新功能
description: 了解遠端桌面 Web 用戶端最新的變更
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 09/02/2020
ms.localizationpriority: medium
ms.openlocfilehash: 18142988108e1eafe59ca7fd83a29dd4dfb87720
ms.sourcegitcommit: 664ed9bb0bbac2c9c0727fc2416d8c437f2d5cbe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2020
ms.locfileid: "89472028"
---
# <a name="whats-new-in-the-web-client"></a>Web 用戶端中的新功能

我們會定期更新[遠端桌面 Web 用戶端](remote-desktop-web-client.md)，進而新增功能並修正問題。 您可以在這裡找到最新的更新。

> [!NOTE]
> 我們已變更 Web 用戶端的版本控制系統。 從 1.0.18.0 版開始，所有 Web 用戶端發行版本將包含數字 (採用 "W.X.Y.Z" 的格式)。 遠端桌面 Web 用戶端的版本號碼結尾一律是 0 (例如，W.X.Y.0)。 每個 Windows 虛擬桌面 Web 用戶端版本都會變更最後一位數，直到發行下一版遠端桌面 Web 用戶端為止 (例如 1.0.18.1)。

## <a name="updates-for-10220"></a>1\.0.22.0 版的更新
*發行日期：2020/9/2*

- 使用者現在可以移動最小化功能表。
- 已改進對 4K 和 Ultra-wide 監視器的支援，並修正了複製大量資料時會導致工作階段當機的問題。
- 已改進在遠端工作階段中使用輸入法編輯器的支援。 若要深入了解透過 Web 用戶端使用輸入法編輯器，請參閱[透過 Web 用戶端連線到 Windows 虛擬桌面](/azure-docs/articles/virtual-desktop/connect-web.md)。
- 已變更 [所有資源] 頁面 UI。
- 已修正 Web 用戶端傳回 [一般通訊協定錯誤] 的數個連線順序失敗。
- 已修正某些特定按鍵順序未適當處理的鍵盤輸入問題。
- 協助工具改善。

## <a name="updates-for-version-10210"></a>1\.0.21.0 版的更新
*發行日期：11/15/2019*

- 已新增在遠端工作階段中使用輸入法編輯器 (IME) 來輸入複雜字元的支援。
- 已修正使用者無法複製並貼到 macOS 裝置上遠端會話的迴歸問題。
- 已修正將本機 Windows 金鑰傳送至 Firefox 上遠端工作階段的迴歸問題。
- 已新增 RDWeb 密碼變更的連結，由您的系統管理員啟用。

## <a name="updates-for-version-10200"></a>1\.0.20.0 版的更新
*發行日期：2019/10/18*

- 已新增對 Windows 7 和 Windows Server 2008 R2 主機連線的支援。
- 已修正某些應用程式圖示顯示為透明磚的問題。
- 已修正 Windows 7 上 Internet Explorer 瀏覽器的連線問題。
- 已修正調整瀏覽器大小時，發生非預期中斷連線的問題。
- 協助工具改善。
- 已更新協力廠商程式庫。

## <a name="updates-for-version-10180"></a>1\.0.18.0 版的更新
*發行日期：5/14/2019*

- 已在 [設定] 索引標籤中新增 [資源啟動方法] 設定，讓使用者在瀏覽器中開啟資源，或下載要使用另一個用戶端處理的 .rdp 檔案。 此設定可以由您的系統管理員設定。關於這項功能的系統管理員設定的詳細資料，可以在 [Web 用戶端設定文件](remote-desktop-web-client-admin.md)中找到。
- 已修正色彩呈現問題，可讓遠端工作階段中的色彩更鮮明。
- 已修改與遠端資源摘要錯誤相關的錯誤訊息。
- 已新增對更多 Office 快速鍵的支援，例如選擇性貼上 (Ctrl+Alt+V)。
- 已新增鍵盤快速鍵，讓使用者在遠端工作階段中叫用 Windows 鍵 (Alt+F3)
- 已更新使用者嘗試使用過期密碼進行驗證的錯誤訊息。
- 已重新整理 [所有資源] 頁面上的摘要 UI。
- 已解決工作階段重新連線期間發生的對話方塊重疊問題。
- 已修正資源工作列中的遠端資源圖示大小。

## <a name="updates-for-version-1011"></a>1\.0.11 版的更新
*發行日期：2/22/2019*

- 已在 Windows Server 2019 中啟用不含 RD 閘道的 RD 代理人連線。
- 依字母順序排序摘要 (亦即，RemoteApps 第一個、Desktops 第二個)。
- 已修正多個協助工具錯誤以改善螢幕助讀程式相容性。
- 已更新我們的建置工具。
- 多個錯誤修正。

## <a name="updates-for-version-107"></a>1\.0.7 版的更新
*發行日期：1/24/2019*

- 現在支援在內部網路離線使用。
- 已改善在非 Microsoft Edge 瀏覽器上的呈現問題。
- 已實作摘要擷取重試嘗試次數的限制以防止 DoS。
- 已修正協助工具錯誤，讓視障使用者使用 Web 用戶端。
- 已改進對使用者顯示的摘要錯誤的錯誤訊息。
- 已新增 Ctrl + Alt + End (Windows) 和 fn + control + option + delete (Mac) 的捷徑來叫用遠端電腦中的 Ctrl + Alt + Del。
- 已改進當機事件的遙測。
- 已改進我們的組建管線和建置工具。
- 多個錯誤修正。

## <a name="updates-for-version-101"></a>1\.0.1 版的更新
*發行日期：10/29/2018*

- 已在 [關於] 頁面上新增 [擷取支援資訊]  的選項來診斷問題。
- 現在支援 inPrivate 模式。
- 已改進對非英文鍵盤的支援。
- 已修正非英文字元的工具提示顯示不正確的問題。
- 已修正影響 Chrome 使用者的圖形呈現問題。
- 已更新完整 DST 支援的時區重新導向。
- 已改進記憶體不足錯誤的錯誤訊息。
- 多個錯誤修正。

## <a name="updates-for-version-100"></a>1\.0.0 版的更新
*發行日期：07/16/2018*

- 現在已可廣泛使用遠端桌面 Web 用戶端。
- 系統管理員可以全域關閉 Web 用戶端的遙測。
- 多個錯誤修正。

## <a name="updates-for-version-090"></a>0\.9.0 版的更新
*發行日期：07/05/2018*

- Web 用戶端的全新登入體驗。
- 啟動桌面或應用程式連線 (單一登入) 時，不再提示輸入認證。
- 已將 Web 用戶端移至新的 URL：<https://server_FQDN/RDWeb/webclient/index.html>
- 已新增時區重新導向。
- 多個錯誤修正。

## <a name="updates-for-version-081"></a>0\.8.1 版的更新
*發行日期：05/17/2018*

- 解決 CVE-2018-0886 中所述 CredSSP 加密 Oracle 修復的更新。
- 已修正啟用列印時，部分語言連線失敗的問題。
- 已改進閘道不是部署一部分時的錯誤訊息。
- 已新增 [說明]  和 [意見反應]  選項。

## <a name="updates-for-version-080"></a>0\.8.0 版的更新
*發行日期：03/28/2018*

- Web 用戶端最初的公開預覽版本。
- 使用 **CTRL+C** 和 **CTRL+V**，透過剪貼簿複製/貼上文字。
- 列印至 PDF 檔案。
- 進行 18 種語言的當地語系化。
