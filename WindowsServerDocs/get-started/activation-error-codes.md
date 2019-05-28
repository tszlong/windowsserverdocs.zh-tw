---
title: 疑難排解啟用 」 錯誤碼
description: 了解如何疑難排解啟用 」 錯誤碼
ms.topic: article
ms.date: 03/21/2019
ms.technology: server-general
ms.assetid: ''
author: kaushika-msft
ms.author: kaushika-msft
ms.localizationpriority: medium
ms.openlocfilehash: 076fc3408810ba7b0f51c6a428955f99a66dd394
ms.sourcegitcommit: 7478dd3959d893bd42620e30e3b56f35407f1dec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2019
ms.locfileid: "64880879"
---
# <a name="troubleshooting-activation-error-codes"></a>疑難排解啟用 」 錯誤碼

**家庭使用者**這篇文章是用於支援的代理程式和 IT 專業人員。 如果您想要尋求 Windows 啟動錯誤訊息的詳細資訊，請移至下列 Microsoft 網站：

[取得 Windows 啟用錯誤的說明](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors) 

當您嘗試使用多重啟用金鑰 (MAK) 或金鑰管理服務 (KMS) 來執行大量啟用一或多個以 Windows 為基礎的電腦上時，您會收到錯誤訊息包含特定的錯誤碼。 這篇文章討論如何針對這些錯誤進行疑難排解。

**錯誤碼和描述**

