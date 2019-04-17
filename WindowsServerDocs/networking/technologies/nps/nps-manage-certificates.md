---
title: 管理具有 NPS 所使用的憑證
description: 本主題提供與 Windows Server 2016 中的網路原則伺服器使用憑證伺服器的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2044cf30cc90c1673e05a1948ac9196d05940d1f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-used-with-nps"></a>管理具有 NPS 所使用的憑證

>適用於：Windows Server（以每年次管道）、Windows Server 2016

如果您的部署憑證為基礎的驗證方法、延伸驗證 Protocol\-Tls \(EAP\-TLS\)、受保護延伸驗證 Protocol\-傳輸層級的安全性 \(PEAP\-TLS\)，和 PEAP\ Microsoft 挑戰交換驗證通訊協定第 2 \ (MS\-CHAP v2\)，您必須到所有伺服器 NPS 都註冊伺服器的憑證。 必須伺服器的憑證：

- 中所述，符合最低伺服器的憑證需求[適用於 PEAP 和 EAP 需求設定憑證範本](nps-manage-cert-requirements.md)

- 憑證授權單位發行 \(CA\) 信任 client 的電腦。 CA 受信任，其憑證有目前使用者的本機電腦的受信任的根憑證授權單位憑證存放區。

下列指示協助管理的部署位置受信任的根 CA 是第三方 CA，例如 Verisign，或您已為您公用基礎結構部署 CA NPS 伺服器的憑證 \(PKI\) 使用 Active Directory 憑證服務 \(AD CS\)。

## <a name="change-the-cached-tls-handle-expiry"></a>變更快取的 TLS 控點到期

在初始驗證程序 EAP\ TLS、PEAP\ TLS 和 PEAP\-MS\-CHAP v2，NPS 伺服器快取連接 client 的 TLS 連接屬性的一部分。 Client 也會快取 NPS 伺服器的 TLS 連接屬性的一部分。

每個人的收藏這些 TLS 連接屬性稱為 TLS 控點。

Client 電腦可以快取的 TLS 控點多驗證器，而 NPS 伺服器可以快取的許多 client 電腦的 TLS 控點。

Client 伺服器上的快取的 TLS 控點可讓您更快速地重新驗證程序。 例如，wireless 電腦 reauthenticates 具有 NPS 伺服器，NPS 伺服器 wireless client 可以檢查的 TLS 控點與快速可以判斷 client 連接為重新連線。 NPS 伺服器授權連接而不需要執行完整的驗證。

相對地，client 檢查 NPS 伺服器 TLS 控點，、判斷它是重新連線，且不需要執行伺服器的驗證。

在電腦上執行 Windows 10 與 Windows Server 2016，預設 TLS 控點到期是 10 小時。

在某些情形下，您可能想要增加或減少 TLS 控點到期時間。

例如，您可以減少的 TLS 控點到期時間的位置，系統管理員的使用者的憑證已被撤銷和憑證已經過期的環境中。 在本案例中，使用者可以仍然連上網路如果 NPS 伺服器已過期不快取的 TLS 控點。 減少 TLS 控點到期可能協助防止撤銷憑證使用這類使用者重新連接。

>[!NOTE]
>本案例最好的方法是停用在 Active Directory 帳號，或帳號移除連上網路原則的網路的權限授與的 Active Directory 群組。 這些變更所有網域控制站的傳播可能也會延遲，不過，因為複寫延遲。 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>在 Client 電腦上設定的 TLS 控點到期時間

若要變更的時間用 client 電腦快取 NPS 伺服器的 TLS 控點，您可以使用此程序。 成功驗證 NPS 伺服器之後, client 電腦快 TLS 控點為取 NPS 伺服器的 TLS 連接屬性。 TLS 控點有 \(36,000,000 milliseconds\) 10 小時的預設時持續時間。 您可以增加或減少 TLS 控點到期時間，使用下列程序。

資格在**系統管理員**，或相當於，才能完成此程序最小值。

>[!IMPORTANT]
>必須在 NPS 伺服器，不 client 的電腦上執行此程序。

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>若要設定 TLS 處理 client 電腦上的到期時間

1. NPS 在伺服器上，開放作業系統。

