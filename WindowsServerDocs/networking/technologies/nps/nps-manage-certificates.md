---
title: 管理搭配 NPS 使用的憑證
description: 本主題提供與 Windows Server 2016 中的網路原則伺服器使用伺服器憑證的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73f3d6a1e9dc6ae1520b1d685b6b05b5f3aed601
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864229"
---
# <a name="manage-certificates-used-with-nps"></a>管理搭配 NPS 使用的憑證

>適用於：Windows Server （半年通道），Windows Server 2016

如果您部署以憑證為基礎的驗證方法，例如可延伸驗證通訊協定\-傳輸層安全性\(EAP\-TLS\)，受保護的可延伸驗證通訊協定\-傳輸層安全性\(PEAP\-TLS\)，與 PEAP\-Microsoft Challenge Handshake 驗證通訊協定第 2 版\(MS\-MS-CHAP v2\)，您必須在所有您 NPSs 註冊伺服器憑證。 伺服器憑證必須︰

- 符合最低的伺服器憑證需求，如中所述[設定 PEAP 和 EAP 需求的憑證範本](nps-manage-cert-requirements.md)

- 發出的憑證授權單位\(CA\)用戶端電腦信任。 當其憑證存在於目前的使用者和本機電腦的受信任的根憑證授權單位憑證存放區時，CA 是受信任。

下列指示協助您管理在部署位置受信任的根 CA 是第三方 CA，例如 Verisign，或您已部署公開金鑰基礎結構的 CA 中的 NPS 憑證\(PKI\)使用作用中Directory 憑證服務\(AD CS\)。

## <a name="change-the-cached-tls-handle-expiry"></a>變更快取的 TLS 控制碼到期

在初始驗證程序中的 EAP\-TLS、 PEAP\-TLS 與 PEAP\-MS\-MS-CHAP v2，NPS 會快取連線的用戶端的 TLS 連線內容的一部分。 用戶端也會快取的 NPS 的 TLS 連線內容的一部分。

這些的 TLS 連線內容的每個個別集合稱為 TLS 控制碼。

用戶端電腦可以快取的 TLS 控制碼多個驗證器，而 NPSs 可以快取許多用戶端電腦的 TLS 控制代碼。

在用戶端和伺服器上快取的 TLS 控制碼允許進行更快速的重新驗證程序。 比方說，當無線電腦會重新驗證與 NPS 中，NPS 可供無線用戶端檢查 TLS 控制碼，並可以快速決定用戶端連線為重新連線。 NPS 授權連線，而不執行完整驗證。

同樣地，用戶端檢查 nps 的 TLS 控制碼，判斷它是重新連接時，並不需要執行伺服器驗證。

在電腦上執行 Windows 10 和 Windows Server 2016，預設 TLS 控制碼到期日為 10 小時。

在某些情況下，您可能想要增加或減少的 TLS 控制碼到期時間。

比方說，您可以減少在其中的使用者憑證已撤銷系統管理員和憑證已過期的情況下的 TLS 控制碼到期時間。 在此案例中，使用者可以仍然連接到網路 NPS 是否尚未過期的快取的 TLS 控制代碼。 減少 TLS 控制碼到期可能有助於防止這類使用者，使用已撤銷的憑證重新連接。

>[!NOTE]
>此案例的最佳解決方案是停用 Active Directory 中的使用者帳戶，或從 Active Directory 群組被授與權限連接到網路原則中的網路中移除的使用者帳戶。 所有網域控制站的這些變更的傳播可能也會延遲，不過，由於複寫延遲。 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>設定用戶端電腦上的 TLS 控制碼到期時間

您可以使用此程序，變更用戶端電腦快取 NPS 的 TLS 控制碼的時間量。 成功驗證之後 NPS，用戶端電腦會快的 NPS 的 TLS 連線內容取做為 TLS 控制代碼。 TLS 控制碼具有預設持續時間為 10 小時\(36,000,000 毫秒\)。 您可以增加或減少的 TLS 控制碼到期時間，使用下列程序。

中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。

>[!IMPORTANT]
>在 NPS 中，不是在用戶端電腦上，必須執行此程序。

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>若要設定 TLS 處理用戶端電腦上的到期時間

1. 在 NPS 中，開啟 [登錄編輯器]。

2. 瀏覽至登錄機碼**HKEY\_本機\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在上**編輯**功能表上，按一下**新增**，然後按一下 **金鑰**。

4. 型別**ClientCacheTime**，然後按 ENTER 鍵。

