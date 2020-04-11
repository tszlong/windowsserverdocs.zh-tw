---
title: 遠端桌面用戶端常見問題集
description: 關於遠端桌面用戶端的常見問題集
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: a91980477b34bb529e3e6f3c6ff66da9ea7f3c84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856051"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>關於遠端桌面用戶端的常見問題集

>適用於：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

現在您已在裝置 (Android、Mac、iOS 或 Windows) 上設定遠端桌面用戶端，您可能會有疑問。 以下是有關遠端桌面用戶端最常見問題的解答。 

- [設定](#setting-up)
- [連線、閘道和網路](#connection-gateway-and-networks)
- [Web 用戶端](#web-client)
- [監視器、音訊和滑鼠](#monitors-audio-and-mouse)
- [Mac 硬體](#mac-client---hardware-questions)
- [特定錯誤訊息](#specific-errors)

這些問題大多都是所有用戶端都可能會有的問題，但少數問題則只會在特定用戶端上出現。

如果您有其他問題需要我們回答，請在本文的意見反應中提出。

## <a name="setting-up"></a>設定

### <a name="which-pcs-can-i-connect-to"></a>我可以連線至哪些電腦？

請參閱[支援的設定](remote-desktop-supported-config.md)一文，以了解您可以連線至哪些電腦。

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>如何為電腦進行遠端桌面設定？

我已設定好裝置，但我認為電腦尚未就緒。 需要協助嗎？

首先，您是否看到了遠端桌面設定精靈？ 它會引導您為電腦做好遠端存取的準備。 請在您的電腦下載並執行該工具，將一切設定妥當。 

否則，如果您想要手動執行，請繼續往下閱讀。

在 Windows 10 上，請執行下列作業：

1. 在您要連接的裝置上，開啟 [設定]  。
2. 選取 [系統]  ，然後選取 [遠端桌面]  。
3. 使用滑桿來啟用遠端桌面。
4. 一般而言，最好將電腦保持在警醒且可探索的狀態，以利進行連線。 按一下 [顯示設定]  移至電腦的電源設定，以便變更此設定。
   > [!NOTE]
   > 您無法連線至處於睡眠或休眠狀態的電腦，因此請確定遠端電腦的睡眠和休眠設為 [永不]  。 (並非所有電腦都有休眠功能。)


請在 [如何連線至此電腦]  下方記下此電腦的名稱。 您將需要用這項資訊來設定用戶端。

您可以為特定使用者授與存取此電腦的權限 - 若要這麼做，請按一下 [選取可從遠端存取此電腦的使用者]  。
系統管理員群組的成員會自動取得存取權。

在 Windows 8.1 上，請依照[使用遠端桌面連線來連線至其他桌面](https://support.microsoft.com/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8)中的指示，允許遠端連線。



## <a name="connection-gateway-and-networks"></a>連線、閘道和網路

### <a name="why-cant-i-connect-using-remote-desktop"></a>為何我無法使用遠端桌面連線？

以下是您在嘗試連線至遠端電腦遇到常見問題時可採行的一些解決方案。 如果這些解決方案沒有作用，您可以在 [Microsoft 社群網站](https://go.microsoft.com/fwlink/p/?LinkId=242079)尋求進一步協助。

- **找不到遠端電腦。** 請確定您具有正確的電腦名稱，然後查看是否您已正確輸入該名稱。 如果仍然無法連線，請嘗試使用遠端電腦的 IP 位址，而不使用電腦名稱。
- **網路有問題。** 請確定您有網際網路連線。 
- **遠端桌面連接埠可能會被防火牆封鎖。** 如果您使用 Windows 防火牆，請遵循下列步驟：

  1. 開啟 [Windows 防火牆]。 
  2. 按一下 [允許應用程式或功能通過 Windows 防火牆]  。 
  3. 按一下 [變更設定]  。 系統可能會要求您輸入系統管理員密碼，以確認您的選擇。
  4. 在 [允許應用程式和功能]  下方選取 [遠端桌面]  ，然後點選或按一下 [確定]  。

     如果您使用不同的防火牆，請確定遠端桌面的連接埠 (通常為 3389) 已開啟。
- **遠端電腦上可能未設定遠端連線。** 若要修正此問題，請往回參考本主題中的[如何為電腦進行遠端桌面設定？](#how-do-i-set-up-a-pc-for-remote-desktop)問題。
- **遠端電腦可能僅允許電腦連接已設定網路層級驗證的目標。** 
- **遠端電腦可能關閉。** 您無法連線至處於關閉、睡眠或休眠狀態的電腦，因此請確定遠端電腦的睡眠和休眠設為 [永不]  (並非所有電腦都有休眠功能)。

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>為何找不到我的電腦或無法加以連線？

檢查下列項目：

- 電腦是否開啟並處於警醒狀態？
- 您是否輸入正確的名稱或 IP 位址？

   > [!IMPORTANT]
   > 要使用電腦名稱，您的網路必須能透過 DNS 正確解析名稱。 在許多家用網路中，您必須使用 IP 位址來連線，而不是主機名稱。
- 電腦是否位於不同的網路上？ 您是否將電腦設定成允許外部連線通過？  如需說明，請參閱[允許從網路外部存取您的電腦](remote-desktop-allow-outside-access.md)。
- 您是否連線至支援的 Windows 版本？ 

   > [!NOTE]
   > 在沒有第三方軟體的情況下，不支援 Windows XP Home、Windows Media Center Edition、Windows Vista Home、Windows 7 家用版或簡易版。

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>為何無法登入遠端電腦？

如果您可以看到遠端電腦的登入畫面，但無法登入，您可能尚未新增至遠端電腦上的遠端桌面使用者群組，或任何具有系統管理員權限的群組。 請要求系統管理員為您新增。

### <a name="which-connection-methods-are-supported-for-company-networks"></a>公司網路支援哪些連線方法？

如果您想要從公司網路外部存取辦公室桌面，您的公司必須提供遠端存取機制。 RD 用戶端目前支援下列功能：

- 終端機伺服器閘道或遠端桌面閘道
- 遠端桌面 Web 存取
- VPN (透過 iOS 內建 VPN 選項)

### <a name="vpn-doesnt-work"></a>VPN 無法運作

VPN 問題可能由數個原因所導致。 第一個步驟是確認 VPN 可在與電腦或 Mac 電腦相同的網路上運作。 如果您無法對電腦或 Mac 進行測試，您可以嘗試以裝置的瀏覽器存取公司的內部網路網頁。

須確認的其他事項：
- **3G 網路封鎖或阻斷 VPN。** 市面上有數個 3G 提供者似乎會封鎖或阻斷 3G 流量。 請確認 VPN 連線可正常運作超過一分鐘。
- **L2TP 或 PPTP VPN。** 如果您在 VPN 中使用 L2TP 或 PPTP，請在 VPN 設定中將 [傳送所有流量] 設為 [開啟]。
- **VPN 設定錯誤。** 設定錯誤的 VPN 伺服器可能是 VPN 連線完全無法運作或在一段時間後停止運作的原因。 如果發生這種情況，請確實以 iOS 裝置的網頁瀏覽器或相同網路上的電腦或 Mac 進行測試。

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>如何測試 VPN 是否正常運作？

確認您的裝置上已啟用 VPN。 您可以前往內部網路上的網頁，或使用只能透過 VPN 進行連線的 Web 服務，來測試您的 VPN 連線。

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>如何設定 L2TP 或 PPTP VPN 連線？

如果您在 VPN 中使用 L2TP 或 PPTP，請確實在 VPN 設定中將 [傳送所有流量]  設為 [開啟]  。

## <a name="web-client"></a>Web 用戶端

### <a name="which-browsers-can-i-use"></a>我可以使用哪些瀏覽器？

Web 用戶端支援 Microsoft Edge、Internet Explorer 11、Mozilla Firefox (v55.0 和更新版本)、Safari 和 Google Chrome。

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>可以使用哪些電腦來存取 Web 用戶端？

Web 用戶端支援 Windows、macOS、Linux 和 ChromeOS。 目前不支援行動裝置。

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>是否可在沒有閘道的遠端桌面部署中使用 Web 用戶端？

不可以。 用戶端必須要有遠端桌面閘道才能連線。 不知道這是什麼意思嗎？ 請詢問您的系統管理員。

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>遠端桌面 Web 用戶端是否會取代遠端桌面 Web 存取頁面？

不可以。 遠端桌面 Web 用戶端與遠端桌面 Web 存取頁面裝載於不同的 URL。 您可以使用 Web 用戶端或 Web 存取頁面，在瀏覽器中檢視遠端資源。

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>是否可將 Web 用戶端內嵌在另一個網頁中？

目前不支援此功能。

## <a name="monitors-audio-and-mouse"></a>監視器、音訊和滑鼠

### <a name="how-do-i-use-all-of-my-monitors"></a>如何使用我所有的監視器？
若要使用兩個或更多畫面，請執行下列作業：

1. 以滑鼠右鍵按一下要啟用多個畫面的遠端桌面，然後按一下 [編輯]  。
2. 啟用 [使用所有監視器]  和 [全螢幕]  。

### <a name="is-bi-directional-sound-supported"></a>是否支援雙向音效？
雙向音效可以在 Windows 用戶端針對每個連線進行設定。 您可以在 [本機資源]  選項索引標籤的 [遠端音訊]  區段中存取相關設定。

### <a name="what-can-i-do-if-the-sound-wont-play"></a>無法播放音效時，該如何處理？
登出工作階段 (不只是中斷連線，而應完全登出)，然後再次登入。

## <a name="mac-client---hardware-questions"></a>Mac 用戶端 - 硬體問題
### <a name="is-retina-resolution-supported"></a>是否支援視網膜解析度？
是，遠端桌面用戶端支援視網膜解析度。

### <a name="how-do-i-enable-secondary-right-click"></a>如何啟用滑鼠右鍵二次點按？
若要在開啟的工作階段內使用滑鼠右鍵點按功能，您有三個選項：

- 標準電腦雙按鈕 USB 滑鼠
- Apple Magic Mouse：若要啟用滑鼠右鍵點按功能，請按一下擴充座中的 [系統喜好設定]  ，再按一下 [滑鼠]  ，然後啟用 [二次點按]  。
- Apple Magic Trackpad 或 MacBook Trackpad：若要啟用滑鼠右鍵點按功能，請按一下擴充座中的 [系統喜好設定]  ，再按一下 [滑鼠]  ，然後啟用 [二次點按]  。

### <a name="is-airprint-supported"></a>是否支援 AirPrint？
否，遠端桌面用戶端不支援 AirPrint。 (Mac 和 iOS 用戶端都不支援)。

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>為何工作階段中出現不正確的字元？
如果您使用國際鍵盤，您可能會發現工作階段中出現的字元與您在 Mac 鍵盤上輸入的字元不符的問題。

這可能會在下列案例中發生：

- 您使用遠端工作階段無法辨識的鍵盤。 當遠端桌面無法辨識鍵盤時，則會預設為遠端電腦上一次使用的語言。
- 您連線至遠端電腦上先前中斷連線的工作階段，而遠端電腦所使用的鍵盤語言與您目前嘗試使用的語言不同。

您可以手動設定遠端工作階段的鍵盤語言，以修正此問題。 請參閱下一節中的步驟。

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>語言設定對遠端工作階段中的鍵盤有何影響？
Mac 鍵盤有許多類型的配置。 其中有些是 Mac 的特定配置或自訂配置，在您從遠端連線的 Windows 版本上可能沒有完全相符的項目。 遠端工作階段會將您的鍵盤對應至遠端電腦上最相符的可用鍵盤語言。 

如果您的 Mac 鍵盤配置設定為電腦版的語言鍵盤 (例如，法文 – 電腦)，則您所有的按鍵應該都會正確對應，且鍵盤應可正常運作。

如果您的 Mac 鍵盤配置設定為 Mac 版的鍵盤 (例如，法文)，則遠端工作階段會將您對應至電腦版的法文語言。 您在 OSX 上慣常使用的某些 Mac 鍵盤快速鍵，在遠端 Windows 工作階段中將無法運作。

如果您的鍵盤配置設定為某個語言 (例如，加拿大法文) 的變化，且遠端工作階段無法將您對應至該確切的變化，則遠端工作階段會將您對應至最接近的語言 (例如，法文)。 您在 OSX 上慣常使用的某些 Mac 鍵盤快速鍵，在遠端 Windows 工作階段中將無法運作。

如果您的鍵盤配置設定為遠端工作階段完全不相符的配置，則遠端工作階段會預設為讓您使用該電腦上一次使用的語言。 在此情況下，或是您需要變更遠端工作階段的語言以符合您的 Mac 鍵盤時，您可以手動將遠端工作階段中的鍵盤語言設定為與您想要使用的最相符的語言，如下所示。

若要變更遠端桌面工作階段中的鍵盤配置，請使用下列指示：

**在 Windows 10 或 Windows 8 上：**

1. 在遠端工作階段中，開啟 [地區及語言]。 按一下 [開始 > 設定 > 時間與語言]  。 開啟 [地區及語言]  。
2. 新增您要使用的語言。 然後，關閉 [地區及語言] 視窗。
3. 現在，在遠端工作階段中，您會看到切換語言的功能。 (在遠端工作階段右側的時鐘附近。)按一下您要切換到的語言 (例如 **Eng**)。

您可能需要關閉並重新啟動您目前使用的應用程式，鍵盤變更才會生效。


## <a name="specific-errors"></a>特定錯誤

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>為何會出現「權限不足」錯誤？
系統不允許您存取您要連線到的工作階段。 最可能的原因是您嘗試連線至系統管理工作階段。 只有系統管理員才能連線至主控台。 請在遠端桌面的進階設定中確認主控台開關已關閉。 如果這不是問題的成因，請連絡系統管理員以取得進一步協助。

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>為何用戶端指出沒有 CAL？
當遠端桌面用戶端連線至遠端桌面伺服器時，伺服器會發出遠端桌面服務用戶端存取使用權 (RDS CAL)，供用戶端儲存。 每當用戶端再次連線時，它都會使用其 RDS CAL，伺服器將不會再發出另一個授權。 當裝置上的 RDS CAL 遺失或損毀時，伺服器會發出另一個授權。 在達到授權的裝置數目上限時，伺服器即不會再發出新的 RDS CAL。 請連絡網路系統管理員以取得協助。

### <a name="why-did-i-get-an-access-denied-error"></a>為何會出現「拒絕存取」錯誤？
「拒絕存取」錯誤由遠端桌面閘道所產生，導因於連線嘗試期間的認證不正確。 請確認您的使用者名稱和密碼。 如果連線先前可運作，而最近才發生錯誤，則可能是您變更了 Windows 使用者帳戶密碼，但尚未在遠端桌面設定中加以更新。

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>「RPC 錯誤 23014」或「錯誤 0x59e6」有何意義？
發生 **RPC 錯誤 23014** 或**錯誤 0x59E6，請等候幾分鐘後再重試**時，表示 RD 閘道伺服器已達到作用中連線數目上限。 根據在 RD 閘道上執行的 Windows 版本，連線數目上限有所不同：Windows Server 2008 R2 Standard 實作將連線數目限制為 250 個。 Windows Server 2008 R2 Foundation 實作將連線數目限制為 50 個。 所有其他 Windows 實作皆允許無限制的連線數目。

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>「無法剖析 NTLM 挑戰」錯誤有何意義？
此錯誤是遠端電腦的設定不正確所導致。 請確定遠端電腦上的 RDP 安全性層級設定設為「用戶端相容」。 (如需相關協助，請洽詢系統管理員。)

### <a name="what-does-ts_rap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>「TS_RAP 不允許您連線至指定的主機」有何意義？
當閘道伺服器上的資源授權原則禁止您的使用者名稱連線至遠端電腦時，就會發生此錯誤。 這可能會在下列實例中發生：

- 遠端電腦名稱與閘道的名稱相同。 隨後，當您嘗試連線至遠端電腦時，連線反而轉向至閘道，但您可能沒有其存取權限。 如果您需要連線至閘道，請不要將外部閘道名稱作為電腦名稱。 請改為使用 "localhost" 或 IP 位址 (127.0.0.1)，或是內部伺服器名稱。
- 您的使用者帳戶不是遠端存取的使用者群組成員。
