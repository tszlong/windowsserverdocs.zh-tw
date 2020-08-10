---
title: 解決 Windows 啟用錯誤碼
description: 了解如何針對啟用錯誤碼進行疑難排解
ms.topic: troubleshooting
ms.date: 9/18/2019
author: kaushika-msft
ms.author: kaushika-msft; v-tea
ms.localizationpriority: medium
ms.custom:
- CI ID 116803
- CSSTroubleshoot
manager: dcscontentpm
ms.openlocfilehash: 375c028acc0a972fcc1ca02e840075cdafd7369e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941790"
---
# <a name="resolve-windows-activation-error-codes"></a>解決 Windows 啟用錯誤碼

> [!NOTE]
> 本文適用於技術支援專員和 IT 專業人員。 如需 Windows 啟用錯誤訊息的詳細資訊，請參閱[取得 Windows 啟用錯誤的說明](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors)。

本文提供疑難排解資訊，協助您回應在嘗試使用多次啟用金鑰 (MAK) 或金鑰管理服務 (KMS) 在一或多部 Windows 電腦上執行大量啟用時，您會收到的錯誤訊息。 在下表中尋找錯誤碼，然後選取連結，以查看有關該錯誤碼及其解決方式的詳細資訊。

如需大量啟用的詳細資訊，請參閱[大量啟用規劃](/windows/deployment/volume-activation/plan-for-volume-activation-client)。

如需目前和最新 Windows 版本的大量啟用詳細資訊，請參閱[大量啟用 [用戶端]](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-windows-10)。

