---
title: 設定 PEAP 與 EAP 需求的憑證範本
description: 本主題提供使用網路原則伺服器和 Windows Server 2016 中的遠端存取使用憑證的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41f6f88705fa3d58be695fd825d960e7df21cd24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885879"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>設定 PEAP 與 EAP 需求的憑證範本

>適用於：Windows Server （半年通道），Windows Server 2016

所有的憑證用於使用可延伸驗證通訊協定的網路存取驗證\-傳輸層安全性\(EAP\-TLS\)，受保護的可延伸驗證通訊協定\-傳輸層安全性\(PEAP\-TLS\)，與 PEAP\-Microsoft Challenge Handshake 驗證通訊協定第 2 版\(MS\-MS-CHAP v2\)必須符合 X.509 憑證的需求，並適用於使用安全通訊端層/傳輸層級安全性 (SSL/TLS) 的連線。 用戶端和伺服器憑證有其他需求。

>[!IMPORTANT]
>本主題提供設定憑證範本的指示。 若要使用這些指示，則需要您已部署您自己公開金鑰基礎結構\(PKI\)與 Active Directory 憑證服務\(AD CS\)。

## <a name="minimum-server-certificate-requirements"></a>最小的伺服器憑證需求

使用 PEAP\-MS\-MS-CHAP v2，PEAP\-TLS 或 EAP\-作為驗證方法的 TLS，NPS 必須使用符合最小的伺服器憑證需求的伺服器憑證。 

用戶端電腦可以設定為使用驗證伺服器憑證**確認伺服器憑證**用戶端電腦或群組原則中的選項。 

當伺服器憑證符合下列需求時，用戶端電腦會接受伺服器的驗證嘗試：

- 主體名稱中包含的值。 如果您將憑證發行到您的伺服器執行網路原則伺服器 (NPS) 具有空白的主體名稱時，憑證無法使用。 若要驗證您的 NPS 若要設定主體名稱的憑證範本：

    1. 開啟 [憑證範本]。
    2. 在 詳細資料 窗格中，以滑鼠右鍵按一下您想要變更此項目，然後按一下 憑證範本**屬性**。
    3. 按一下 **主體名稱**索引標籤，然後再按一下**Active Directory 資訊來建立**。
    4. 在 **主體名稱格式**，選取的值不**無**。

- 電腦憑證，伺服器鏈結到信任的根憑證授權單位 (CA) 而且不會失敗的遠端存取原則或網路原則中指定任何 CryptoAPI，所執行的檢查。

- 在 NPS 或 VPN 伺服器的電腦憑證已使用伺服器驗證目的，在 擴充金鑰使用方法 (EKU) 延伸模組。 （伺服器驗證的物件識別元為 1.3.6.1.5.5.7.3.1）。

- 使用必要的密碼編譯設定來設定伺服器憑證：

    1. 開啟 [憑證範本]。
    2. 在 詳細資料 窗格中，以滑鼠右鍵按一下您想要變更此項目，然後按一下 憑證範本**屬性**。
    3. 按一下  **Cryptography**索引標籤，並請務必設定下列各項：
       - **提供者類別目錄：** 金鑰儲存提供者
       - **演算法名稱：** RSA
       - **提供者：** Microsoft 平台密碼編譯提供者
       - **最小金鑰大小：** 2048
       - **雜湊演算法：** SHA2
    4. 按一下 [下一步] 。

- 使用主體別名 (SubjectAltName) 延伸，如果使用，必須包含伺服器的 DNS 名稱。 若要設定註冊伺服器的網域名稱系統 (DNS) 名稱的憑證範本： 

    1. 開啟 [憑證範本]。
    2. 在 詳細資料 窗格中，以滑鼠右鍵按一下您想要變更此項目，然後按一下 憑證範本**屬性**。
    3. 按一下 **主體名稱**索引標籤，然後再按一下**Active Directory 資訊來建立**。
    4. 在 **在次要主體名稱中包含這項資訊**，選取**DNS 名稱**。

當使用 PEAP 與 EAP-TLS，NPSs 就會顯示一份所有已安裝的憑證在電腦憑證存放區，但有下列例外狀況：

- 不會顯示不包含伺服器驗證目的，EKU 延伸中的憑證。

- 不會顯示不包含主體名稱的憑證。

- 登錄為基礎並不會顯示智慧卡登入憑證。

如需詳細資訊，請參閱 <<c0> [ 部署 802.1x 有線和無線部署的伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="minimum-client-certificate-requirements"></a>最低用戶端憑證需求

透過 EAP-TLS 或 PEAP-TLS，伺服器會接受用戶端驗證嘗試，當憑證符合下列需求：

- 企業 CA 所發出的用戶端憑證是否對應至使用者或電腦帳戶在 Active Directory 網域服務中\(AD DS\)。

- 在用戶端鏈結至信任的根 CA，使用者或電腦憑證 EKU 延伸中包含用戶端驗證目的\(用戶端驗證的物件識別碼是 1.3.6.1.5.5.7.3.2\)，且不會失敗檢查由 CryptoAPI 和之間指定的遠端存取原則或網路原則，也不在 NPS 網路原則中指定的憑證物件識別項檢查。

- 802.1x 用戶端不會使用以登錄為基礎的憑證可能是智慧卡登入或受密碼保護的憑證。

- 對於使用者憑證，主體別名\(SubjectAltName\)憑證中的延伸模組包含的使用者主體名稱\(UPN\)。 若要在憑證範本中設定 UPN:

    1. 開啟 [憑證範本]。
    2. 在 詳細資料 窗格中，以滑鼠右鍵按一下您想要變更此項目，然後按一下 憑證範本**屬性**。
    3. 按一下 **主體名稱**索引標籤，然後再按一下**Active Directory 資訊來建立**。
    4. 在 **在次要主體名稱中包含這項資訊**，選取**使用者主體名稱\(UPN\)**。

- 對於電腦憑證，主體別名\(SubjectAltName\)憑證中的延伸模組必須包含完整的網域名稱\(FQDN\)的用戶端，也稱為*DNS 名稱*。 若要設定此名稱在 憑證範本：

    1. 開啟 [憑證範本]。
    2. 在 詳細資料 窗格中，以滑鼠右鍵按一下您想要變更此項目，然後按一下 憑證範本**屬性**。
    3. 按一下 **主體名稱**索引標籤，然後再按一下**Active Directory 資訊來建立**。
    4. 在 **在次要主體名稱中包含這項資訊**，選取**DNS 名稱**。

使用 PEAP\-TLS 和 EAP\-TLS 用戶端就會顯示所有已安裝憑證的清單顯示在 [憑證] 嵌入式管理單元中，但有下列例外狀況：

- 無線用戶端不會顯示登錄為基礎和智慧卡登入憑證。 

- 無線用戶端和 VPN 用戶端不會顯示受密碼保護的憑證。 

- 不會顯示不包含用戶端驗證目的，EKU 延伸中的憑證。


如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