5. 以滑鼠右鍵按一下**ClientCacheTime**，按一下**新增**，然後按一下**DWORD （32 位元） 值**。

6. 輸入的時間，以毫秒為單位，您想要用戶端電腦快取資訊，請參閱 NPS 的 TLS 控制代碼之後第一個成功的驗證嘗試的 nps。

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>NPSs 上設定的 TLS 控制碼到期時間

您可以使用此程序來變更 NPSs 快取的用戶端電腦的 TLS 控制碼的時間量。 已成功驗證之後存取用戶端，NPSs 快為 TLS 控制碼取 TLS 用戶端電腦的連接屬性。 TLS 控制碼具有預設持續時間為 10 小時\(36,000,000 毫秒\)。 您可以增加或減少的 TLS 控制碼到期時間，使用下列程序。

中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。

>[!IMPORTANT]
>在 NPS 中，不是在用戶端電腦上，必須執行此程序。

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>若要設定的 TLS 控制碼 NPSs 到期時間

1. 在 NPS 中，開啟 [登錄編輯器]。

2. 瀏覽至登錄機碼**HKEY\_本機\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在上**編輯**功能表上，按一下**新增**，然後按一下 **金鑰**。

4. 型別**ServerCacheTime**，然後按 ENTER 鍵。

5. 以滑鼠右鍵按一下**ServerCacheTime**，按一下**新增**，然後按一下**DWORD （32 位元） 值**。

6. 輸入的時間，以毫秒為單位，您想要 NPSs 之後第一個成功的驗證嘗試的用戶端快取的用戶端電腦的 TLS 控制代碼。

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>取得受信任的根 CA 憑證的 sha-1 雜湊

您可以使用此程序，從本機電腦已安裝的憑證取得安全雜湊演算法 (sha-1) 的受信任的根憑證授權單位 (CA) 的雜湊。 在某些情況下，例如在部署群組原則時，就必須將憑證指定使用憑證的 sha-1 雜湊。

使用群組原則時，您可以指定一或多個的受信任的根 CA 憑證用戶端必須使用才能驗證 NPS 與 EAP 或 PEAP 的相互驗證的過程。 若要指定受信任的根 CA 憑證用戶端必須用來驗證伺服器憑證，您可以輸入憑證的 sha-1 雜湊。

此程序示範如何使用憑證 Microsoft Management Console (MMC) 嵌入式管理單元取得 sha-1 雜湊的受信任的根 CA 憑證。 

若要完成此程序，您必須隸屬**使用者**群組在本機電腦上。

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>若要取得 sha-1 雜湊的受信任的根 CA 憑證

1. 在 執行 對話方塊或 Windows PowerShell 中，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console \(MMC\)隨即開啟。 在 MMC 中，按一下**檔案**，然後按一下**新增/移除 Snap\in**。 **新增或移除嵌入式管理單元**對話方塊隨即開啟。

2. 在 **新增或移除嵌入式管理單元**，請在**可用的嵌入式管理單元**，連按兩下**憑證**。 憑證嵌入式管理單元精靈 隨即開啟。 按一下 [**電腦帳戶**，然後按一下**下一步]**。

3. 在**選取電腦**，請確認**本機電腦 （執行這個主控台的電腦）** 是選取狀態，按一下**完成**，然後按一下  **確定**.

4. 在左窗格中，按兩下**Certificates (Local Computer)**，然後按兩下**受信任的根憑證授權單位**資料夾。

5. **憑證**資料夾是子資料夾的**受信任的根憑證授權單位**資料夾。 按一下 **憑證**資料夾。

6. 在 [詳細資料] 窗格中，瀏覽至您信任的根 CA 的憑證。 按兩下憑證。 **憑證**對話方塊隨即開啟。

7. 在 **憑證** 對話方塊中，按一下**詳細資料** 索引標籤。

8. 在欄位清單中，捲動至並選取**指紋**。

9. 在下方窗格中，會顯示您的憑證的 sha-1 雜湊的十六進位字串。 選取 sha-1 雜湊，然後按 Copy 命令 Windows 鍵盤快速鍵\(CTRL\+C\)複製到 Windows 剪貼簿的雜湊。

10. 開啟您想要貼上 sha-1 雜湊、 正確地找出資料指標，然後按 貼上命令 Windows 鍵盤快速鍵的位置\(CTRL\+V\)。 

如需有關憑證與 NPS 的詳細資訊，請參閱[設定 PEAP 和 EAP 需求的憑證範本](nps-manage-cert-requirements.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
