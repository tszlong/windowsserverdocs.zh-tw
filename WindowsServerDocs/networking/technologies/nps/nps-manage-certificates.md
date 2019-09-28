---
title: 管理搭配 NPS 使用的憑證
description: 本主題提供在 Windows Server 2016 中使用伺服器憑證與網路原則伺服器的相關資訊。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b83f68b52a9cceef779e5204e295bbc9e45e7a14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396202"
---
# <a name="manage-certificates-used-with-nps"></a>管理搭配 NPS 使用的憑證

>適用於：Windows Server (半年度管道)、Windows Server 2016

如果您部署以憑證為基礎的驗證方法，例如可延伸的驗證通訊協定 @ no__t-0Transport 層安全性 \(EAP @ no__t-2TLS @ no__t-3，受保護的可延伸驗證通訊協定 @ no__t-4Transport 層安全性 @no__t 5PEAP @ no__t-6TLS @ no__t-7 和 PEAP @ no__t-8Microsoft 挑戰交握驗證通訊協定第2版 \(MS @ no__t-10CHAP v2 @ no__t-11，您必須向所有 Nps 註冊伺服器憑證。 伺服器憑證必須：

- 如[設定 PEAP 和 EAP 需求的憑證範本](nps-manage-cert-requirements.md)中所述，符合最低伺服器憑證需求

- 由用戶端電腦信任的憑證授權單位單位 \(CA @ no__t-1 發行。 當 CA 的憑證存在於目前使用者和本機電腦的「受信任的根憑證授權單位」憑證存放區時，就會信任 CA。

下列指示可協助您管理部署中的 NPS 憑證，其中信任的根 CA 是協力廠商 CA （例如 Verisign），或者是您已為公開金鑰基礎結構部署的 CA \(PKI @ no__t-1，方法是使用 Active Directory憑證服務 \(AD CS @ no__t-3。

## <a name="change-the-cached-tls-handle-expiry"></a>變更快取的 TLS 控制碼到期

在 EAP @ no__t-0TLS、PEAP @ no__t-1TLS 和 PEAP @ no__t-2 毫秒的初始驗證程式期間，NPS 會快取部分連線用戶端的 TLS 連接屬性。 用戶端也會快取一部分 NPS 的 TLS 連接屬性。

這些 TLS 連接屬性的每個個別集合稱為「TLS 控制碼」。

用戶端電腦可以快取多個驗證器的 TLS 控制碼，而 Nps 可以快取許多用戶端電腦的 TLS 控制碼。

用戶端和伺服器上的快取 TLS 控制碼可讓重新驗證程式更快速地進行。 例如，當使用 NPS 就重新驗證的無線電腦時，NPS 可以檢查無線用戶端的 TLS 控制碼，而且可以快速判斷用戶端連線是否為重新連接。 NPS 會授權連接，而不會執行完整驗證。

同樣地，用戶端會檢查 NPS 的 TLS 控制碼，判斷它是重新連線，而不需要執行伺服器驗證。

在執行 Windows 10 和 Windows Server 2016 的電腦上，預設的 TLS 控制碼到期時間為10小時。

在某些情況下，您可能會想要增加或減少 TLS 控制碼到期時間。

例如，您可能會想要在系統管理員撤銷使用者憑證，且憑證已過期的情況下，減少 TLS 控制碼到期時間。 在此案例中，如果 NPS 有尚未過期的快取 TLS 控制碼，則使用者仍然可以連接到網路。 減少 TLS 控制碼到期可能有助於防止這類具有撤銷憑證的使用者重新連接。

>[!NOTE]
>此案例的最佳解決方案是停用 Active Directory 中的使用者帳戶，或從授與許可權以連線到網路原則中網路的 Active Directory 群組中移除使用者帳戶。 不過，將這些變更傳播到所有網域控制站的工作，可能也會因為複寫延遲而延遲。 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>設定用戶端電腦上的 TLS 控制碼到期時間

您可以使用此程式來變更用戶端電腦快取 NPS 的 TLS 控制碼所需的時間。 成功驗證 NPS 之後，用戶端電腦會將 NPS 的 TLS 連接屬性快取為 TLS 控制碼。 TLS 控制碼的預設持續時間為10小時 \(36000000 毫秒 @ no__t-1。 您可以使用下列程式來增加或減少 TLS 控制碼到期時間。

若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。

>[!IMPORTANT]
>此程式必須在 NPS 上執行，而不是在用戶端電腦上。

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>設定用戶端電腦上的 TLS 控制碼到期時間

1. 在 NPS 上，開啟 [登錄編輯程式]。

2. 流覽至登錄機碼**HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在 [**編輯**] 功能表上，按一下 [**新增**]，然後按一下 [機**碼**]。

4. 輸入**ClientCacheTime**，然後按 enter。

5. 在 [ **ClientCacheTime**] 上按一下滑鼠右鍵，按一下 [**新增**]，然後按一下 **[DWORD （32-位）值**]。

6. 輸入 NPS 第一次成功驗證嘗試之後，用戶端電腦快取 NPS TLS 控制碼的時間量（以毫秒為單位）。

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>設定 Nps 上的 TLS 控制碼到期時間

使用此程式來變更 Nps 快取用戶端電腦 TLS 控制碼的時間量。 成功驗證存取用戶端之後，請 Nps 用戶端電腦的 tls 連線屬性作為 TLS 控制碼。 TLS 控制碼的預設持續時間為10小時 \(36000000 毫秒 @ no__t-1。 您可以使用下列程式來增加或減少 TLS 控制碼到期時間。

若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。

>[!IMPORTANT]
>此程式必須在 NPS 上執行，而不是在用戶端電腦上。

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>設定 Nps 上的 TLS 控制碼到期時間

1. 在 NPS 上，開啟 [登錄編輯程式]。

2. 流覽至登錄機碼**HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在 [**編輯**] 功能表上，按一下 [**新增**]，然後按一下 [機**碼**]。

4. 輸入**ServerCacheTime**，然後按 enter。

5. 在 [ **ServerCacheTime**] 上按一下滑鼠右鍵，按一下 [**新增**]，然後按一下 **[DWORD （32-位）值**]。

6. 輸入用戶端成功嘗試驗證後，Nps 快取用戶端電腦 TLS 控制碼的時間量（以毫秒為單位）。

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>取得受根信任 CA 憑證的 SHA-1 雜湊

使用此程式，從本機電腦上安裝的憑證中，取得受信任根憑證授權單位（CA）的安全雜湊演算法（SHA-1）雜湊。 在某些情況下，例如部署群組原則時，必須使用憑證的 SHA-1 雜湊來指定憑證。

使用群組原則時，您可以指定一個或多個受信任的根 CA 憑證，用戶端必須使用這些憑證，才能在使用 EAP 或 PEAP 進行相互驗證的過程中驗證 NPS。 若要指定用戶端必須用來驗證伺服器憑證的受根信任 CA 憑證，您可以輸入憑證的 SHA-1 雜湊。

此程式示範如何使用 [憑證] Microsoft Management Console （MMC）嵌入式管理單元，取得受根信任 CA 憑證的 SHA-1 雜湊。 

若要完成此程式，您必須是本機電腦上的**Users**群組成員。

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>取得受根信任 CA 憑證的 SHA-1 雜湊

1. 在 [執行] 對話方塊或 Windows PowerShell 中，輸入**mmc**，然後按 enter。 此時會開啟 Microsoft Management Console \(MMC @ no__t-1。 在 MMC 中，按一下 [檔案]，**然後按一下 [** **新增/移除 Snap\in**]。 [**新增或移除嵌入式管理單元**] 對話方塊隨即開啟。

2. 在 [**新增或移除嵌入式管理單元**] 的 [可用的嵌入式**管理**單元] 中，按兩下 [**憑證**]。 [憑證] 嵌入式管理單元隨即開啟。 按一下 [**電腦帳戶**]，然後按 **[下一步]** 。

3. 在 [**選取電腦**] 中，確定已選取 [**本機電腦（執行這個主控台的電腦）** ]，然後按一下 **[完成]** ，再按一下 **[確定]** 。

4. 在左窗格中，按兩下 [**憑證（本機電腦）** ]，然後按兩下 [**受信任的根憑證授權**單位] 資料夾。

5. [**憑證**] 資料夾是 [信任的**根憑證授權**單位] 資料夾的子資料夾。 按一下 [**憑證**] 資料夾。

6. 在詳細資料窗格中，流覽至信任的根 CA 的憑證。 按兩下憑證。 [**憑證**] 對話方塊隨即開啟。

7. 在 [**憑證**] 對話方塊中，按一下 [**詳細資料**] 索引標籤。

8. 在欄位清單中，流覽至並選取 [**指紋**]。

9. 在下方窗格中，會顯示您憑證的 SHA-1 雜湊的十六進位字串。 選取 [SHA-1 雜湊]，然後按下 [複製] 命令的 Windows 鍵盤快捷方式 \(CTRL @ no__t-1C @ no__t-2，將雜湊複製到 Windows 剪貼簿。

10. 開啟您想要貼上 SHA-1 雜湊的位置，正確地找出游標，然後按下 貼上 命令的 Windows 鍵盤快捷方式 \(CTRL @ no__t-1V @ no__t-2。 

如需憑證和 NPS 的詳細資訊，請參閱[設定 PEAP 和 EAP 需求的憑證範本](nps-manage-cert-requirements.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
