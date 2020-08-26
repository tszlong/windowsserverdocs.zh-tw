---
title: Windows 桌面用戶端的新功能
description: 了解適用於 Windows 桌面的遠端桌面用戶端最新變更
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 08/20/2020
ms.localizationpriority: medium
ms.openlocfilehash: b23c19304aa7773dbb3c4e1406e065fb68947c5d
ms.sourcegitcommit: 8e5530ba7f7d3e2569590949e1f443d908683a17
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2020
ms.locfileid: "88702837"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Windows 桌面用戶端的新功能

您可以在[開始使用 Windows 桌面用戶端](windowsdesktop.md)找到更多有關 Windows 桌面用戶端的詳細資訊。 您可以在下方找到最新的用戶端更新。

## <a name="latest-client-versions"></a>最新的用戶端版本

用戶端可以針對不同的[使用者群組](windowsdesktop-admin.md#configure-user-groups)進行設定。 下表列出每個使用者群組可用的目前版本：

|使用者群組 |版本  |
|-----------|---------|
|公用     |1.2.1186 |
|Insider    |1.2.1272 |

## <a name="updates-for-version-121272-insider"></a>1\.2.1272 版的更新 [INSIDER]

*發行日期：2020/8/11*

下載：[Windows 64 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4D7LK)、[Windows 32 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4D5aF)、[Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4Dan3)

- 已新增從使用者身分識別自動偵測主權雲端的功能。
- 已新增為所有使用者啟用自訂 URL 訂用帳戶的功能。
- 已修正摘要工作列上應用程式釘選的問題。
- 已修正訂閱 URL 時會損毀的問題。
- 改善使用觸控或手寫筆拖曳遠端應用程式視窗時的體驗。

## <a name="updates-for-version-121186"></a>1\.2.1186 版的更新

*發行日期：2020/07/28*

下載：[Windows 64 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4C7Qy)、[Windows 32 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4Ciex)、[Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4C7Qx)

- 您現在可以使用用戶端頂端命令列上的溢位功能表 ([ **...** ]) 選項，訂閱具有多個使用者帳戶的工作區。 為了區分工作區，工作區標題現在包含使用者名稱，如同所有應用程式捷徑標題一樣。
- 將其他資訊新增至訂用帳戶錯誤訊息，以改善疑難排解功能。
- 現在在重新整理期間，會保留工作區的摺疊/展開狀態。
- 將 [**傳送診斷並關閉**] 按鈕，新增至 [**連線資訊**] 對話方塊中。
- 已修正遠端工作階段中的 CTRL + SHIFT 鍵問題。

## <a name="updates-for-version-121104"></a>1\.2.1104 版的更新

*發行日期：06/23/2020*

下載：[Windows 64 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4zeHS)、[Windows 32 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4zrAd)、[Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4zeHR)

- 已更新 [訂閱] 選項的自動探索邏輯，以支援 Windows 虛擬桌面的 Azure Resource Manager 整合版本。 只有 Windows 虛擬桌面資源的客戶，應不再需要同意 Windows 虛擬桌面 (傳統)。
- 已改善對高 DPI 裝置的支援，縮放比例最高可達 400%。
- 已修正未顯示中斷連線對話方塊的問題。
- 已修正命令列工具提示的顯示期間超出預期的問題。
- 已修正您嘗試在重新整理之後立即訂閱時發生當機的問題。
- 已修正在某些語言中無法正確剖析日期和時間所導致的當機。

## <a name="updates-for-version-121026"></a>1\.2.1026 版的更新

*發行日期：2020/05/27*

下載：[Windows 64 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4xsGB)、[Windows 32 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4xd8P)、[Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4xq7m)

- 訂閱時，您現在可以選擇您的帳戶，而不用輸入電子郵件地址。
- 新增一個新的**使用 URL 訂閱**選項，您可以使用該選項指定要訂閱的工作區 URL，或當無法自動找到您的資源時利用[電子郵件探索](../rds-email-discovery.md) (如果有的話)。 這類似於其他遠程桌面用戶端中的訂用帳戶處理程序。 這可以用來直接訂閱 Windows 虛擬桌面工作區。
- 新增了對使用新的 [URI 配置](remote-desktop-uri.md)來訂閱工作區的支援，其可透過電子郵件傳送給使用者或新增至支援網站。
- 已新增新的 [連線資訊] 對話方塊，以提供桌面和應用程式工作階段的用戶端、網路和伺服器詳細資料。 在全螢幕模式下，您可以從連線列存取此對話方塊，而在視窗模式下，則可以從 [系統] 功能表存取。
- 在視窗模式下啟動的桌面工作階段現在一律會最大化，而不是在視窗最大化時進入全螢幕模式。 使用 [系統] 功能表中的 [全螢幕] 選項，即可進入全螢幕模式。
- 取消訂閱提示現在會顯示警告圖示，並以項目符號清單的方式顯示工作區名稱。
- 已在其他錯誤對話方塊中新增 [詳細資料] 區段，以協助診斷問題。
- 已在錯誤對話方塊的 [詳細資料] 區段中新增時間戳記。
- 已修正下列問題：RDP 檔案設定 **desktop size id** 無法正常運作。
- 已修正下列問題：在啟動工作階段之後未套用 [調整大小時更新解析度] 顯示器設定。
- 已修正 [桌面設定] 面板中的當地語系化問題。
- 已修正在 [桌面設定] 面板上透過 Tab 鍵瀏覽控制項時的焦點方塊大小。
- 已修正會導致資源名稱在高對比模式下不易辨識的問題。
- 已修正會導致動作中心內的更新通知每天顯示多次的問題。

## <a name="updates-for-version-12945"></a>1\.2.945 版的更新

*發行日期：2020/04/28*

下載：[Windows 64 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4vhNM)、[Windows 32 位元](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4vhNO)、[Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4vuSV)

- 當您以滑鼠右鍵按一下連線中心上的桌面圖示時，就可以新增桌面連線的 [顯示設定] 選項。
  - 現在有三個顯示組態選項：**所有顯示**、**單一顯示**，以及**選取 顯示**。
  - 我們現在只會在選取顯示組態時顯示可用的設定。
  - 在選取顯示模式中，新的 [將目前的顯示最大化] 選項可讓您以動態方式變更用於工作階段的顯示，而不需要重新連線。 啟用時，將工作階段最大化會讓工作階段視窗觸及的所有顯示上的畫面全滿。
  - 我們已在所有顯示的中新增新的**使用視窗型時單一顯示**選項，並選取顯示模式。 當您結束全螢幕模式時，這個選項會自動將您的工作階段切換為單一顯示，當您將視窗最大化時，會自動返回多個顯示。
- 我們已將新的**顯示設定**群組新增至系統功能表；以滑鼠右鍵按一下視窗型桌面工作階段的標題列時即會顯示。 這可讓您在工作階段期間動態變更一些設定。 例如，您可以變更新的**使用視窗型時單一顯示模式**和**將目前顯示最大化**設定。
- 結束全螢幕並第一次進入全螢幕時，工作階段視窗會回到其原始位置。
- 工作區的背景重新整理已變更為每四小時一次，而不是每小時一次。 現在會在啟動用戶端時自動重新整理。
- 從「關於」頁面重設使用者資料時，現在會在完成時將您重新導向至「連線中心」，而不是關閉用戶端。
- 桌面連線的 [系統] 功能表項目已重新排序，而且 [說明] 主題現在會指向用戶端文件。
- 使用 Tab 鍵瀏覽和螢幕讀取器解決了一些協助工具問題。
- 已修正下列問題：[Azure Active Directory 驗證] 對話方塊出現在工作階段視窗後面。
- 已修正在不同縮放比例的顯示之間拖曳桌面工作階段視窗時，發生閃爍和壓縮的問題。
- 已修正重新導向相機時發生的錯誤。
- 已修正多個損毀問題以改善可靠性。

## <a name="updates-for-version-12790"></a>1\.2.790 版的更新

*發行日期：2020/03/24*

- 為了與其他遠端桌面用戶端保持一致，已將工作區的「更新」動作重新命名為「重新整理」。
- 您現在可以直接從其操作功能表重新整理工作區。
- 立即手動重新整理工作區，可確保所有本機內容都會更新。
- 您現在可以從 [關於] 頁面重設用戶端的使用者資料，而不需要將應用程式解除安裝。
- 您也可以使用 msrdcw.exe /reset 搭配選用的 /f 參數來重設用戶端的使用者資料，以略過提示。
- 現在，當我們瀏覽至 [關於] 頁面時，會自動尋找用戶端更新。
- 更新了按鈕色彩以保持一致性。

## <a name="updates-for-version-12675"></a>1\.2.675 版的更新

*發行日期：02/25/2020*

- 如果 RDP 檔案遺漏簽章，或其中一個 signscope 屬性已修改，則現在會封鎖對 Windows 虛擬桌面的連線。
- 當工作區是空的或已移除時，連接中心不會再顯示為空白。
- 已在中斷連線的訊息上新增活動識別碼和錯誤碼，以改善疑難排解。 您可以使用 **Ctrl + C** 複製對話方塊訊息。
- 已修正導致桌面連線設定不會偵測到顯示的問題。
- 用戶端更新不會再自動重新啟動電腦。
- 無視窗圖示應該不會再出現在工作列上。

## <a name="updates-for-version-12605"></a>1\.2.605 版的更新

*發行日期：01/29/2020*

- 您現在可以選取要用於桌面連線的顯示器。 若要變更此設定，請在桌面連線的圖示上按一下滑鼠右鍵，然後選取 [設定]。
- 已修正連線設定未顯示正確可用的縮放比例問題。
- 已修正 [朗讀程式] 無法讀取連線起始時顯示的對話方塊問題。
- 已修正當 Azure Active Directory 和 Active Directory 名稱不相符時，所顯示錯誤使用者名稱的問題。
- 已修正在未連線到網路的情況下，在起始連線時使用戶端停止回應的問題。
- 已修正在連接耳機時導致用戶端停止回應的問題。

## <a name="updates-for-version-12535"></a>1\.2.535 版的更新

*發行日期：12/04/2019*

- 您現在可以直接從用戶端頂端命令列上的 [更多選項] 按鈕，存取有關更新的資訊。
- 您現在可以從用戶端的命令列報告意見反應。
- 現在，只有在意見反應中樞可供使用時，才會顯示 [意見反應] 選項。
- 透過原則停用通知時，確定不會顯示更新通知。
- 已修正某些 RDP 檔案無法啟動的問題。
- 已修正某些持續性設定損毀所導致的用戶端啟動當機。

## <a name="updates-for-version-12431"></a>1\.2.431 版的更新

*發行日期：2019/11/12*

- 現已推出 32 位元和 ARM64 版本的用戶端！
- 現在，用戶端會儲存您對連線列所做的任何變更 (例如其位置、大小和釘選狀態)，並將這些變更套用至工作階段。
- 更新的閘道資訊和連線狀態對話方塊。
- 已解決在 Azure Active Directory 權杖過期之後嘗試連線時，同時提示兩個認證的問題。
- 在 Windows 7 上，如果使用者在伺服器不允許其儲存認證時已儲存認證，則系統會正確提示他們認證。
- 重新連線時，Azure Active Directory 提示現在會出現在連線視窗的前方。
- 釘選到工作列的項目現在會在摘要重新整理期間更新。
- 改善使用觸控時在連線中心的捲動功能。
- 從解決方法下拉式功能表中移除空白行。
- 已從 Windows 認證管理員中移除不必要的項目。
- 結束全螢幕時，桌面工作階段會適當調整大小。
- 當您在進入睡眠模式後恢復工作階段時，[RemoteApp 中斷連線] 對話方塊現在會在前景中顯示。
- 已解決協助工具問題，例如鍵盤瀏覽。

## <a name="updates-for-version-12247"></a>1\.2.247 版的更新

*發行日期：09/17/2019*

- 已改善當地語系化版本的遞補語言。 (例如，FR-CA 會正確地以法文顯示，而不是英文。)
- 移除訂用帳戶時，用戶端現在會正確地從認證管理員移除已儲存的認證。
- 用戶端更新程序現已在啟動後自動執行，且用戶端會在完成後重新啟動。
- 用戶端現在可以在 Windows 10 上以 S 模式使用。
- 已修正導致使用者名稱中有空格的使用者更新程序失敗的問題。
- 已修正在連線期間驗證時所發生的損毀。
- 已修正關閉用戶端時所發生的損毀。
