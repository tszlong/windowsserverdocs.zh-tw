---
title: 遠端桌面用戶端常見問題集
description: 遠端桌面用戶端的相關的常見問題集
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: e6f91aa02cd0f19d480c24309be5797c273b0f2e
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804956"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>遠端桌面用戶端的相關的常見問題集

>適用於：Windows 10，Windows 8.1、 Windows Server 2019、 Windows Server 2016、windows Server 2012 R2

既然您已設定遠端桌面用戶端，在您的裝置 （Android、 Mac、 iOS 或 Windows） 上，您可能有問題。 以下是有關遠端桌面用戶端最常問的問題的解答。 

- [設定](#setting-up)
- [連線、 閘道和網路](#connection-gateway-and-networks)
- [web 用戶端](#web-client)
- [監視、 音訊及滑鼠](#monitors-audio-and-mouse)
- [Mac 硬體](#mac-client---hardware-questions)
- [特定的錯誤訊息](#specific-errors)

這些問題大多適用於所有的用戶端，但有少數的用戶端特定項目。

如果您有其他想要我們回答的問題，讓它們保持為意見上這篇文章。

## <a name="setting-up"></a>設定

### <a name="which-pcs-can-i-connect-to"></a>若要連接哪些電腦？

請參閱[支援組態](remote-desktop-supported-config.md)您可以連線到哪些電腦的相關資訊的文章。

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>我該如何針對遠端桌面設定的電腦？

我在我的裝置設定，但是我認為的電腦已準備好。 幫助嗎？

首先，您有看到遠端桌面安裝精靈嗎？ 它會引導您準備好您的電腦進行遠端存取。 下載並執行的工具在您的電腦，以取得所有項目上設定。 

否則，如果您想要以手動方式執行動作，繼續閱讀。

適用於 Windows 10 中，執行下列作業：

1. 在您想要連接到裝置上，開啟**設定**。
2. 選取 **系統**，然後**遠端桌面**。
3. 您可以使用滑桿來啟用遠端桌面。
4. 一般情況下，最好是保持電腦，甦醒狀態與可探索，以利於進行連線。 按一下 **顯示設定**移至您的電腦，您可以在其中變更此設定的電源設定。
   > [!NOTE]
   > 您無法連線至睡眠的電腦或休眠狀態，因此請確定設定睡眠與休眠遠端電腦設定為**永不**。 （休眠不提供在所有電腦上）。


請記下此電腦的名稱**如何連線到此 PC**。 您將需要此項來設定用戶端。

您可以授與存取此 PC-這麼做，請按一下 特定使用者的權限**選取即可從遠端存取此電腦的使用者**。
系統管理員群組的成員自動擁有存取權。

Windows 8.1，請遵循以允許遠端連線中的指示[連接到使用遠端桌面連線的其他桌面](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8)。



## <a name="connection-gateway-and-networks"></a>連接、 閘道和網路

### <a name="why-cant-i-connect-using-remote-desktop"></a>為何我無法連線使用遠端桌面？

以下是一些可能的解決方案，嘗試連線到遠端電腦時，可能會遇到的常見問題。 如果這些解決方案沒有作用，您可以找到更多協助[Microsoft Community 網站](https://go.microsoft.com/fwlink/p/?LinkId=242079)。

- **找不到遠端電腦。** 請確定您有權限的電腦名稱，然後查看是否您已正確輸入該名稱。 如果您仍然無法連線，請嘗試使用遠端電腦的 IP 位址，而不電腦名稱。
- **沒有網路有問題。** 請確定您有網際網路連線。 
- **遠端桌面連接埠可能會被防火牆封鎖。** 如果您使用 Windows 防火牆，請遵循下列步驟：

  1. 開啟 [Windows 防火牆]。 
  2. 按一下 **允許應用程式或功能通過 Windows 防火牆**。 
  3. 按一下 **變更設定**。 您可能會要求系統管理員密碼，或確認您的選擇。
  4. 底下**允許應用程式和功能**，選取**遠端桌面**，然後點選或按一下 **確定**。

     如果您使用不同的防火牆，請確定已開啟遠端桌面 (通常為 3389) 的連接埠。
- **遠端連接可能未設定遠端電腦。** 若要修正此問題，往回捲動至[如何設定電腦的遠端桌面？](#how-do-i-set-up-a-pc-for-remote-desktop)本主題中的問題。
- **遠端電腦可能只允許網路層級驗證設定用來連接電腦。** 
- **遠端電腦可能已關閉。** 您無法連接到已關閉、 睡眠、 電腦或休眠狀態，因此請確定設定睡眠與休眠遠端電腦設定為**永不**（休眠無法在所有電腦上。）。

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>為什麼無法找到或連線到我的電腦嗎？

檢查下列項目：

- 是電腦上和甦醒狀態？
- 您未輸入正確的名稱或 IP 位址？

   > [!IMPORTANT]
   > 使用電腦名稱，需要您網路的正確透過 DNS 名稱解析。 在許多家用網路中，您必須連接，而不是主機名稱使用的 IP 位址。
- 電腦位於不同的網路嗎？ 您是否有設定，讓外部連線到電腦？  請參閱[允許您從網路外部的電腦存取](remote-desktop-allow-outside-access.md)取得協助。
- 您要連線到支援的 Windows 版本？ 

   > [!NOTE]
   > 第 3 方軟體沒有不支援 Windows XP 家用版、 Windows Media Center Edition、 Windows Vista Home 和 Windows 7 Home 或入門。

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>為什麼無法我登入遠端電腦？

如果您所見的遠端電腦的 [登入] 畫面，但您無法登入，您可能會有尚未加入至遠端桌面使用者群組，或在遠端電腦上的系統管理員權限的任何群組。 請詢問您若要這樣做為您的系統管理員。

### <a name="which-connection-methods-are-supported-for-company-networks"></a>支援公司網路的連線方法？

如果您想要存取您的 office 桌面從公司網路外部時，您的公司必須提供您一種遠端存取。 RD 用戶端目前支援以下功能：

- 終端機伺服器閘道或遠端桌面閘道
- 遠端桌面 Web 存取
- VPN （透過內建 VPN 選項 iOS）

### <a name="vpn-doesnt-work"></a>VPN 無法運作

VPN 問題可以有數個原因。 第一個步驟是確認 VPN，適用於 PC 或 Mac 電腦與相同的網路。 如果您不能測試在 PC 或 Mac，您可以嘗試存取公司內部網路網頁，以您的裝置瀏覽器。

若要檢查其他項目：
- **3g 網路封鎖或損毀 VPN。** 有數個 3g 提供者的世界中似乎區塊或損毀 3g 流量。 確認 VPN 連線可正常使用超過一分鐘。
- **L2TP 或 PPTP Vpn。** 如果您使用 L2TP 或 PPTP，在您的 VPN，請傳送所有流量中都設為 ON 的 VPN 組態。
- **VPN 的設定不正確。** 設定錯誤的 VPN 伺服器可以是 VPN 連線，永遠不會處理或原因段時間後停止運作。 請確定測試使用 iOS 裝置的網頁瀏覽器，或在 PC 或 Mac，在相同網路上，如果發生這種情況。

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>如何測試 VPN 是否正常運作？

請確認您的裝置上，會啟用 VPN。 您可以前往您的內部網路上的網頁，或使用 web 服務，僅可透過 VPN，以測試您的 VPN 連線。

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>如何設定 L2TP 或 PPTP VPN 連線？

如果您使用 L2TP 或 PPTP 在您的 VPN，請務必設定**傳送所有流量**要**ON** VPN 組態中。

## <a name="web-client"></a>web 用戶端

### <a name="which-browsers-can-i-use"></a>可以使用哪些瀏覽器？

Web 用戶端支援 Microsoft Edge、 Internet Explorer 11，Mozilla Firefox (v55.0 和更新版本)、 Safari 和 Google Chrome。

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>若要存取 web 用戶端可以使用哪些電腦？

Web 用戶端支援 Windows、 macOS、 Linux 和 ChromeOS。 在此階段不支援行動裝置。

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>可以使用 web 用戶端在遠端桌面部署中不需要閘道嗎？

資料分割 用戶端需要遠端桌面閘道連線。 不知道什麼意思？ 您的系統管理員詢問它。

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>遠端桌面 web 用戶端是否會取代 [遠端桌面 Web 存取] 頁面？

資料分割 遠端桌面 web 用戶端會裝載於不同的 URL 比 [遠端桌面 Web 存取] 頁面。 若要在瀏覽器中檢視的遠端資源，您可以使用 web 用戶端或 Web 存取的頁面。

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>可以在另一個網頁中內嵌 web 用戶端？

目前不支援此功能。

## <a name="monitors-audio-and-mouse"></a>監視、 音訊及滑鼠

### <a name="how-do-i-use-all-of-my-monitors"></a>如何監視我的所有使用？
若要使用兩個或多個畫面，執行下列作業：

1. 以滑鼠右鍵按一下您想要啟用多個畫面，然後按一下 遠端桌面**編輯**。
2. 啟用**使用所有監視器**並**全螢幕**。

### <a name="is-bi-directional-sound-supported"></a>支援雙向聲音？
遠端桌面用戶端不支援上游 （從用戶端到伺服器，如麥克風） 的聲音。

### <a name="what-can-i-do-if-the-sound-wont-play"></a>如果將不會播放聲音，我可以做什麼？
登出工作階段 （不只是中斷連接，一路登出），然後再次登入。

## <a name="mac-client---hardware-questions"></a>Mac 用戶端-硬體問題
### <a name="is-retina-resolution-supported"></a>Retina 解析度支援？
是，遠端桌面用戶端支援 retina 解析度。

### <a name="how-do-i-enable-secondary-right-click"></a>如何啟用次要以滑鼠右鍵按一下？
為了讓使用 「 按一下滑鼠右鍵，在開啟的工作階段，您有三個選項：

- 標準 PC 兩個 USB 滑鼠
- Apple Magic 滑鼠：若要啟用按一下滑鼠右鍵，按一下**系統偏好設定**在 dock 中，按一下**滑鼠**，然後再啟用**次要按一下**。
- Apple Magic 軌跡板或 MacBook 軌跡板：若要啟用按一下滑鼠右鍵，按一下**系統偏好設定**在 dock 中，按一下**滑鼠**，然後再啟用**次要按一下**。

### <a name="is-airprint-supported"></a>AirPrint 支援？
否，「 遠端桌面用戶端不支援 AirPrint。 （這是適用於 Mac 和 iOS 的用戶端）。

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>為什麼並不正確的字元會顯示在工作階段？
如果您使用的國際鍵盤，您可能會發現問題出現在工作階段中的字元進行比對字元 Mac 鍵盤上鍵入。

這可能會發生在下列情況：

- 使用 遠端工作階段無法辨識鍵盤。 當遠端桌面無法辨識鍵盤時，它會預設為遠端電腦上一次使用的語言。
- 您要連接到遠端電腦上先前中斷連線工作階段和遠端電腦使用不同的鍵盤語言的語言比您目前嘗試使用。

您可以手動將鍵盤語言設定的遠端工作階段來修正此問題。 請參閱下一節中的步驟。

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>語言設定如何影響鍵盤在遠端工作階段？
有許多種 Mac 鍵盤配置。 其中有些是 Mac 特定版面配置或為其完全符合可能無法使用的是遠端處理到的 Windows 版本上的自訂配置。 遠端工作階段會比對遠端電腦上可用的鍵盤語言的最佳對應鍵盤。 

如果您的 Mac 鍵盤配置設定為您的金鑰應該正確對應語言鍵盤 （例如，法文 – 電腦） 和鍵盤的 PC 版本應該行得通。

如果您的 Mac 鍵盤配置設定鍵盤 （例如，法文） 的 Mac 版的遠端工作階段會對應您 PC 版本的法文語言。 Mac 鍵盤快速鍵，您用來在 OSX 上使用的一些無法在遠端的 Windows 工作階段。

如果您的鍵盤配置設定為一種語言 （例如，加拿大法文） 的一種變化，而且遠端工作階段無法將您對應至該確切的變化，遠端工作階段會將您對應至最接近的語言 （例如，法文）。 Mac 鍵盤快速鍵，您用來在 OSX 上使用的一些無法在遠端的 Windows 工作階段。

如果您的鍵盤配置設定為遠端工作階段不能完全符合的版面配置時，遠端工作階段會預設為讓您上一次使用與該電腦的語言。 在此情況下，或在您要變更以符合您的 Mac 鍵盤，遠端工作階段的語言的情況下您可以手動設定鍵盤語言的語言會最符合您想要使用，如下所示的一個遠端工作階段中。

若要變更鍵盤配置內的遠端桌面工作階段中使用下列指示：

**在 Windows 10 或 Windows 8:**

1. 從在遠端工作階段中，開啟 地區及語言。 按一下 **開始 > 設定 > 時間與語言**。 開啟**地區和語言**。
2. 新增您想要使用的語言。 然後關閉 [地區及語言] 視窗。
3. 現在，遠端工作階段中，您會看到語言之間切換的功能。 （在右側的遠端工作階段中，時鐘附近。）按一下您想要切換到的語言 (例如**Eng**)。

您可能需要關閉並重新啟動應用程式，您目前使用的鍵盤變更才會生效。


## <a name="specific-errors"></a>特定的錯誤

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>為什麼會收到 「 權限不足 」 錯誤？
您不允許存取您想要連線到工作階段。 最可能的原因是您嘗試連線到系統管理工作階段。 只有系統管理員才能連線到主控台。 請確認主控台開關已關閉遠端桌面的進階設定中。 如果這不是問題的來源，請連絡您的系統管理員以取得進一步協助。

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>用戶端為何顯示為沒有 CAL？
當遠端桌面用戶端連線到遠端桌面伺服器時，伺服器就會發出遠端桌面服務用戶端存取使用權 (RDS CAL) 儲存用戶端。 只要用戶端連線一次，它會使用其 RDS CAL，而且伺服器不會發出另一個授權。 如果在裝置上的 RDS CAL 會遺失或損毀，伺服器會發出另一個授權。 當授權的裝置數目上限為止伺服器不會發出新的 RDS Cal。 請連絡網路系統管理員尋求協助。

### <a name="why-did-i-get-an-access-denied-error"></a>為什麼我會得到 「 拒絕存取 」 錯誤？
嘗試連接期間，「 拒絕存取 」 錯誤是由遠端桌面閘道和認證不正確結果的產生。 請確認您的使用者名稱和密碼。 如果連接之前，且最近發生的錯誤，您可能變更 Windows 使用者帳戶密碼，而且您尚未它尚未更新中的遠端桌面設定。

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>「 RPC 錯誤 23014 」 或 「 錯誤 0x59e6 」 平均值的功能？
中的情況下**RPC 錯誤 23014**或是**之後等候幾分鐘後重試錯誤 0x59E6**，RD 閘道伺服器已達到使用中連接數目上限。 根據 Windows 版本在 RD 閘道上執行的連線數目上限與不同：Windows Server 2008 R2 Standard 實作限制為 250 的連線數目。 Windows Server 2008 R2 Foundation 實作限制為 50 的連線數目。 所有其他的 Windows 實作允許無限的數目的連線。

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>「 剖析 NTLM 挑戰失敗 」 錯誤代表什麼意思？
此錯誤被因為在遠端電腦上設定不正確。 請確定 RDP 安全性層級設定，在遠端電腦上的設定為 「 用戶端相容 」。 （請連絡您的系統管理員如果您需要執行此動作的說明。）

### <a name="what-does-tsrap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>功能"TS_RAP 不允許您連接到指定的主控件 」 表示？
當閘道伺服器上的資源授權原則會停止您的使用者名稱，無法連線到遠端電腦時，就會發生此錯誤。 這種情形會在下列情況：

- 遠端電腦名稱是與閘道的名稱相同。 然後，當您嘗試連接到遠端電腦，連線到閘道反而可能沒有存取權限。 如果您需要連線到閘道，不要使用外部的閘道名稱作為電腦名稱。 改為使用"localhost"或 IP 位址 (127.0.0.1) 或內部伺服器名稱。
- 您的使用者帳戶不是遠端存取的使用者群組的成員。