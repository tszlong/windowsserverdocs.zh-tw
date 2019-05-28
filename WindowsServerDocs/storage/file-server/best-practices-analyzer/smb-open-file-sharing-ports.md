---
Title: SMB:檔案及印表機共用連接埠應該開啟
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: d761e4532a5be92d43e09904e9df8f2aa61b6bb8
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63738468"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB:檔案及印表機共用連接埠應該開啟


更新日期：2011 年 2 月 2日日

適用於：Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中，Windows Server 2008 R2

*本主題旨在解決最佳做法分析程式掃描所識別的特定問題。您應該已經檔案服務 Best Practices Analyzer 對它們執行，且發生本主題提及之問題的電腦套用本主題中的資訊。如需最佳做法和掃描的詳細資訊，請參閱* [Best Practices Analyzer](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a)。


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>作業系統</strong></p></td>
<td><p>Windows Server</p></td>
</tr>
<tr class="even">
<td><p><strong>產品/功能</strong></p></td>
<td><p>檔案服務</p></td>
</tr>
<tr class="odd">
<td><p><strong>Severity</strong></p></td>
<td><p>錯誤</p></td>
</tr>
<tr class="even">
<td><p><strong>分類</strong></p></td>
<td><p>組態</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>問題

> *是必要的檔案及印表機共用的防火牆連接埠開啟 （連接埠 445 和 139）。*

## <a name="impact"></a>影響

> *電腦無法存取共用的資料夾及此伺服器上的其他伺服器訊息區 SMB 為基礎的網路服務。*

## <a name="resolution"></a>解析度

> *啟用檔案及印表機共用，透過電腦的防火牆進行通訊。*

您至少必須有 **Administrators** 群組的成員資格或同等權限，才能完成此程序。

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>開啟防火牆連接埠，若要啟用檔案及印表機共用

1.  開啟 [控制台] 中，按一下**系統及安全性**，然後按一下**Windows 防火牆**。

2.  在左窗格中，按一下**進階設定**，然後在主控台樹狀目錄中，按一下**輸入規則**。

3.  底下**輸入規則**，找出規則**檔案及印表機共用 (NB-工作階段-In)** 並**檔案及印表機共用 (Smb-in)** 。

4.  每個規則，該規則，以滑鼠右鍵按一下，然後按一下 **啟用規則**。

## <a name="additional-references"></a>其他參考資料

[了解共用資料夾與 Windows 防火牆](http://technet.microsoft.com/en-us/library/cc731402.aspx)(http://technet.microsoft.com/en-us/library/cc731402.aspx)