如需舊版 Windows 的大量啟用詳細資訊，請參閱 KB 929712  [Windows Vista、Windows Server 2008、Windows Server 2008 R2 和 Windows 7 的大量啟用資訊](https://support.microsoft.com/help/929712/volume-activation-information-for-windows-vista-windows-server-2008-wi)。

## <a name="diagnostic-tool"></a>診斷工具

> [!NOTE]
> 此工具的目的是為了在執行 Windows 企業版、專業版或伺服器版本的電腦上，協助修正 Windows 啟用問題。


Microsoft 支援及修復小幫手 (SaRA) 可簡化 Windows KMS 啟動的疑難排解。 從[這裡](https://aka.ms/SaRA-WindowsActivation)下載診斷工具。

此工具會嘗試啟動 Windows。 如果傳回啟動錯誤碼，此工具會顯示已知錯誤碼的目標解決方案。

以下是支援的錯誤碼：0xC004F038、0xC004F039、0xC004F041、0xC004F074、0xC004C008、0x8007007b、0xC004C003、0x8007232B。

## <a name="summary-of-error-codes"></a>錯誤碼摘要

|錯誤碼 |錯誤訊息 |啟用類型&nbsp;|
|-----------|--------------|----------------|
|[0x8004FE21](#0x8004fe21-this-computer-is-not-running-genuine-windows) |這部電腦未執行正版 Windows。  |MAK<br />KMS 用戶端 |
|[0x80070005](#0x80070005-access-denied) |拒絕存取。 要求的動作需要提高的權限。 |MAK<br />KMS 用戶端<br />KMS 主機 |
|[0x8007007b](#0x8007007b-dns-name-does-not-exist) |0x8007007b DNS 名稱不存在。 |KMS 用戶端 |
|[0x80070490](#0x80070490-the-product-key-you-entered-didnt-work) |輸入的產品金鑰無效。 請檢查產品金鑰並再試一次，或輸入其他金鑰。 |MAK |
|[0x800706BA](#0x800706ba-the-rpc-server-is-unavailable) |RPC 伺服器無法使用。 |KMS 用戶端 |
|[0x8007232A](#0x8007232a-dns-server-failure) |DNS 伺服器失敗。  |KMS 主機  |
|[0x8007232B](#0x8007232b-dns-name-does-not-exist) |DNS 名稱不存在。 |KMS 用戶端 |
|[0x8007251D](#0x8007251d-no-records-found-for-dns-query) |找不到 DNS 查詢記錄。 |KMS 用戶端 |
|[0x80092328](#0x80092328-dns-name-does-not-exist) |DNS 名稱不存在。  |KMS 用戶端 |
|[0xC004B100](#0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated) |啟用伺服器判定無法啟用電腦。 |MAK |
|[0xC004C001](#0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid) |啟用伺服器判定所指定的產品金鑰無效 |MAK|
|[0xC004C003](#0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked) |啟用伺服器判定所指定的產品金鑰已被封鎖 |MAK |
|[0xC004C008](#0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used) |啟用伺服器判定無法使用所指定的產品金鑰。 |KMS |
|[0xC004C020](#0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit) |啟用伺服器報告多次啟用金鑰已超過其限制。 |MAK |
|[0xC004C021](#0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded) |啟用伺服器報告已超過多次啟用金鑰延伸限制。 |MAK |
|[0xC004F009](#0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired) |軟體保護服務報告指出寬限期已過。 |MAK |
|[0xC004F00F](#0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance) |軟體授權伺服器報告指出硬體識別碼繫結超出容忍度。 |MAK<br />KMS 用戶端<br />KMS 主機 |
|[0xC004F014](#0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available) |軟體保護服務報告指出產品金鑰無法使用 |MAK<br />KMS 用戶端 |
|[0xC004F02C](#0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect) |軟體保護服務報告指出離線啟用資料格式不正確。 |MAK<br />KMS 用戶端 |
|[0xC004F035](#0xc004f035-invalid-volume-license-key) |軟體保護服務報告指出無法使用大量授權產品金鑰啟用電腦。 |KMS 用戶端<br />KMS 主機 |
|[0xC004F038](#0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient) |軟體保護服務報告指出無法啟用電腦。 您的金鑰管理服務 (KMS) 報告的計數不足。 請連絡您的系統管理員。 |KMS 用戶端 |
|[0xC004F039](#0xc004f039-the-key-management-service-kms-is-not-enabled) |軟體保護服務報告指出無法啟用電腦。 金鑰管理服務 (KMS) 未啟用。 |KMS 用戶端 |
|[0xC004F041](#0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated) |軟體保護服務判定金鑰管理伺服器 (KMS) 並未啟用。 必須啟用 KMS。  |KMS 用戶端 |
|[0xC004F042](#0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used) |軟體保護服務判定無法使用所指定的金鑰管理服務 (KMS)。 |KMS 用戶端 |
|[0xC004F050](#0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid) |軟體保護服務報告指出產品金鑰無效。 |MAK<br />KMS<br />KMS 用戶端 |
|[0xC004F051](#0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked) |軟體保護服務報告指出產品金鑰被封鎖。 |MAK<br />KMS |
|[0xC004F064](#0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired) |軟體保護服務報告指出非正版寬限期已過。 |MAK |
|[0xC004F065](#0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period) |軟體保護服務報告指出應用程式在有效的非正版期間執行。 |MAK<br />KMS 用戶端 |
|[0xC004F06C](#0xc004f06c-the-request-timestamp-is-invalid) |軟體保護服務報告指出無法啟用電腦。 金鑰管理服務 (KMS) 判斷出要求時間戳記無效。  |KMS 用戶端 |
|[0xC004F074](#0xc004f074-no-key-management-service-kms-could-be-contacted) |軟體保護服務報告指出無法啟用電腦。 無法聯繫金鑰管理服務 (KMS)。 如需其他資訊，請參閱應用程式事件記錄檔。  |KMS 用戶端 |

## <a name="causes-and-resolutions"></a>原因和解決方式

### <a name="0x8004fe21-this-computer-is-not-running-genuine-windows"></a>0x8004FE21 這部電腦未執行正版 Windows

#### <a name="possible-cause"></a>可能的原因

多種原因會導致發生此問題。 最可能的造成原因是電腦上尚未安裝語言套件 (MUI)，這些電腦所執行 Windows 版本沒有其他語言套件的授權。

> [!NOTE]
> 此問題不一定表示遭到竄改。 某些應用程式可以安裝多語支援，即使該 Windows 版本沒有這些語言套件的授權也一樣。

如果 Windows 遭到惡意程式碼修改為允許安裝其他功能，也可能會發生此問題。 如果特定系統檔案已損毀，也可能會發生此問題。

#### <a name="resolution"></a>解決方法

若要解決此問題，您必須重新安裝作業系統。

### <a name="0x80070005-access-denied"></a>0x80070005 拒絕存取

此錯誤訊息的完整文字與下列類似：

> 拒絕存取。 要求的動作需要提高的權限。

#### <a name="possible-cause"></a>可能的原因

使用者帳戶控制 (UAC) 禁止啟用處理程序在未提升權限的命令提示字元視窗中執行。

#### <a name="resolution"></a>解決方法

從提升權限的命令提示字元執行 **slmgr.vbs**。 若要這麼做，請在 [開始] 功能表上，以滑鼠右鍵按一下 **cmd.exe**，然後選取 [以系統管理員身分執行]。

### <a name="0x8007007b-dns-name-does-not-exist"></a>0x8007007b DNS 名稱不存在

#### <a name="possible-cause"></a>可能的原因

如果 KMS 用戶端在 DNS 中找不到 KMS SRV 資源記錄，就可能會發生此問題。

#### <a name="resolution"></a>解決方法

如需針對這類 DNS 相關問題進行疑難排解的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

### <a name="0x80070490-the-product-key-you-entered-didnt-work"></a>0x80070490 輸入的產品金鑰無效

此錯誤的完整文字與下列類似：
> 輸入的產品金鑰無效。 請檢查產品金鑰並再試一次，或輸入其他金鑰。

#### <a name="possible-cause"></a>可能的原因

因為輸入的 MAK 無效，或由於 Windows Server 2019 中的已知問題，所以發生此問題。

#### <a name="resolution"></a>解決方法

若要暫時解決此問題並啟動電腦，請在提升權限的命令提示字元執行 **slmgr -ipk <5x5 key>** 。

### <a name="0x800706ba-the-rpc-server-is-unavailable"></a>0x800706BA RPC 伺服器無法使用

#### <a name="possible-cause"></a>可能的原因

未在 KMS 主機上設定防火牆設定，或 DNS SRV 記錄已過時。

#### <a name="resolution"></a>解決方法

在 KMS 主機上，確定已針對金鑰管理服務 (TCP 連接埠 1688) 啟用防火牆例外。

確定 DNS SRV 記錄指向有效的 KMS 主機。

針對網路連線進行疑難排解。

如需針對這類 DNS 相關問題進行疑難排解的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

### <a name="0x8007232a-dns-server-failure"></a>0x8007232A DNS 伺服器失敗

#### <a name="possible-cause"></a>可能的原因

系統有網路或 DNS 問題。

#### <a name="resolution"></a>解決方法

針對網路與 DNS 進行疑難排解。

### <a name="0x8007232b-dns-name-does-not-exist"></a>0x8007232B DNS 名稱不存在

#### <a name="possible-cause"></a>可能的原因

KMS 用戶端在 DNS 中找不到 KMS 伺服器資源記錄 (SRV RR)。

#### <a name="resolution"></a>解決方法

確認已安裝 KMS 主機，且已啟用 DNS 發佈 (預設值)。 若 DNS 無法使用，請使用 **slmgr.vbs /skms <*kms_host_name*>** 將 KMS 用戶端指向 KMS 主機。

如果您沒有 KMS 主機，請取得並安裝 MAK。 然後，啟動系統。

如需針對這類 DNS 相關問題進行疑難排解的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

### <a name="0x8007251d-no-records-found-for-dns-query"></a>0x8007251D 找不到 DNS 查詢記錄

#### <a name="possible-cause"></a>可能的原因

KMS 用戶端在 DNS 中找不到 KMS SRV 記錄。

#### <a name="resolution"></a>解決方法

針對網路連線與 DNS 進行疑難排解。 如需如何針對這類 DNS 相關問題進行疑難排解的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

### <a name="0x80092328-dns-name-does-not-exist"></a>0x80092328 DNS 名稱不存在

#### <a name="possible-cause"></a>可能的原因

如果 KMS 用戶端在 DNS 中找不到 KMS SRV 資源記錄，就可能會發生此問題。

#### <a name="resolution"></a>解決方法

如需針對這類 DNS 相關問題進行疑難排解的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

### <a name="0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated"></a>0xC004B100 啟用伺服器判定無法啟用電腦

#### <a name="possible-cause"></a>可能的原因

不支援該 MAK。

#### <a name="resolution"></a>解決方法

若要針對此問題進行疑難排解，請確認您所使用的 MAK 是 Microsoft 所提供的 MAK。 若要驗證 MAK 是否有效，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。

### <a name="0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid"></a>0xC004C001 啟用伺服器判定所指定的產品金鑰無效

#### <a name="possible-cause"></a>可能的原因

您輸入的 MAK 無效。

#### <a name="resolution"></a>解決方法

確認該金鑰是 Microsoft 所提供的 MAK。 如需其他協助，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。

### <a name="0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked"></a>0xC004C003 啟用伺服器判定所指定的產品金鑰已被封鎖

#### <a name="possible-cause"></a>可能的原因

該 MAK 在啟用伺服器上已被封鎖。

#### <a name="resolution"></a>解決方法

若要取得新的 MAK，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。 取得新的 MAK 之後，請嘗試再次安裝和啟用 Windows。

### <a name="0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used"></a>0xC004C008 啟用伺服器判定無法使用所指定的產品金鑰

#### <a name="possible-cause"></a>可能的原因

該 KMS 金鑰已超過其啟用限制。 KMS 主機金鑰最多可以六部不同的電腦上啟用高達 10次。

#### <a name="resolution"></a>解決方法

如果您需要額外啟用，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。

### <a name="0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit"></a>0xC004C020 啟用伺服器報告多次啟用金鑰已超過其限制

#### <a name="possible-cause"></a>可能的原因

該 MAK 已超過其啟用限制。 根據設計，MAK 可啟用的次數有限。

#### <a name="resolution"></a>解決方法

如果您需要額外啟用，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。

### <a name="0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded"></a>0xC004C021 啟用伺服器報告已超過多次啟用金鑰延伸限制

#### <a name="possible-cause"></a>可能的原因

該 MAK 已超過其啟用限制。 根據設計，MAK 啟用的次數有限。

#### <a name="resolution"></a>解決方法

如果您需要額外啟用，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。

### <a name="0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired"></a>0xC004F009 軟體保護服務報告指出寬限期已過

#### <a name="possible-cause"></a>可能的原因

寬限期在系統啟用前就已過期。 現在，系統是處於「通知」狀態。

#### <a name="resolution"></a>解決方法

如需協助，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。

### <a name="0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance"></a>0xC004F00F 軟體授權伺服器報告指出硬體識別碼繫結超出容忍度

#### <a name="possible-cause"></a>可能的原因

系統上的硬體已變更或驅動程式已更新。

#### <a name="resolution"></a>解決方法

如果您使用 MAK 啟用，請在 OOT 寬限期內使用線上或電話啟用方式來重新啟用系統。

如果您使用 KMS 啟用，請重新啟動 Windows 或執行 **slmgr.vbs /ato**。

### <a name="0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available"></a>0xC004F014 軟體保護服務報告指出產品金鑰無法使用

#### <a name="possible-cause"></a>可能的原因

系統尚未安裝任何產品金鑰。

#### <a name="resolution"></a>解決方法

如果您使用 MAK 啟用，請安裝 MAK 產品金鑰。

如果您使用 KMS 啟用，請檢查 Pid.txt 檔案 (位於 \sources 資料夾中的安裝媒體) 以取得 KMS 設定金鑰。 安裝金鑰。

### <a name="0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect"></a>0xC004F02C 軟體保護服務報告指出離線啟用資料格式不正確

#### <a name="possible-cause"></a>可能的原因

系統偵測到在電話啟用期間輸入的資料無效。

#### <a name="resolution"></a>解決方法

確認輸入的 CID 是正確的。

### <a name="0xc004f035-invalid-volume-license-key"></a>0xC004F035 大量授權金鑰無效

此錯誤訊息的完整文字與下列類似：

> 錯誤：大量授權金鑰無效。 若要啟用，您必須將產品金鑰變更為有效的多次啟用金鑰 (MAK) 或零售金鑰。 您必須有合格的作業系統授權以及大量授權的 Windows 7 之升級授權，或從零售來源取得 Windows 7 的完整授權。 此軟體的任何其他安裝都違反您合約及適用的著作權法。

錯誤文字正確，但模稜兩可。 此錯誤表示電腦在其 BIOS 中遺漏 Windows 標記，而該標記可將其識別為執行合格 Windows 版本的 OEM 系統。 KMS 用戶端啟用需要此資訊。 此程式碼的更具體意義是「錯誤：大量授權金鑰無效」

#### <a name="possible-cause"></a>可能的原因

只有 Windows 7 大量授權版本已授權升級。 Microsoft 不支援在未安裝合格作業系統的電腦上安裝大量授權作業系統。

#### <a name="resolution"></a>解決方法

若要啟用，您必須執行下列其中一項動作：

- 將產品金鑰變更為有效的多次啟用金鑰 (MAK) 或零售金鑰。 您必須有合格的作業系統授權以及大量授權的 Windows 7 之升級授權，或從零售來源取得 Windows 7 的完整授權。
  > [!NOTE]
  > 如果您在嘗試啟用時收到錯誤 0x80072ee2，請改用後面的電話啟用方法。
- 依照下列步驟透過電話啟用：
   1. 執行 **slmgr /dti**，然後記錄安裝識別碼的值。 </li>
   1. 請洽詢 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)並提供安裝識別碼，以便接收確認識別碼。</li>
   1. 若要使用確認識別碼進行啟用，請執行 **slmgr /atp &lt;確認識別碼&gt;** 。

### <a name="0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient"></a>0xC004F038 您的金鑰管理服務 (KMS) 報告的計數不足

此錯誤訊息的完整文字與下列類似：

> 軟體保護服務報告指出無法啟用電腦。 您的金鑰管理服務 (KMS) 報告的計數不足。 請連絡您的系統管理員。

#### <a name="possible-cause"></a>可能的原因

KMS 主機上的計數不足。 若為 Windows Server，KMS 計數必須大於或等於 5。 若為 Windows (用戶端)，KMS 計數必須大於或等於 25。

#### <a name="resolution"></a>解決方法
在您可以使用 KMS 來啟動 Windows 之前，KMS 集區中必須有更多電腦。 若要取得 KMS 主機上的最新計數，請執行 **Slmgr.vbs /dli**。

### <a name="0xc004f039-the-key-management-service-kms-is-not-enabled"></a>0xC004F039 金鑰管理服務 (KMS) 未啟用

此錯誤訊息的完整文字與下列類似：

> 軟體保護服務報告指出無法啟用電腦。 金鑰管理服務 (KMS) 未啟用。

#### <a name="possible-cause"></a>可能的原因

KMS 未回應 KMS 要求。

#### <a name="resolution"></a>解決方法

針對 KMS 主機與用戶端之間的網路連線進行疑難排解。 確定 TCP 連接埠 1688 (預設值) 並未被防火牆封鎖或過濾。

### <a name="0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated"></a>0xC004F041 T軟體保護服務判定金鑰管理伺服器 (KMS) 並未啟用

此錯誤訊息的完整文字與下列類似：

> 軟體保護服務判定金鑰管理伺服器 (KMS) 並未啟用。 必須啟用 KMS。

#### <a name="possible-cause"></a>可能的原因

KMS 主機未啟用。

#### <a name="resolution"></a>解決方法

使用線上或電話啟用方式來啟用 KMS 主機。

### <a name="0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used"></a>0xC004F042 軟體保護服務判定無法使用所指定的金鑰管理服務 (KMS)

#### <a name="possible-cause"></a>可能的原因

如果 KMS 用戶端連絡無法啟用用戶端軟體的 KMS 主機，便會發生此錯誤。 例如，在包含應用程式特定與作業系統特定 KMS 主機的混合式環境中，這種情況可能很常見。

#### <a name="resolution"></a>解決方法

確定如果您使用特定 KMS 主機來啟用特定的應用程式或作業系統，KMS 用戶端就會連線到正確的主機。

### <a name="0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid"></a>0xC004F050 軟體保護服務報告指出產品金鑰無效

#### <a name="possible-cause"></a>可能的原因

這可能是因為未正確輸入 KMS 金鑰，或在作業系統的發行版本輸入搶鮮版 (Beta) 金鑰。

#### <a name="resolution"></a>解決方法

在對應的 Windows 版本上安裝適當的 KMS 金鑰。 檢查拼字。 如果您以剪貼方式輸入金鑰，請確定金鑰中的長破折號沒有被取代為連字號。

### <a name="0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked"></a>0xC004F051 軟體保護服務報告指出產品金鑰被封鎖

#### <a name="possible-cause"></a>可能的原因

啟用伺服器判定 Microsoft 已封鎖產品金鑰。

#### <a name="resolution"></a>解決方法

取得新的 MAK 或 KMS 金鑰、在系統上安裝該金鑰，然後進行啟用。

### <a name="0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired"></a>0xC004F064 軟體保護服務報告指出非正版寬限期已過

#### <a name="possible-cause"></a>可能的原因

Windows 啟用工具 (WAT) 判定系統不是正版。

#### <a name="resolution"></a>解決方法

如需協助，請連絡 [Microsoft 授權啟用中心](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。

### <a name="0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period"></a>0xC004F065 軟體保護服務報告指出應用程式在有效的非正版期間執行

#### <a name="possible-cause"></a>可能的原因

Windows 啟用工具判定系統不是正版。 系統將會在非正版寬限期內繼續執行。

#### <a name="resolution"></a>解決方法

取得並安裝正版產品金鑰，並在寬限期內啟用系統。 否則，系統會在寬限期結束時進入「通知」狀態。

### <a name="0xc004f06c-the-request-timestamp-is-invalid"></a>0xC004F06C 要求時間戳記無效

此錯誤訊息的完整文字與下列類似：

> 軟體保護服務報告指出無法啟用電腦。 金鑰管理服務 (KMS) 判斷出要求時間戳記無效。

#### <a name="possible-cause"></a>可能的原因

用戶端電腦上的系統時間與 KMS 主機上的時間差異太大。 基於各種原因，時間同步對於系統與網路安全性而言非常重要。

#### <a name="resolution"></a>解決方法

請變更用戶端上的系統時間使其與 KMS 主機同步，以修正此問題。 建議使用網路時間通訊協定 (NTP) 時間來源或 Active Directory Domain Services 來進行時間同步。 此問題導因於使用 UTP 時間，而且與時區選擇無關。

### <a name="0xc004f074-no-key-management-service-kms-could-be-contacted"></a>0xC004F074 無法聯繫金鑰管理服務 (KMS)

此錯誤訊息的完整文字與下列類似：

> 軟體保護服務報告指出無法啟用電腦。 無法聯繫金鑰管理服務 (KMS)。 如需其他資訊，請參閱應用程式事件記錄檔。

#### <a name="possible-cause"></a>可能的原因

所有 KMS 主機系統都傳回錯誤。

#### <a name="resolution"></a>解決方法

在應用程式事件記錄中，找出具有事件識別碼 12288 並與啟用嘗試相關聯的每個事件。 針對這些事件的錯誤進行疑難排解。

如需針對 DNS 相關問題進行疑難排解的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。
