---
title: KMS 啟用的已知問題
description: 說明在 KMS 啟用過程中可能發生的常見問題，並提供解決方式和指引
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 3446ad0954510d8c96e9a2d361f24c90d325b782
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826251"
---
# <a name="kms-activation-known-issues"></a>KMS 啟用：已知問題

本文說明可能在金鑰管理服務 (KMS) 啟用期間產生的常見問題，並提供可供解決問題的指引。

> [!NOTE]
> 如果您懷疑您的問題與 DNS 相關，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

## <a name="should-i-back-up-kms-host-information"></a>我應該備份 KMS 主機資訊嗎？

KMS 主機不需要備份。 不過，如果您使用工具來定期清除事件記錄檔，則存放在記錄檔中的啟用歷程記錄可能會遺失。 如果您使用事件記錄檔來追蹤或記載 KMS 啟用，請從事件檢視器的 [應用程式和服務記錄檔] 資料夾定期匯出金鑰管理服務事件記錄檔。

如果您使用 System Center Operations Manager，System Center 資料倉儲資料庫會存放事件記錄檔資料以供報告，因此您不必另外備份事件記錄檔。

## <a name="is-the-kms-client-computer-activated"></a>KMS 用戶端電腦是否已啟用？

在 KMS 用戶端電腦上，開啟 [系統]  控制台，然後尋找 [Windows 已啟用]  訊息。 或者，執行 Slmgr.vbs 並使用 **/dli** 命令列選項。

## <a name="the-kms-client-computer-does-not-activate"></a>KMS 用戶端電腦並未啟用

確認已符合 KMS 啟用閾值。 在 KMS 主機電腦上，執行 Slmgr.vbs 並使用 **/dli**命令列選項來判斷主機的目前計數。 直到 KMS 主機的計數為 25 時，才能啟用 Windows 7 用戶端電腦。 Windows Server 2008 R2 KMS 用戶端的 KMS 計數必須為 5 才能啟用。 如需 KMS 需求的詳細資訊，請參閱[大量啟用規劃指南](https://go.microsoft.com/fwlink/?linkid=155926)。 

在 KMS 用戶端電腦上，查看應用程式事件記錄檔中的事件識別碼 12289。 檢查此事件以取得下列資訊：

- 結果碼為 **0**？ 除此之外都是錯誤。
- 事件中的 KMS 主機名稱是否正確？
- KMS 連接埠是否正確？
- KMS 主機是否可存取？
- 如果用戶端正在執行非 Microsoft 防火牆，是否必須設定輸出連接埠？

在 KMS 主機電腦上，查看 KMS 事件記錄檔中的事件識別碼 12290。 檢查此事件以取得下列資訊：

- KMS 主機是否記錄來自用戶端電腦的要求？ 確認已列出 KMS 用戶端電腦的名稱。 確認用戶端和 KMS 主機可以通訊。 用戶端是否收到回應？
- 如果未記錄來自 KMS 用戶端的任何事件，則要求不會送達 KMS 主機，或 KMS 主機無法處理它。 請確定路由器不會封鎖使用 TCP 連接埠 1688 的流量 (如果使用預設連接埠)，而且允許 KMS 用戶端的具狀態流量。

## <a name="what-does-this-error-code-mean"></a>此錯誤碼的意義為何？

除了具有事件識別碼 12290 的 KMS 事件以外，Windows 會將所有啟用事件記錄到事件提供者名稱 Microsoft-Windows-Security-SPP 之下的應用程式事件記錄檔。 Windows 會將 KMS 事件記錄到 [應用程式和服務] 資料夾中的金鑰管理服務記錄檔。 IT 專業人員可以執行 Slui.exe 來顯示大部分啟用相關錯誤碼的說明。 此命令的一般語法如下：

```cmd
slui.exe 0x2a ErrorCode
```

例如，如果事件識別碼 12293 包含錯誤碼 0x8007267C，您可以執行下列命令來顯示該錯誤的說明：

```cmd
slui.exe 0x2a 0x8007267C
```

如需特定錯誤碼和如何予以解決的詳細資訊，請參閱[解決常見啟用錯誤碼](activation-error-codes.md)。

## <a name="clients-are-not-adding-to-the-kms-count"></a>用戶端並未新增至 KMS 計數

若要重設用戶端電腦識別碼 (CMID) 和其他產品啟用資訊，請執行 **sysprep /generalize** 或 **slmgr /rearm**。 否則，每部用戶端電腦看起來完全相同，而且 KMS 主機不會將它們計為個別的 KMS 用戶端。

## <a name="kms-hosts-are-unable-to-create-srv-records"></a>KMS 主機無法建立 SRV 記錄

網域名稱系統 (DNS) 可能會限制寫入權限，或可能不支援動態 DNS (DDNS)。 在此情況下，請為 KMS 主機授與 DNS 資料庫的寫入權限，或手動建立服務 (SRV) 資源記錄 (RR)。 如需 KMS 和 DNS 問題的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

## <a name="only-the-first-kms-host-is-able-to-create-srv-records"></a>只有第一部 KMS 主機能夠建立 SRV 記錄

如果組織有一部以上的 KMS 主機，除非變更 SRV 預設權限，否則其他主機可能無法更新 SRV RR。 如需 KMS 和 DNS 問題的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

## <a name="i-installed-a-kms-key-on-the-kms-client"></a>我已在 KMS 用戶端上安裝 KMS 金鑰

KMS 金鑰只應該安裝在 KMS 主機上，而不是在 KMS 用戶端上。 執行 **slmgr.vbs -ipk &lt;SetupKey&gt;** 。 如需可用於將電腦設定為 KMS 用戶端的金鑰表格，請參閱 [KMS 用戶端安裝程式金鑰](KMSclientkeys.md)。 這些金鑰是公開已知的，而且是版本專屬。 請記得刪除 DNS 中任何不必要的 SRV RR，然後重新啟動電腦。

## <a name="a-kms-host-failed"></a>KMS 主機失敗

如果 KMS 主機失敗，您必須在新主機上安裝 KMS 主機金鑰，然後啟動主機。 請確定新的 KMS 主機在 DNS 資料庫中具有 SRV RR。 如果您使用與失敗 KMS 主機相同的電腦名稱和 IP 位址來安裝新的 KMS 主機，新的 KMS 主機可以使用失敗主機的 DNS SRV 記錄。 如果新主機具有不同的電腦名稱稱，您可以手動移除失敗主機的 DNS SRV RR，或者 (如果 DNS 中已啟用清除) 讓 DNS 自動移除它。 如果網路使用 DDNS，新的 KMS 主機會在 DNS 伺服器上自動建立新的 SRV RR。 新的 KMS 主機接著會開始收集用戶端更新要求，並在符合 KMS 啟用閾值時立即開始啟用用戶端。

如果 KMS 用戶端使用自動探索，則會在原始 KMS 主機未回應更新要求時，自動選取另一個 KMS 主機。 如果用戶端未使用自動探索，您必須執行 **slmgr.vbs /skms**，手動更新指派給失敗 KMS 主機的 KMS 用戶端電腦。 若要避免這種情況，請將 KMS 用戶端設定為使用自動探索。 如需相關資訊，請參閱[大量啟用部署指南](https://go.microsoft.com/fwlink/?linkid=150083)。
