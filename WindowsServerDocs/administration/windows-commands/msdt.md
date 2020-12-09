---
title: msdt
description: Msdt 命令的參考文章，此命令會在命令列上叫用疑難排解套件或作為自動化腳本的一部分，並在不需要使用者輸入的情況下啟用其他選項。
ms.topic: reference
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0270039fcbc9f99ff2569635ddd75918baed783e
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865407"
---
# <a name="msdt"></a>msdt

在命令列或自動腳本的一部分叫用疑難排解套件，並在不需要使用者輸入的情況下啟用其他選項。

## <a name="syntax"></a>語法

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /id `<packagename>` | 指定要執行的診斷封裝。 如需可用封裝的清單，請參閱 [可用的疑難排解套件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs)。 |
| /path `<directory|.diagpkg file|.diagcfg file>` | 指定診斷封裝的完整路徑。 如果您指定目錄，目錄必須包含診斷套件。 您無法搭配 * */id * *、 **/dci** 或 **/cab** 參數使用 **/path** 參數。 |                                                                                   |
| /dci `<passkey>` | 會預先填入通行金鑰欄位。 只有當支援提供者提供密碼金鑰時，才會使用此參數。 |
| /dt `<directory>` | 顯示指定目錄中的疑難排解記錄。 診斷結果會儲存在使用者的 **%LOCALAPPDATA%\Diagnostics** 或 **%LOCALAPPDATA%\ElevatedDiagnostics** 目錄中。 |
| /af `<answerfile>` | 指定 XML 格式的回應檔案，其中包含一或多個診斷互動的回應。 |
| /modal `<ownerHWND>` | 使疑難排解套件強制回應父主控台視窗所指定的視窗，以十進位的 (HWND) 來處理。 啟動疑難排解套件的應用程式通常會使用此參數。 如需取得主控台視窗控制碼的詳細資訊，請參閱 [如何取得主控台視窗控制碼 (HWND) ](https://support.microsoft.com/help/124103/how-to-obtain-a-console-window-handle-hwnd)。 |
| /moreoptions `<true|false>` | 啟用 (true) 或隱藏 (false) 會詢問使用者是否想要探索其他選項的最後一個疑難排解畫面。 此參數通常會在疑難排解套件由不屬於作業系統一部分的疑難排解工具啟動時使用。 |
| /param `<parameters>` | 在命令列指定一組互動回應，類似于回應檔案。 此參數通常不會用於以 TSP 設計工具建立的疑難排解套件內容中。 如需有關開發自訂參數的詳細資訊，請參閱 [Windows 疑難排解平臺](/previous-versions/windows/desktop/wintt/windows-troubleshooting-toolkit-portal)。 |
| /advanced | 啟動疑難排解套件時，依預設會展開 [歡迎使用] 頁面上的 [advanced] 連結。 |
| /custom | 提示使用者在套用之前先確認每個可能的解決方法。 |

### <a name="return-codes"></a>傳回碼

疑難排解套件包含一組根本原因，其中每一個都描述特定的技術問題。 完成疑難排解套件工作之後，每個根本原因都會傳回固定、未修正、偵測到 (但無法修復) 或找不到的狀態。 除了疑難排解員使用者介面中所報告的特定結果之外，疑難排解引擎也會傳回結果中的程式碼，其中描述的結果通常是因為疑難排解員是否已修正原始問題。 程式碼為：

| 程式碼 | 描述 |
| ---- | ----------- |
| -1 | **中斷：** 疑難排解程式在疑難排解工作完成之前已關閉。 |
| 0 | 已 **修正：** 疑難排解員已識別並修正至少一個根本原因，而且沒有任何根本原因會維持不固定狀態。 |
| 1 | **存在但未修正：** 疑難排解員找出一個或多個保持不固定狀態的根本原因。 即使已修正另一個根本原因，也會傳回此程式碼。 |
| 2 | **找不到：** 疑難排解員未找出任何根本原因。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [可用的疑難排解套件](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs)

- [TroubleshootingPack Powershell 參考](/powershell/module/troubleshootingpack/)