2. 登錄鍵瀏覽**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在**編輯**功能表上，按**新**，，然後按一下 [**鍵**。

4. 輸入**ClientCacheTime**，然後按 ENTER 鍵。

5. 以滑鼠右鍵按一下**ClientCacheTime**，按一下 [**新**，然後按一下 [ **DWORD（32 位元）值**。

6. 輸入的時間，（毫秒），您想要 client 電腦之後的第一個成功驗證嘗試 NPS 伺服器快取 NPS 伺服器的 TLS 控點。

## <a name="configure-the-tls-handle-expiry-time-on-nps-servers"></a>NPS 伺服器上設定的 TLS 控點到期時間

若要變更的時間伺服器 NPS 快取 client 電腦的 TLS 控點，使用此程序。 在成功驗證存取 client 後, NPS 伺服器快為 TLS 控點取 client 電腦的 TLS 連接屬性。 TLS 控點有 \(36,000,000 milliseconds\) 10 小時的預設時持續時間。 您可以增加或減少 TLS 控點到期時間，使用下列程序。

資格在**系統管理員**，或相當於，才能完成此程序最小值。

>[!IMPORTANT]
>必須在 NPS 伺服器，不 client 的電腦上執行此程序。

### <a name="to-configure-the-tls-handle-expiry-time-on-nps-servers"></a>若要設定 TLS 處理 NPS 伺服器上的到期時間

1. NPS 在伺服器上，開放作業系統。

2. 登錄鍵瀏覽**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在**編輯**功能表上，按**新**，，然後按一下 [**鍵**。

4. 輸入**ServerCacheTime**，然後按 ENTER 鍵。

5. 以滑鼠右鍵按一下**ServerCacheTime**，按一下 [**新**，然後按一下 [ **DWORD（32 位元）值**。

6. 輸入的時間，（毫秒），您想要 NPS 伺服器成功驗證第一次嘗試透過 client 之後快取的 client 電腦的 TLS 控點。

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>取得 SHA-1 Hash 的受信任的根憑證

使用此程序的本機電腦已安裝的憑證取得的受信任的根憑證授權單位 hash 安全 Hash 演算法 (sha-1)。 在某些情形下，例如，在群組原則部署時，就必須使用 SHA-1 湊的憑證來指定憑證。

使用群組原則，您可以指定一或多個受信任的根 CA 憑證戶端必須使用以 EAP 或 PEAP 互加好友的驗證程序期間驗證 NPS 伺服器。 若要指定受信任的根憑證戶端必須使用驗證伺服器的憑證，您可以輸入 SHA-1 湊的憑證。

此程序示範如何使用的憑證 Microsoft Management Console (MMC) 嵌入式管理單元，以取得的受信任的根憑證 SHA-1 湊。 

若要完成此程序，您必須成員的**使用者**群組本機電腦上。

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>若要取得 SHA-1 hash 的 CA 信任的根憑證

1. 在對話方塊中執行或 Windows PowerShell 中，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console \(MMC\) 開啟。 在 MMC 中，按一下 [**檔案**，然後按**新增/移除 Snap\in**。 **中新增或移除嵌入式管理單元**對話方塊。

2. 在**中新增或移除嵌入式管理單元**，請在**可用嵌入式管理單元**，按兩下 [**憑證**。 嵌入式管理單元精靈開啟憑證。 按一下**電腦 account**，然後按一下 [**下**。

3. 在**選擇電腦**，確保**本機電腦（的電腦執行此主控台）**已選取，按一下 [**完成**，，然後按一下 [ **[確定]**。

4. 在左窗格中，按兩下 [**憑證（本機電腦）**，然後按兩下 [**受信任的根憑證授權單位**資料夾。

5. **的憑證**資料夾是子資料夾的**受信任的根憑證授權單位**資料夾。 按一下**的憑證**資料夾。

6. 在詳細資料窗格中，瀏覽您信任的根 CA 憑證。 按兩下憑證。 **憑證**對話方塊。

7. 在**憑證**對話方塊中，按**的詳細資料**索引標籤。

8. 在清單中的欄位，捲動到 [並選取 [**指紋**。

9. 在較低窗格中，會顯示為您的憑證 SHA-1 湊十六進位字串。 選取 SHA-1 hash，並為副本然後按下的 Windows 鍵盤快速鍵的命令 \(CTRL\+C\) 湊複製到剪貼簿 Windows。

10. 打開位置至您想要貼上 SHA-1 湊，正確地找出游標，然後按貼上的 Windows 鍵盤快速鍵的命令 \(CTRL\+V\)。 

如需有關憑證和 NPS 的詳細資訊，請查看[設定憑證範本 PEAP 和 EAP 需求的](nps-manage-cert-requirements.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
