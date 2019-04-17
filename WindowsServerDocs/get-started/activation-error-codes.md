---
title: 疑難排解啟用錯誤碼
description: 了解如何啟用錯誤碼進行疑難排解
ms.topic: article
ms.date: 03/21/2019
ms.technology: server-general
ms.assetid: ''
author: kaushika-msft
ms.author: kaushika-msft
ms.localizationpriority: medium
ms.openlocfilehash: c69c5d1d26b5b4baea8c8a2ee886ebc486418872
ms.sourcegitcommit: bc681dc782ce0e03d98a412951ec0fd84a99474d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2019
ms.locfileid: "9275875"
---
# 疑難排解啟用錯誤碼

**家用版使用者**這篇文章供使用，藉由支援服務專員和 IT 專業人員。 如果您正在尋求有關 Windows 啟用錯誤訊息的詳細資訊，請移至下列 Microsoft 網站：

[取得 Windows 啟用錯誤的說明](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors) 

當您嘗試使用多次啟用金鑰 (MAK) 或金鑰管理服務 (KMS) 來進行大量啟用一或多個 Windows 為基礎的電腦上時，您會收到錯誤訊息包含特定的錯誤碼。 本文討論如何針對這些錯誤進行疑難排解。

**錯誤碼和描述**

|錯誤碼 |錯誤訊息 |啟用類型 |可能的原因 |疑難排解步驟 |
|-----------|--------------|----------------|---------------|----------------------|
|    0xC004C001    |    啟用伺服器來決定指定的產品金鑰無效    |    MAK    |    輸入不正確的 MAK。    |    確認金鑰是 Microsoft 所提供的 MAK。 針對有關您封鎖的啟用問題請連絡[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)     |
|    0xC004C003     |    啟用伺服器來決定會封鎖指定的產品金鑰    |    MAK    |    MAK 啟用伺服器上封鎖。    |    連絡人： [Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)來取得新的 MAK 和安裝/啟用系統。    |
|    0xC004C008     |    啟用伺服器決定無法使用指定的產品金鑰。    |    KMS    |    KMS 金鑰已超過啟用限制。    |    KMS 主機金鑰，會啟動高達 10 倍六個不同的電腦上。 如果需要更多的啟用，請連絡[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004B100    |    啟用伺服器判定電腦無法啟動。    |    MAK    |    如果 MAK 不受支援，可能會發生此問題。    |    若要疑難排解此問題，請確認已使用 MAK MAK 由 Microsoft 所提供。 若要確認 MAK 可有效，連絡: [Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C020     |    啟用伺服器報告多次啟用金鑰已超過其限制。    |    MAK    |    MAK 已超過啟用限制。    |    Mak 的設計具備有限的啟用次數。   請連絡[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C021     |    啟用伺服器回報已超過多次啟用金鑰擴充功能限制。    |    MAK    |    MAK 已超過啟用限制。    |    Mak 的設計具備有限的啟用次數。   請連絡[Microsoft 授權啟用中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)    |
|    0xC004F009     |    「 軟體保護服務回報寬限期已過期。    |    MAK    |    在寬限期內到期之前，系統已啟用。 現在，系統是處於通知狀態。    |    請參閱節 「 使用者體驗 」。    |
|    0xC004F00F     |    軟體授權伺服器報告的硬體識別碼繫結超出容錯層級。    |    MAK/KMS 用戶端/KMS 主機    |    已變更的硬體或驅動程式已更新系統上。    |    MAK： 在線上使用 OOT 寬限期內重新啟用系統或電話啟用。      KMS： 重新啟動或執行 slmgr.vbs /ato。    |
|    0xC004F014     |    軟體保護服務報告沒有可用的產品金鑰    |    MAK/KMS 用戶端    |    系統上已不安裝任何產品金鑰。    |    安裝 MAK 產品金鑰，或安裝安裝 KMS 金鑰安裝媒體上 \sources\pid.txt 中找到。    |
|    0xC004F02C     |    「 軟體保護服務回報離線啟用資料的格式不正確。    |    MAK/KMS 用戶端    |    系統已偵測到輸入電話啟用期間的資料不正確。    |    確認德的輸入是正確的。    |
|    0xC004F035     |    「 軟體保護服務回報的電腦可能未啟用使用大量授權產品金鑰...「 等於此錯誤程式碼錯誤文字是正確的但模稜兩可。 這個錯誤表示電腦遺失了 Windows 中的標記 BIOS – OEM 系統，以指出傳送搭配版本的 Windows，KMS 用戶端啟用是必要的合格電腦的系統上提供。 錯誤： 無效大量授權金鑰的順序，以啟用時，您需要有效的多次啟用金鑰 (MAK) 或零售金鑰來變更您的產品金鑰。 您必須將合格作業系統授權和磁碟區的授權數 Windows 7 升級授權或完整授權適用於 Windows 7 來源的零售版。 此軟體的任何其他安裝是違反合約和適用的著作權法。    |    KMS 用戶端/KMS 主機    |    Windows 7 的大量版本僅升級授權。 不支援安裝並沒有安裝合格作業系統的電腦上的磁碟區的作業系統。    |    安裝合格的為 Microsoft 的作業系統版本，並再使用 MAK 進行啟用。    |
|    0xC004F038     |    「 軟體保護服務回報的電腦可能不會啟用。 報告的金鑰管理服務 (KMS) 計數不足。 請連絡您的系統管理員。    |    KMS 用戶端    |    無法夠高 KMS 主機上的計數。 適用於 Windows Server ≥5 或 ≥25 Windows 用戶端，必須是 KMS 計數。    |    若要啟用 KMS 用戶端的 KMS 集區需要更多的電腦。 執行 Slmgr.vbs /dli KMS 主機上取得目前的計數。    |
|    0xC004F039     |    「 軟體保護服務回報的電腦可能不會啟用。 金鑰管理服務 (KMS) 不會啟用。    |    KMS 用戶端    |    當 KMS 要求不會獲得解答，就會發生此錯誤。    |    疑難排解 KMS 主機和用戶端之間的網路連線。 請確定 TCP 連接埠 1688 （預設值） 未被防火牆封鎖或否則已篩選出。    |
|    0xC004F041     |    「 軟體保護服務決定金鑰管理伺服器 (KMS) 未啟用。 必須啟用 KMS。    |    KMS 用戶端    |    未啟用 KMS 主機。    |    啟用 KMS 主機使用下列任一線上或電話啟用。    |
|    0xC004F042     |    「 軟體保護服務決定，無法使用指定的金鑰管理服務 (KMS)。    |    KMS 用戶端    |    KMS 用戶端與 KMS 主機不相符。    |    當 KMS 用戶端連絡無法啟用用戶端軟體的 KMS 主機，就會發生這個錯誤。 這可以是常見，例如包含應用程式和作業系統專屬的 KMS 主機的混合環境中。    |
|    0xC004F050     |    「 軟體保護服務報告指出產品金鑰無效。    |    KMS，KMS 用戶端，MAK    |    這可能有錯字 KMS 金鑰或輸入搶鮮版金鑰在發行版本的作業系統中，所造成。    |    對應的 Windows 版本上安裝適當的 KMS 金鑰。 拼字檢查。 如果該金鑰複製和貼上，請確定 em 破折號取代不取代索引鍵中虛線。    |
|    0xC004F051     |    「 軟體保護服務回報已封鎖的產品金鑰。    |    MAK/KMS    |    啟用伺服器上的產品金鑰是由 Microsoft 封鎖。    |    取得新的 MAK/KMS 金鑰、 將它安裝在系統上，並啟用。    |
|    0xC004F064     |    「 軟體保護服務回報正版寬限期已過期。    |    MAK    |    Windows 啟用工具 (WAT) 已判斷系統不是正版。    |    請參閱磁碟區啟用作業指南。    |
|    0xC004F065     |    「 軟體保護服務所報告應用程式有效的非正版期間內執行。    |    MAK/KMS 用戶端    |    Windows 啟用工具已判斷系統不是正版。 系統將會繼續在非正版寬限期內執行。    |    取得並安裝正版產品金鑰，並在寬限期內啟動系統。 否則，系統會進入在寬限期結束通知狀態。    |
|    0xC004F06C     |    「 軟體保護服務回報的電腦可能不會啟用。 金鑰管理服務 (KMS) 決定要求時間戳記無效。    |    KMS 用戶端    |    用戶端電腦上的系統時間是從 KMS 主機上的時間太不同。    |    時間同步相當重要系統和網路安全性的各種不同的原因。 藉由變更系統時間同步 KMS 用戶端上的修正此問題。 建議使用網路時間通訊協定 (NTP) 時間來源或時間同步處理 Active Directory 網域服務。 這個問題使用 UTP 時間，是獨立的時區選取項目。    |
|    0x80070005     |    拒絕存取。 所要求的動作需要提升權限的權限。    |    KMS 用戶端/MAK/KMS 主機    |    使用者帳戶控制 (UAC) 會禁止啟用處理程序非提升權限的命令提示字元中執行。    |    從提升權限的命令提示字元執行 slmgr.vbs。   Cmd.exe，以滑鼠右鍵按一下，然後按一下 [以系統管理員身分執行。    |
|    0x8007232A     |    DNS 伺服器失敗。    |    KMS 主機    |    系統沒有網路或 DNS 問題。    |    疑難排解網路和 DNS。    |
|    0x8007232B     |    DNS 名稱不存在。    |    KMS 用戶端    |    KMS 用戶端在 DNS 中找不到 KMS SRV 給。 如果在網路上的 KMS 主機不存在，就應該安裝 MAK。    |    確認已安裝 KMS 主機，以及 DNS 發佈啟用 （預設值）。 如果無法使用 DNS 時，使用 slmgr.vbs /skms <kms_host_name> KMS 用戶端點到 KMS 主機。您可以選擇性地取得並安裝 MAK;然後，啟動系統。 最後，疑難排解 DNS。    |
|    0x800706BA     |    RPC 伺服器無法使用。    |    KMS 用戶端    |    KMS 主機上未設定防火牆設定或 DNS SRV 記錄過時。    |    請確定在 KMS 主機電腦上啟用金鑰管理服務防火牆例外狀況。 請確定 SRV 記錄指向有效的 KMS 主機。 疑難排解網路連線。    |
|    0x8007251D     |    針對 DNS 查詢找不到記錄。    |    KMS 用戶端    |    KMS 用戶端在 DNS 中找不到 KMS SRV 給。    |    疑難排解網路連線和 DNS。    |
|    0xC004F074     |    「 軟體保護服務回報的電腦可能不會啟用。 無法連絡沒有金鑰管理服務 (KMS)。 請參閱應用程式事件記錄檔，如需其他資訊。    |    KMS 用戶端    |    所有的 KMS 主機系統傳回錯誤。    |    疑難排解從每個事件識別碼 12288 嘗試啟動相關聯的錯誤。    |
|    0x8004FE21     |    此電腦未執行正版 Windows。    |    MAK/KMS 用戶端    |    此問題可能會發生幾個原因。 最有可能的原因是，執行未授權適用於其他語言套件的 Windows 版本的電腦上已安裝的語言套件 (MUI)。 （請注意這不一定表示遭竄改。   某些應用程式可以安裝多語系支援即使該版本的 Windows 未授權的這些語言套件。）如果 Windows 已經過修改由惡意程式碼，以允許安裝額外功能，也可能會發生此問題。 如果特定系統檔案已損毀，也可能會發生此問題。    |    若要解決此問題，您必須重新安裝作業系統。    |
|    0x80092328     |    0x80092328 DNS 名稱不存在。    |    KMS 用戶端    |    如果 KMS 用戶端找不到 KMS SRV 資源記錄在 DNS 中，可能會發生此問題。    |    若要解決此問題，請依照下列 Microsoft 知識庫文章中的步驟執行： 當您嘗試啟用 Windows Vista 企業版、 Windows Vista Business、 Windows 7 或 Windows Server 2008 929826 的錯誤訊息: 「 碼 0x8007232b 」    |
|    0x8007007b    |    0x8007007b DNS 名稱不存在。    |    KMS 用戶端    |    如果 KMS 用戶端找不到 KMS SRV 資源記錄在 DNS 中，可能會發生此問題。    |    若要解決此問題，請依照下列 Microsoft 知識庫文章中的步驟執行： 當您嘗試啟用 Windows Vista 企業版、 Windows Vista Business、 Windows 7 或 Windows Server 2008 929826 的錯誤訊息: 「 碼 0x8007232b 」    |
