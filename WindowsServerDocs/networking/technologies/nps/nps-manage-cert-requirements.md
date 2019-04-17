---
title: 設定憑證範本 PEAP 和 EAP 需求
description: 本主題提供使用的憑證的網路原則 Server 與 Windows Server 2016 中的遠端存取的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e597a65982aeead907c9a41f17f1785a0bf81016
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>設定憑證範本 PEAP 和 EAP 需求

>適用於：Windows Server（以每年次管道）、Windows Server 2016

所有可用的網路存取驗證延伸驗證 Protocol\-Tls \(EAP\-TLS\)、保護延伸驗證 Protocol\ 傳輸層安全性 \(PEAP\-TLS\)、與 PEAP\ Microsoft 挑戰交換驗證通訊協定第 2 的憑證 \ (MS\-CHAP v2\) 必須符合的連接，請使用安全通訊端層日傳輸層級安全性 (SSL 日 TLS) 與 x.509 的需求。 Client 和伺服器的憑證有其他的需求。

>[!IMPORTANT]
>本主題提供適用於設定憑證範本的指示。 若要使用這些指示執行，是需要您的部署 \(AD CS\) Active Directory 憑證服務使用您自己公用基礎結構 \(PKI\)。

## <a name="minimum-server-certificate-requirements"></a>最小伺服器的憑證需求

使用 PEAP\-MS\-CHAP v2、PEAP\ TLS 或 EAP\ TLS 的驗證方法、NPS 伺服器必須使用伺服器的憑證，以符合最低伺服器的憑證。 

您可以設定 client 電腦驗證伺服器的憑證來使用**確認伺服器的憑證**上或在群組原則中的選項。 

Client 電腦接受伺服器的驗證嘗試時伺服器的憑證符合下列需求：

- 主體名稱包含的值。 如果您要執行的網路原則 Server (NPS) 空白主體名稱的憑證，不提供 NPS 伺服器的驗證憑證。 若要設定憑證範本主體名稱：

    1. 打開憑證範本。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您想要變更，然後按一下 [憑證範本**屬性**。
    3. 按一下**主體名稱**索引標籤，然後按一下 [**組建這個 Active Directory 資訊的**。
    4. 在**主體名稱的格式**，選取值以外**無**。

- 電腦上受信任的根憑證授權單位和並任何的失敗由 CryptoAPI 及檢查遠端存取原則的網路原則中指定的伺服器鏈結憑證。

- 電腦 NPS 伺服器或 VPN 伺服器的憑證會設定伺服器的驗證用途延伸鍵使用量（）。 （伺服器驗證的物件識別碼是 1.3.6.1.5.5.7.3.1）。

- 設定伺服器的憑證其中演算法所需的值為**RSA**。 若要設定所需的密碼編譯設定：

    1. 打開憑證範本。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您想要變更，然後按一下 [憑證範本**屬性**。
    3. 按一下**密碼編譯**索引標籤。在**的演算法名稱**，按一下 [ **RSA**。 確認**最小金鑰大小**設為**2048 年**。

- 主體替代名稱 (SubjectAltName) 擴充功能，如果您使用，必須包含 DNS 伺服器的名稱。 若要設定憑證範本註冊伺服器名稱網域名稱系統」(DNS): 

    1. 打開憑證範本。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您想要變更，然後按一下 [憑證範本**屬性**。
    3. 按一下**主體名稱**索引標籤，然後按一下 [**組建這個 Active Directory 資訊的**。
    4. 在**在次要主體名稱包含這項資訊**、**DNS 名稱**。

使用 PEAP 和 EAP-TLS 時，NPS 伺服器電腦憑證存放區，使用下列例外顯示所有已安裝的憑證的清單：

- 不會顯示憑證，並包含伺服器驗證用途。

- 不會顯示憑證，並包含主體名稱。

- 登錄型並不會顯示憑證智慧卡登入。

如需詳細資訊，請查看[適用於 802.1 X 的有線和無線部署部署伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="minimum-client-certificate-requirements"></a>最小 client 認證需求

EAP-TLS 或 PEAP-TLS，伺服器接受 client 驗證嘗試時憑證符合下列需求：

- Client 憑證是由企業 CA 發行，或在 Active Directory Domain Services \(AD DS\) 使用者或電腦過去對應。

- 使用者或電腦 client 鏈結到加拿大信任的根憑證包含 EKU 擴充功能 Client 驗證目的 \（Client 驗證的物件識別碼是 1.3.6.1.5.5.7.3.2\），而失敗的由 CryptoAPI 和，詳列於遠端存取原則的網路原則檢查都 NPS 的網路原則中指定憑證物件識別碼檢查。

- 不使用 802.1 X client 登錄為基礎的憑證可能是智慧卡登入或受密碼保護的憑證。

- 主旨另一種方式 \(SubjectAltName\) 副檔名憑證中的使用者的憑證，包含使用者主體名稱 \(UPN\)。 若要設定 UPN 憑證範本中：

    1. 打開憑證範本。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您想要變更，然後按一下 [憑證範本**屬性**。
    3. 按一下**主體名稱**索引標籤，然後按一下 [**組建這個 Active Directory 資訊的**。
    4. 在**在次要主體名稱包含這項資訊**，請選取**使用者主體名稱 \(UPN\)**。

- 適用於電腦的憑證，主題替代 \(SubjectAltName\) 副檔名憑證必須包含完整的網域名稱 \(FQDN\) 的 client，也稱為*的 DNS 名稱*。 在 [憑證範本設定此名稱：

    1. 打開憑證範本。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您想要變更，然後按一下 [憑證範本**屬性**。
    3. 按一下**主體名稱**索引標籤，然後按一下 [**組建這個 Active Directory 資訊的**。
    4. 在**在次要主體名稱包含這項資訊**、**DNS 名稱**。

與 PEAP\ TLS EAP\ TLS，戶端會顯示憑證嵌入式管理單元，使用下列例外所有已安裝的憑證清單：

- Wireless 戶端不會顯示登錄架構和憑證智慧卡登入。 

- Wireless 戶端和 VPN 戶端不會顯示受密碼保護的憑證。 

- 不會顯示憑證，並包含 Client 驗證用途。


如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
