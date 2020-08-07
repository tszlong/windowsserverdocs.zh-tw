---
title: msdt
description: Msdt 命令的參考文章，它會在命令列中叫用疑難排解套件或做為自動化腳本的一部分，並在沒有使用者輸入的情況下啟用其他選項。
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8ef856cd54b93c77d4e260a5e433c67407d9611
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886204"
---
# <a name="msdt"></a>msdt

在命令列或自動腳本的過程中叫用疑難排解套件，並啟用其他選項而不需要使用者輸入。

## <a name="syntax"></a>語法

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /id`<packagename>` | 指定要執行的診斷封裝。 如需可用套件的清單，請參閱[可用的疑難排解套件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs)。 |
| /path`<directory|.diagpkg file|.diagcfg file>` | 指定診斷封裝的完整路徑。 如果您指定目錄，目錄必須包含診斷套件。 您不能將 **/path**參數與 * */id * *、 **/dci**或 **/cab**參數搭配使用。 |                                                                                   |
| /dci`<passkey>` | 會預先填入 [密碼] 欄位。 只有在支援提供者提供通行金鑰時，才會使用這個參數。 |
| /dt`<directory>` | 在指定的目錄中顯示疑難排解記錄。 診斷結果會儲存在使用者的 **%LOCALAPPDATA%\Diagnostics**或 **%LOCALAPPDATA%\ElevatedDiagnostics**目錄中。 |
| /af`<answerfile>` | 指定 XML 格式的回應檔案，其中包含一個或多個診斷互動的回應。 |
| /modal`<ownerHWND>` | 讓疑難排解套件強制回應成為父主控台視窗所指定的視窗，並以 decimal 處理 (HWND) 。 此參數通常是由啟動疑難排解套件的應用程式所使用。 如需取得主控台視窗控制碼的詳細資訊，請參閱[如何取得主控台視窗控制碼 (HWND) ](https://support.microsoft.com/help/124103/how-to-obtain-a-console-window-handle-hwnd)。 |
| /moreoptions`<true|false>` | 啟用 (true) 或抑制最終的疑難排解畫面，) 詢問使用者是否想要探索其他選項的 (false。 當疑難排解程式是由不屬於作業系統的疑難排解工具啟動時，通常會使用此參數。 |
| /param returns`<parameters>` | 在命令列指定一組互動回應，類似于回應檔案。 此參數通常不會用於使用 TSP 設計工具所建立之疑難排解套件的內容中。 如需開發自訂參數的詳細資訊，請參閱[Windows 疑難排解平台](/previous-versions/windows/desktop/wintt/windows-troubleshooting-toolkit-portal)。 |
| /advanced | 啟動疑難排解套件時，預設會展開 [歡迎使用] 頁面上的 [advanced] 連結。 |
| /custom | 提示使用者在套用前，先確認每個可能的解決方式。 |

### <a name="return-codes"></a>傳回碼

疑難排解套件包含一組根本原因，其中每一個都會描述特定的技術問題。 完成疑難排解套件工作之後，每個根本原因都會傳回「固定」狀態、「未修正」、偵測到 (，但無法修復) 或找不到。 除了疑難排解員使用者介面中報告的特定結果之外，疑難排解引擎也會傳回結果中的程式碼，其中描述的是疑難排解員是否已修正原始問題。 程式碼為：

| 程式碼 | 描述 |
| ---- | ----------- |
| -1 | **中斷：** 疑難排解工作完成之前，已關閉疑難排解員。 |
| 0 | 已**修正：** 疑難排解員已找出並修正至少一個根本原因，而且根本原因不會保留在固定狀態。 |
| 1 | **存在，但不是固定的：** 疑難排解員已識別出一個或多個根本原因，而此問題仍處於非固定狀態。 即使已修正另一個根本原因，也會傳回此程式碼。 |
| 2 | **找不到：** 疑難排解員並未識別任何根本原因。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [可用的疑難排解套件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs)

- [TroubleshootingPack Powershell 參考](/powershell/module/troubleshootingpack/?view=win10-ps)
