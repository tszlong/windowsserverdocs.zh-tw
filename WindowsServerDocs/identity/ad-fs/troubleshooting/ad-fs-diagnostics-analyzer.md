---
title: AD FS 說明診斷分析器
description: 本檔說明 AD FS 協助診斷分析器，以及它如何使用 AD FS Diagnostics PowerShell 模組來執行基本檢查。
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5d56a84680467359b68ae1ab115801d82a34c822
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407231"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>AD FS 說明診斷分析器

AD FS 有許多設定，可支援用於驗證和應用程式開發的各種功能。 在疑難排解期間，建議您確保所有的 AD FS 設定都已正確設定。 手動檢查這些設定有時可能會耗用很多時間。 AD FS Help Diagnostics Analyzer 可協助您使用 ADFSToolbox PowerShell 模組執行基本檢查。 執行檢查之後，AD FS 協助提供[診斷分析器](https://aka.ms/adfsdiagnosticsanalyzer)，協助您輕鬆地將結果視覺化，並提供補救步驟。

完成作業會採用3個簡單的步驟：

1. 在主要 AD FS 伺服器或 WAP 伺服器上設定 ADFSToolbox 模組
2. 執行診斷並上傳檔案以 AD FS 說明
3. 查看診斷分析並解決任何問題

請移至[AD FS 說明診斷分析器（ https://aka.ms/adfsdiagnosticsanalyzer)](https://aka.ms/adfsdiagnosticsanalyzer)以開始進行疑難排解。

![AD FS 說明上的 AD FS diagnostics analyzer 工具](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>步驟1：在 AD FS 伺服器上設定 ADFSToolbox 模組

若要執行[診斷分析器](https://aka.ms/adfsdiagnosticsanalyzer)，您必須安裝 ADFSToolbox PowerShell 模組。 如果 AD FS 伺服器可連線到網際網路，您可以直接從 PowerShell 資源庫安裝 ADFSToolbox 模組。 如果沒有網際網路連線，您可以手動安裝它。 

[!WARNING]
如果您使用 AD FS 2.1 或更低版本，您必須安裝 ADFSToolbox 版的1.0.13。 ADFSToolbox 不再于最新版本上支援2.1 或更低的 AD FS。

![AD FS 診斷分析程式-設定](media/ad-fs-diagonostics-analyzer/step1_v2.png)

### <a name="setup-using-powershell-gallery"></a>使用 PowerShell 資源庫進行安裝

如果 AD FS 伺服器具有網際網路連線能力，建議您使用下面提供的 PowerShell 命令，直接從 PowerShell 資源庫安裝 ADFSToolbox 模組。

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```

### <a name="setup-manually"></a>手動安裝

ADFSToolbox 模組必須手動複製到 AD FS 或 WAP 伺服器。 請依照下列指示進行。

1. 在可存取網際網路的電腦上啟動已提升許可權的 PowerShell 視窗。
2. 安裝 AD FS 工具箱 模組。

    ```powershell
    Install-Module -Name ADFSToolbox -Force
    ```
3. 將位於本機電腦 `%SYSTEMDRIVE%\Program Files\WindowsPowerShell\Modules\` 的 ADFSToolbox 資料夾複製到 AD FS 或 WAP 電腦上的相同位置。

4. 在 AD FS 機上啟動已提升許可權的 PowerShell 視窗，然後執行下列 Cmdlet 以匯入模組。

    ```powershell
    Import-Module ADFSToolbox -Force
    ```

## <a name="step-2-execute-the-diagnostics-cmdlet"></a>步驟2：執行診斷 Cmdlet

![AD FS 診斷分析器工具-執行和上傳結果](media/ad-fs-diagonostics-analyzer/step2_v2.png)

您可以使用單一命令，輕鬆地在伺服器陣列中的所有 AD FS 伺服器上執行診斷測試。 PowerShell 模組會使用遠端 PS 會話，在伺服器陣列中的不同伺服器上執行診斷測試。

```powershell
    Export-AdfsDiagnosticsFile [-ServerNames <list of servers>]
```

在伺服器2016或更新版本 AD FS 伺服器陣列中，此命令將會從 AD FS 設定讀取 AD FS 伺服器清單。 然後會嘗試對清單中的每部伺服器進行診斷測試。 如果沒有可用的 AD FS 伺服器清單（例如2012R2），則會對本機電腦執行測試。 若要指定要執行測試的伺服器清單，請使用**ServerNames**引數來提供伺服器清單。 以下提供範例

```powershell
    Export-AdfsDiagnosticsFile -ServerNames @("adfs1.contoso.com", "adfs2.contoso.com")
```

結果是在命令執行所在的相同目錄中建立的 JSON 檔案。 檔案的名稱是 AdfsDiagnosticsFile-\<timestamp\>。 範例檔案名為 AdfsDiagnosticsFile-07312019-184201. json。

## <a name="step-3-upload-the-diagnostics-file"></a>步驟3：上傳診斷檔案

在步驟3的[https://aka.ms/adfsdiagnosticsanalyzer](https://aka.ms/adfsdiagnosticsanalyzer)使用檔案瀏覽器來選取要上傳的結果檔案。

按一下 [上**傳**] 以完成上傳。

藉由使用 Microsoft 帳戶登入，您的診斷結果可以儲存以供稍後的流覽點，並可傳送給 Microsoft 支援服務。 如果您在任何時間點開啟支援案例，Microsoft 將能夠查看診斷分析器的結果，並協助您更快速地對問題進行疑難排解。

![AD FS 診斷分析程式工具-登入](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-4-view-diagnostics-analysis-and-resolve-any-issues"></a>步驟4：查看診斷分析並解決任何問題

測試結果有五個區段：

1. Failed：此區段包含失敗的測試清單。 每個結果包含：
2. 警告：此區段包含已產生警告的測試清單。 這些問題不太可能會導致以更廣泛的方式進行驗證的任何問題，但應該儘早解決。
3. 已通過：此區段包含通過的測試清單，而且沒有使用者的動作專案。
4. 未執行：此區段包含因遺漏資訊而無法執行的測試清單。
5. 不適用：此區段包含未執行的測試清單，因為它們不適用於命令執行所在的特定伺服器。

![AD FS 診斷分析器工具-測試結果](media/ad-fs-diagonostics-analyzer/step3a_v3.png) 清單會顯示每個測試結果的詳細資料，其中會說明測試和解決步驟：

1. 測試名稱：已執行之測試的名稱
2. 描述：測試的描述。
3. 詳細資料：在測試期間執行之整體作業的描述
4. 解決步驟：解決測試所選問題的建議步驟

![AD FS 診斷分析器工具-失敗解決](media/ad-fs-diagonostics-analyzer/step3b_v3.png)

## <a name="next"></a>下一則

- [使用 AD FS Help troublehshooting 指南](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
