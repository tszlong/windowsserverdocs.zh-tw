---
title: 針對啟用錯誤碼進行疑難排解
description: 了解如何針對啟用錯誤碼進行疑難排解
ms.topic: article
ms.date: 03/21/2019
ms.technology: server-general
ms.assetid: ''
author: kaushika-msft
ms.author: kaushika-msft
ms.localizationpriority: medium
ms.openlocfilehash: 076fc3408810ba7b0f51c6a428955f99a66dd394
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "64880879"
---
# <a name="troubleshooting-activation-error-codes"></a>針對啟用錯誤碼進行疑難排解

**家庭用戶**：本文適用於支援專員和 IT 專業人員。 如需 Windows 啟用錯誤訊息的詳細資訊，請前往下列 Microsoft 網站：

[取得 Windows 啟用錯誤的說明](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors) 

當您嘗試使用多次啟用金鑰 (MAK) 或金鑰管理服務 (KMS) 在一或多部 Windows 電腦上執行大量啟用時，您會收到包含特定錯誤碼的錯誤訊息。 本文討論如何針對這些錯誤進行疑難排解。

**錯誤碼與描述**

|錯誤碼 |錯誤訊息 |啟用類型 |可能的原因 |疑難排解步驟 |
|-----------|--------------|----------------|---------------|----------------------|
|    0xC004C001    |    啟用伺服器判定所指定的產品金鑰無效    |    MAK    |    輸入的 MAK 無效。    |    確認該金鑰是 Microsoft 提供的 MAK。 若對您被封鎖的啟用有任何疑問，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)     |
|    0xC004C003     |    啟用伺服器判定所指定的產品金鑰已被封鎖    |    MAK    |    該 MAK 在啟用伺服器上已被封鎖。    |    請連絡：[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)以取得新的 MAK 並安裝/啟用系統。    |
|    0xC004C008     |    啟用伺服器判定無法使用所指定的產品金鑰。    |    KMS    |    該 KMS 金鑰已超過啟用限制。    |    KMS 主機金鑰最多可用來在六部不同的電腦上啟用 10 次。 若需要啟用更多次，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004B100    |    啟用伺服器判定無法啟用電腦。    |    MAK    |    如果該 MAK 不受支援，就可能會發生此問題。    |    若要針對此問題進行疑難排解，請確認所使用 MAK 是 Microsoft 提供的 MAK。 若要確認該 MAK 為有效，請連絡：[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C020     |    啟用伺服器報告指出多次啟用金鑰已超過其限制。    |    MAK    |    該 MAK 已超過啟用限制。    |    MAK 的設計有啟用次數限制。   請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C021     |    啟用伺服器報告指出已超過多次啟用金鑰延伸限制。    |    MAK    |    該 MAK 已超過啟用限制。    |    MAK 的設計有啟用次數限制。   請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)    |
|    0xC004F009     |    軟體保護服務報告指出寬限期已過。    |    MAK    |    寬限期在系統啟用前就已過期。 現在，系統是處於「通知」狀態。    |    請參閱＜使用者經驗＞一節。    |
|    0xC004F00F     |    軟體授權伺服器報告指出硬體識別碼繫結超出容忍度。    |    MAK/KMS 用戶端/KMS 主機    |    系統上的硬體已變更或驅動程式已更新。    |    MAK：在 OOT 寬限期內使用線上啟用或電話啟用方式重新啟用系統。      KMS︰重新啟動或執行 slmgr.vbs /ato。    |
|    0xC004F014     |    軟體保護服務報告指出產品金鑰無法使用    |    MAK/KMS 用戶端    |    系統尚未安裝任何產品金鑰。    |    安裝 MAK 產品金鑰或安裝 KMS 安裝識別碼 (位於安裝媒體的 \sources\pid.txt)。    |
|    0xC004F02C     |    軟體保護服務報告指出離線啟用資料格式不正確。    |    MAK/KMS 用戶端    |    系統偵測到在電話啟用期間輸入的資料無效。    |    確定輸入的 CID 是正確的。    |
|    0xC004F035     |    此錯誤碼相當於「軟體保護服務報告指出無法使用大量授權產品金鑰啟用電腦...」錯誤文字正確，但模稜兩可。 此錯誤指出電腦在 BIOS 中缺少 Windows 標記 - OEM 系統上提供該標記以指出電腦隨附合格的 Windows 版本，這是 KMS 用戶端啟用的需求。 錯誤： 大量授權金鑰無效。若要啟用，您必須將產品金鑰變更為有效的多次啟用金鑰 (MAK) 或零售金鑰。 您必須有合格的作業系統授權以及大量授權的 Windows 7 之升級授權，或從零售來源取得 Windows 7 的完整授權。 此軟體的任何其他安裝都違反您合約及適用的著作權法。    |    KMS 用戶端/KMS 主機    |    只有 Windows 7 大量授權版本已授權升級。 不支援在未安裝合格作業系統的電腦上安裝大量授權作業系統。    |    請安裝 Microsoft 作業系統的合格版本，然後使用 MAK 進行啟用。    |
|    0xC004F038     |    軟體保護服務報告指出無法啟用電腦。 金鑰管理服務 (KMS) 報告的計數不足。 請連絡您的系統管理員。    |    KMS 用戶端    |    KMS 主機上的計數不足。 KMS 計數必須 ≥5 (針對 Windows Server) 或 ≥25 (針對 Windows 用戶端)。    |    KMS 集區中必須有更多電腦，KMS 用戶端才能完成啟用。 執行 Slmgr.vbs /dli 以取得 KMS 主機上的最新計數。    |
|    0xC004F039     |    軟體保護服務報告指出無法啟用電腦。 金鑰管理服務 (KMS) 未啟用。    |    KMS 用戶端    |    當沒有收到 KMS 要求的回應時，便會發生此錯誤。    |    針對 KMS 主機與用戶端之間的網路連線進行疑難排解。 確定 TCP 通訊埠 1688 (預設值) 並未被防火牆封鎖或過濾。    |
|    0xC004F041     |    軟體保護服務判定金鑰管理伺服器 (KMS) 並未啟用。 必須啟用 KMS。    |    KMS 用戶端    |    KMS 主機未啟用。    |    使用線上啟用或電話啟用方式啟用 KMS 主機。    |
|    0xC004F042     |    軟體保護服務判定無法使用所指定的金鑰管理服務 (KMS)。    |    KMS 用戶端    |    KMS 用戶端與 KMS 主機不符。    |    當 KMS 用戶端連絡無法啟用用戶端軟體的 KMS 主機時，便會發生此錯誤。 例如，在包含應用程式與作業系統特定 KMS 主機的混合式環境中，這種情況可能很常見。    |
|    0xC004F050     |    軟體保護服務報告指出產品金鑰無效。    |    KMS、KMS 用戶端、MAK    |    這可能是因為未正確輸入 KMS 金鑰，或在作業系統的發行版本輸入搶鮮版 (Beta) 金鑰。    |    在對應的 Windows 版本上安裝適當的 KMS 金鑰。 檢查拼字。 如果您以剪貼的方式輸入金鑰，請確定金鑰中的長破折號沒有以破折號來取代。    |
|    0xC004F051     |    軟體保護服務報告指出已封鎖產品金鑰。    |    MAK/KMS    |    Microsoft 已封鎖啟用伺服器上的產品金鑰 。    |    取得新的 MAK/KMS 金鑰、在系統上安裝該金鑰，然後啟用。    |
|    0xC004F064     |    軟體保護服務報告指出非正版寬限期已過。    |    MAK    |    Windows 啟用工具 (WAT) 判定系統不是正版。    |    請參閱《大量啟用作業指南》。    |
|    0xC004F065     |    軟體保護服務報告指出應用程式在有效的非正版期間執行。    |    MAK/KMS 用戶端    |    Windows 啟用工具判定系統不是正版。 系統將會在非正版寬限期內繼續執行。    |    取得並安裝正版產品金鑰，並在寬限期內啟用系統。 否則，系統會在寬限期結束時進入「通知」狀態。    |
|    0xC004F06C     |    軟體保護服務報告指出無法啟用電腦。 金鑰管理服務 (KMS) 判定要求時間戳記無效。    |    KMS 用戶端    |    用戶端電腦上系統時間與 KMS 主機上的時間差異太大。    |    基於各種原因，時間同步對於系統與網路安全性而言非常重要。 請變更用戶端上的系統時間使其與 KMS 同步，以修正此問題。 建議使用網路時間通訊協定 (NTP) 時間來源或 Active Directory Domain Services 來進行時間同步。 此問題導因於使用 UTP 時間，且與時區選擇無關。    |
|    0x80070005     |    拒絕存取。 要求的動作需要較高權限。    |    KMS 用戶端/MAK/KMS 主機    |    使用者帳戶控制 (UAC) 禁止啟用處理程序在未提升權限的命令提示字元中執行。    |    從提升權限的命令提示字元執行 slmgr.vbs。   在 [cmd.exe] 上按一下滑鼠右鍵，然後按一下 [以系統管理員身分執行]。    |
|    0x8007232A     |    DNS 伺服器失敗。    |    KMS 主機    |    系統有網路或 DNS 問題。    |    針對網路與 DNS 進行疑難排解。    |
|    0x8007232B     |    DNS 名稱不存在。    |    KMS 用戶端    |    KMS 用戶端在 DNS 中找不到 KMS SRV RR。 如果網路上未存在 KMS 主機，則您應該安裝 MAK。    |    確認已安裝 KMS 主機，且已啟用 DNS 發佈 (預設值)。 若 DNS 無法使用，請使用 slmgr.vbs /skms <KMS 主機名稱> 將 KMS 用戶端指向 KMS 主機；或者，取得並安裝 MAK，然後啟用系統。 最後，針對 DNS 進行疑難排解。    |
|    0x800706BA     |    RPC 伺服器無法使用。    |    KMS 用戶端    |    未在 KMS 主機上設定防火牆設定，或 DNS SRV 記錄已過時。    |    確定已在 KMS 主機電腦上啟用「金鑰管理服務」防火牆例外。 確定 SRV 記錄是指向有效的 KMS 主機。 針對網路連線進行疑難排解。    |
|    0x8007251D     |    找不到 DNS 查詢記錄。    |    KMS 用戶端    |    KMS 用戶端在 DNS 中找不到 KMS SRV RR。    |    針對網路連線與 DNS 進行疑難排解。    |
|    0xC004F074     |    軟體保護服務報告指出無法啟用電腦。 無法連絡金鑰管理服務 (KMS)。 如需其他資訊，請參閱應用程式事件記錄檔。    |    KMS 用戶端    |    所有 KMS 主機系統都傳回錯誤。    |    針對從每個與啟用嘗試建立關聯的事件識別碼 12288 錯誤進行疑難排解。    |
|    0x8004FE21     |    這部電腦未執行正版 Windows。    |    MAK/KMS 用戶端    |    多種原因會導致發生此問題。 最可能的造成原因是電腦上尚未安裝語言套件 (MUI)，這些電腦所執行 Windows 版本沒有其他語言套件的授權。 (注意：這不一定表示遭到竄改。   某些應用程式可以安裝多語支援，即使該 Windows 版本沒有這些語言套件的授權也一樣。)如果 Windows 遭到惡意程式碼修改為允許安裝其他功能，也可能會發生此問題。 如果特定系統檔案已損毀，也可能會發生此問題。    |    若要解決此問題，您必須重新安裝作業系統。    |
|    0x80092328     |    0x80092328 DNS 名稱不存在。    |    KMS 用戶端    |    如果 KMS 用戶端在 DNS 中找不到 KMS SRV 資源記錄，就可能會發生此問題。    |    若要解決此問題，請遵循下列 Microsoft 知識庫文章中的步驟執行：929826 當您嘗試啟用 Windows Vista Enterprise、Windows Vista Business、Windows 7 或 Windows Server 2008 時收到錯誤訊息：「代碼 0x8007232b」    |
|    0x8007007b    |    0x8007007b DNS 名稱不存在。    |    KMS 用戶端    |    如果 KMS 用戶端在 DNS 中找不到 KMS SRV 資源記錄，就可能會發生此問題。    |    若要解決此問題，請遵循下列 Microsoft 知識庫文章中的步驟執行：929826 當您嘗試啟用 Windows Vista Enterprise、Windows Vista Business、Windows 7 或 Windows Server 2008 時收到錯誤訊息：「代碼 0x8007232b」    |
|    0x80070490    |輸入的產品金鑰無效。  請檢查產品金鑰並再試一次，或輸入其他金鑰。 |MAK |因為輸入無效的 MAK，或由於 Windows Server 2019 中的已知問題，所以發生此問題。 |若要解決此問題，請使用命令列 **slmgr -ipk \<5x5 key\>** 來啟用電腦|
