---
title: AD FS 說明診斷分析器
description: 本文件說明 AD FS 協助診斷分析器，它可以執行基本的方式檢查使用 AD FS 診斷 PowerShell 模組。
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8d9acd1adcb8d9566b154abfef940e21609a6684
ms.sourcegitcommit: 4ff3d00df3148e4bea08056cea9f1c3b52086e5d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2019
ms.locfileid: "64773379"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>AD FS 說明診斷分析器

AD FS 具有許多功能，它提供用於驗證與應用程式開發的廣泛的設定。 在進行疑難排解，建議您使用確定所有的 AD FS 設定已正確設定。 手動檢查這些設定可以有時會很費時。 AD FS 協助診斷分析器可以協助您執行使用 ADFSToolbox PowerShell 模組的基本檢查。 後執行檢查，提供 AD FS 說明[診斷分析器](https://aka.ms/adfsdiagnosticsanalyzer)可協助您輕鬆地將結果視覺化，並提供補救步驟。

完成的作業會採用 3 個簡單步驟：

1. 安裝程式在主要 AD FS 伺服器或 WAP 伺服器上的 ADFSToolbox 模組
2. 執行診斷，並將檔案上傳至 AD FS 說明
3. 檢視診斷的分析，並解決任何問題

移至[AD FS 協助診斷分析器 (https://aka.ms/adfsdiagnosticsanalyzer) ](https://aka.ms/adfsdiagnosticsanalyzer)著手進行疑難排解。

![在 AD FS 說明 AD FS 診斷分析器工具](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>步驟 1：安裝 AD FS 伺服器上的 ADFSToolbox 模組

若要執行[診斷分析器](https://aka.ms/adfsdiagnosticsanalyzer)，您必須先安裝 ADFSToolbox PowerShell 模組。 如果 AD FS 伺服器已連線到網際網路，您就可以直接從 PowerShell gallery 安裝 ADFSToolbox 模組。 如果沒有連線到網際網路，複製 GitHub 存放庫手動安裝。

![AD FS 診斷分析器-安裝程式](media/ad-fs-diagonostics-analyzer/step1.png)

### <a name="setup-using-powershell-gallery"></a>使用 PowerShell 資源庫安裝

如果 AD FS 伺服器有網際網路連線，建議您使用安裝 ADFSToolbox 模組直接從 PowerShell 資源庫使用下列 PowerShell 命令。

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```
### <a name="setup-manually-by-cloning-the-repository"></a>手動複製存放庫設定

ADFSToolbox 模組可以安裝以手動方式從 GitHub 直接。 遵循以下指示複製存放庫和 AD FS 伺服器上安裝 ADFSToolbox 模組。

1. 下載[存放庫](https://github.com/Microsoft/adfsToolbox/archive/master.zip)
2. 解壓縮下載的檔案，並將 adfsToolbox master 資料夾複製到 %SYSTEMDRIVE%\\Program Files\\WindowsPowerShell\\模組\\。
3. 匯入 PowerShell 模組。 在提升權限的 PowerShell 視窗中執行下列命令：

   ```powershell
    Import-Module ADFSToolbox -Force
   ```

## <a name="step-2-execute-the-diagnostics-and-upload-the-file-to-ad-fs-help"></a>步驟 2：執行診斷，並將檔案上傳至 AD FS 說明

單一命令可用來輕鬆地執行診斷測試，跨伺服器陣列中的所有 AD FS 伺服器。 PowerShell 模組會使用遠端 PS 工作階段來執行診斷測試，跨伺服器陣列中的不同伺服器。

```powershell
    Export-AdfsDiagnosticsFile [-adfsServers <list of servers>]
```

在 Server 2016 AD FS 陣列中，此命令會從 AD FS 設定讀取 AD FS 伺服器的清單。 診斷測試，則會嘗試針對清單中的每一部伺服器。 如果清單中的 AD FS 伺服器就無法使用 (範例 2012R2)，則測試會針對本機電腦執行。 若要指定對其測試要執行的伺服器清單，請使用**adfsServers**引數提供的伺服器清單。 以下提供範例

```powershell
    Export-AdfsDiagnosticsFile -adfsServers @("sts1.contoso.com", "sts2.contoso.com", "sts3.contoso.com")
```

結果是 JSON 檔案建立在相同的目錄執行命令。 檔案的名稱是 ADFSDiagnosticsFile\<時間戳記\>。 範例檔案名稱是 ADFSDiagnosticsFile 20180716124030。

在步驟 2 [ https://aka.ms/adfsdiagnosticsanalyzer ](https://aka.ms/adfsdiagnosticsanalyzer)使用檔案瀏覽器，以選取要上傳結果檔案。

![AD FS 診斷分析器工具-執行，並將結果上傳](media/ad-fs-diagonostics-analyzer/step2.png)

按一下 **上傳**完成上傳，然後移至下一個步驟。


透過 Microsoft 帳戶登入，您的診斷結果可以儲存以供稍後檢視，以及可以傳送給 Microsoft 支援。 如果在您開啟支援案例的任何時間點，Microsoft 將能夠檢視診斷分析器結果，並協助更快將您的問題進行疑難排解。

![AD FS 診斷分析器工具登入](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-3-view-diagnostics-analysis-and-resolve-any-issues"></a>步驟 3：檢視診斷的分析，並解決任何問題

有四個區段的測試結果：

1. 失敗：本章節包含失敗的測試清單。 每個結果包含：
2. 警告：本節包含已產生警告的測試的清單。 這些問題將會不太可能導致任何問題更廣泛的標尺上的驗證，但應該盡早解決。
3. 不適用：本節包含已未執行，因為它們不是適用於特定的伺服器執行命令的測試清單。
4. 傳遞：本節包含測試傳遞，且有使用者動作項目清單。

![AD FS 診斷分析器工具-測試結果 清單](media/ad-fs-diagonostics-analyzer/step3a_v2.png)每項測試結果會顯示說明的測試和解決方式步驟的詳細資料：

1. 測試名稱：已執行的測試名稱
2. 詳細資料：在測試期間執行整體作業的描述
3. 解決步驟：建議的步驟來解決此問題，測試所反白顯示

![AD FS 診斷分析器工具失敗解決方式](media/ad-fs-diagonostics-analyzer/step3b_v2.png)

## <a name="next"></a>下一個

- [使用 AD FS 說明 troublehshooting 輔助線](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