|錯誤碼 |錯誤訊息 |啟用類型 |可能的原因 |疑難排解步驟 |
|-----------|--------------|----------------|---------------|----------------------|
|    0xC004C001    |    啟用伺服器判定所指定的產品金鑰無效    |    MAK    |    輸入的 MAK 無效。    |    請確認索引鍵是由 Microsoft 所提供的 MAK。 關於已封鎖的啟動問題，請連絡[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)     |
|    0xC004C003     |    啟用伺服器判定指定的產品金鑰被封鎖    |    MAK    |    該 MAK 在啟用伺服器上已被封鎖。    |    請連絡:[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)以取得新的 MAK 並安裝/啟用系統。    |
|    0xC004C008     |    啟用伺服器判定無法使用指定的產品金鑰。    |    KMS    |    該 KMS 金鑰已超過啟用限制。    |    KMS 主機金鑰，會啟動最多 10 倍六個不同的電腦上。 若需要啟用更多電腦，請連絡[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004B100    |    啟用伺服器判定電腦無法啟動。    |    MAK    |    如果該 MAK 是不受支援，可能會發生此問題。    |    若要疑難排解此問題，請確認已使用該 MAK 是 Microsoft 所提供的 MAK。 若要確認該 MAK 是有效，請連絡:[Microsoft 授權啟動中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C020     |    啟用伺服器報告多次啟用金鑰已超過其限制。    |    MAK    |    該 MAK 已超過啟用限制。    |    MAK 的設計有啟用次數限制。   請連絡[Microsoft 授權啟動中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C021     |    啟用伺服器報告已超過多次啟用金鑰延伸限制。    |    MAK    |    該 MAK 已超過啟用限制。    |    MAK 的設計有啟用次數限制。   請連絡[Microsoft 授權啟動中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)    |
|    0xC004F009     |    軟體保護服務報告寬限期已過期。    |    MAK    |    在寬限期到期之前在系統啟用。 現在，系統是處於「通知」狀態。    |    請參閱＜使用者經驗＞一節。    |
|    0xC004F00F     |    軟體授權伺服器報告的硬體識別碼繫結超出容錯層級。    |    MAK/KMS 用戶端/KMS 主機    |    變更硬體或驅動程式已更新系統上。    |    MAK：重新啟用系統在 OOT 寬限期內，使用線上或電話啟用。      KMS︰重新啟動，或執行 slmgr.vbs /ato。    |
|    0xC004F014     |    軟體保護服務報告不提供產品金鑰    |    MAK/KMS 用戶端    |    系統尚未安裝任何產品金鑰。    |    安裝 MAK 產品金鑰，或安裝位於安裝媒體上的 \sources\pid.txt KMS 安裝識別碼。    |
|    0xC004F02C     |    軟體保護服務報告離線啟用資料的格式不正確。    |    MAK/KMS 用戶端    |    系統偵測到在電話啟用期間輸入的資料無效。    |    確定輸入的 CID 是正確的。    |
|    0xC004F035     |    此錯誤的程式碼等同於 「 軟體保護服務報告電腦可能未啟動與大量授權產品金鑰...」錯誤文字是正確的但模稜兩可。 此錯誤表示此電腦缺少謅 BIOS – 提供 OEM 系統，以指出傳送具有合格版本的 Windows，也就是 KMS 用戶端啟動過程的需求的電腦上的 Windows 標記。 錯誤： 無效大量授權金鑰中的順序若要啟用，您需要將您的產品金鑰變更為有效的多重啟用金鑰 (MAK) 或零售金鑰。 您必須有合格的作業系統授權和大量授權 Windows 7 升級授權或完整授權適用於 Windows 7，從零售商。 任何其他安裝此軟體是違反您的合約及適用的著作權法。    |    KMS 用戶端 /KMS 主機    |    Windows 7 大量授權版本只能升級授權。 不支援沒有合格的作業系統安裝在電腦上安裝磁碟區的作業系統。    |    安裝的 Microsoft 作業系統，合格的版本，並接著使用 MAK 啟用。    |
|    0xC004F038     |    軟體保護服務報告電腦無法啟動。 報告由您的金鑰管理服務 (KMS) 的計數不足。 請連絡您的系統管理員。    |    KMS 用戶端    |    KMS 主機上的計數不足。 KMS 計數必須是 ≥5 適用於 Windows Server 或 Windows 用戶端 ≥25。    |    若要啟用的 KMS 用戶端的 KMS 集區中需要更多的電腦。 執行 Slmgr.vbs /dli 以取得 KMS 主機上的目前計數。    |
|    0xC004F039     |    軟體保護服務報告電腦無法啟動。 未啟用金鑰管理服務 (KMS)。    |    KMS 用戶端    |    當未回答的 KMS 要求時，就會發生此錯誤。    |    疑難排解 KMS 主機和用戶端之間的網路連線。 確定 TCP 連接埠 1688 （預設值） 並未被防火牆封鎖或過濾。    |
|    0xC004F041     |    軟體保護服務判定未啟用 金鑰管理伺服器 (KMS)。 必須啟用 KMS。    |    KMS 用戶端    |    KMS 主機未啟用。    |    啟用 KMS 主機使用線上或電話啟用。    |
|    0xC004F042     |    軟體保護服務判定，無法使用指定的金鑰管理服務 (KMS)。    |    KMS 用戶端    |    KMS 用戶端與 KMS 主機不符。    |    當 KMS 用戶端連絡無法啟用用戶端軟體的 KMS 主機時，就會發生此錯誤。 這可以是包含應用程式和作業系統特定 KMS 主機，例如的混合式環境中的常見項目。    |
|    0xC004F050     |    軟體保護服務報告指出產品金鑰無效。    |    KMS、KMS 用戶端、MAK    |    KMS 金鑰未正確，或輸入 Beta 機碼的作業系統的發行版本中，這可能被造成。    |    在對應的 Windows 版本上安裝適當的 KMS 金鑰。 檢查拼字。 如果機碼複製並貼上，確定長破折號沒有已取代索引鍵中連字號。    |
|    0xC004F051     |    軟體保護服務報告，將會封鎖的產品金鑰。    |    MAK/KMS    |    啟用伺服器上的產品金鑰已被 Microsoft 封鎖。    |    取得新的 MAK/KMS 金鑰、 將它安裝在系統上，並啟用。    |
|    0xC004F064     |    軟體保護服務報告非正版寬限期已過期。    |    MAK    |    判定此系統並非正版的 Windows 啟用工具 (WAT)。    |    請參閱大量啟用作業指南。    |
|    0xC004F065     |    軟體保護服務報告應用程式正在執行中有效的非正版週期。    |    MAK/KMS 用戶端    |    判定此系統並非正版的 Windows 啟用工具。 系統將會繼續執行非正版寬限期內。    |    取得和安裝正版產品金鑰，並在寬限期內啟用系統。 否則，系統將會進入結尾的寬限期內 「 通知 」 狀態。    |
|    0xC004F06C     |    軟體保護服務報告電腦無法啟動。 金鑰管理服務 (KMS) 判斷出要求時間戳記無效。    |    KMS 用戶端    |    用戶端電腦上的系統時間與 KMS 主機上的時間也不同。    |    時間同步處理很重要的原因有許多系統和網路安全性。 變更系統時間與 KMS 同步處理用戶端上的，以修正此問題。 建議使用的網路時間通訊協定 (NTP) 時間來源或時間同步處理 Active Directory 網域服務。 此問題於使用 UTP 時間與時區選擇無關。    |
|    0x80070005     |    拒絕存取。 要求的動作需要提高權限。    |    KMS 用戶端/MAK/KMS 主機    |    使用者帳戶控制 (UAC) 禁止啟用處理程序，從非提高權限的命令提示字元中執行。    |    從提升權限的命令提示字元執行 slmgr.vbs。   在 [cmd.exe] 上按一下滑鼠右鍵，然後按一下 [以系統管理員身分執行]。    |
|    0x8007232A     |    DNS 伺服器失敗。    |    KMS 主機    |    系統有網路或 DNS 問題。    |    針對網路與 DNS 進行疑難排解。    |
|    0x8007232B     |    DNS 名稱不存在。    |    KMS 用戶端    |    KMS 用戶端在 DNS 中找不到 KMS SRV RR。 如果在網路上沒有 KMS 主機，則應該安裝 MAK。    |    確認已安裝 KMS 主機，且 DNS 發佈為啟用 （預設值）。 若 DNS 無法使用，將 KMS 用戶端指向 KMS 主機使用 slmgr.vbs /skms < kms_host_name&gt >。（選擇性） 取得並安裝 MAK;然後，啟用系統。 最後，針對 DNS 進行疑難排解。    |
|    0x800706BA     |    RPC 伺服器無法使用。    |    KMS 用戶端    |    未在 KMS 主機上，設定防火牆設定或 DNS SRV 記錄已過時。    |    請確定金鑰管理服務的防火牆例外狀況已啟用 KMS 主機電腦上。 請確定 SRV 記錄指向有效的 KMS 主機。 針對網路連線進行疑難排解。    |
|    0x8007251D     |    針對 DNS 查詢中找不到任何記錄。    |    KMS 用戶端    |    KMS 用戶端在 DNS 中找不到 KMS SRV RR。    |    針對網路連線與 DNS 進行疑難排解。    |
|    0xC004F074     |    軟體保護服務報告電腦無法啟動。 無法連絡任何金鑰管理服務 (KMS)。 如需其他資訊，請參閱應用程式事件記錄檔。    |    KMS 用戶端    |    所有 KMS 主機系統都傳回錯誤。    |    從每個事件識別碼與啟用嘗試關聯的 12288 疑難排解錯誤。    |
|    0x8004FE21     |    這部電腦並未執行正版的 Windows。    |    MAK/KMS 用戶端    |    有幾個原因，可能會發生此問題。 最可能的原因是，在執行未獲授權的其他語言套件的 Windows 版本的電腦上已安裝語言套件 (MUI)。 （請注意這不一定表示遭到竄改。   有些應用程式可以安裝多國語言支援這些語言組件未授權該版本的 Windows 時，即使。）如果 Windows 已修改的惡意程式碼，以允許其他要安裝的功能，也可能會發生此問題。 如果某些系統檔案損毀，也可能會發生此問題。    |    若要解決此問題，您必須重新安裝作業系統。    |
|    0x80092328     |    0x80092328 DNS 名稱不存在。    |    KMS 用戶端    |    如果 KMS 用戶端找不到 KMS SRV 資源記錄在 DNS 中，可能會發生此問題。    |    若要解決此問題，請遵循下列 Microsoft 知識庫文章中的步驟：929826 出現錯誤訊息時您嘗試啟用 Windows Vista Enterprise、 Windows Vista Business、 Windows 7 或 Windows Server 2008:「 程式碼 0x8007232b 」    |
|    0x8007007b    |    0x8007007b DNS 名稱不存在。    |    KMS 用戶端    |    如果 KMS 用戶端找不到 KMS SRV 資源記錄在 DNS 中，可能會發生此問題。    |    若要解決此問題，請遵循下列 Microsoft 知識庫文章中的步驟：929826 出現錯誤訊息時您嘗試啟用 Windows Vista Enterprise、 Windows Vista Business、 Windows 7 或 Windows Server 2008:「 程式碼 0x8007232b 」    |
|    0x80070490    |您所輸入的產品金鑰無法運作。  檢查產品金鑰並再試一次，或輸入不同。 |MAK |因為輸入的 MAK 無效，或因為 Windows Server 2019 的已知問題，就會發生此問題。 |若要解決此問題，請使用命令列啟動電腦**slmgr-ipk \<5x5 金鑰\>**|
