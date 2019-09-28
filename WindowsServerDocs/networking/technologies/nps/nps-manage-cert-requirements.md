---
title: 設定 PEAP 與 EAP 需求的憑證範本
description: 本主題提供在 Windows Server 2016 中使用憑證搭配網路原則伺服器和遠端存取的相關資訊。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f60e5a1da1388a6dd2432a3603f83d6ca2990a29
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405403"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>設定 PEAP 與 EAP 需求的憑證範本

>適用於：Windows Server (半年度管道)、Windows Server 2016

所有用於網路存取驗證的憑證（使用可延伸的驗證通訊協定 @ no__t-0Transport 層安全性） \(EAP @ no__t-2TLS @ no__t-3，受保護的可延伸驗證通訊協定 @ no__t-4Transport 層安全性\(PEAP @ no__t-6TLS @ no__t-7 和 PEAP @ no__t-8Microsoft 挑戰交握驗證通訊協定第2版 \(MS @ no__t-10CHAP v2 @ no__t-11 必須符合 x.509 憑證的需求，並且適用于使用安全通訊端的連線層/傳輸層級安全性（SSL/TLS）。 用戶端和伺服器憑證都有額外的需求。

>[!IMPORTANT]
>本主題提供設定憑證範本的指示。 若要使用這些指示，您必須使用 Active Directory 憑證服務 \(AD CS @ no__t-3，部署您自己的公開金鑰基礎結構 \(PKI @ no__t-1。

## <a name="minimum-server-certificate-requirements"></a>最低伺服器憑證需求

使用 PEAP @ no__t-0 毫秒 @ no__t-1CHAP v2、PEAP @ no__t-2TLS 或 EAP @ no__t-3TLS 作為驗證方法時，NPS 必須使用符合最低伺服器憑證需求的伺服器憑證。 

用戶端電腦可以設定為使用用戶端電腦上的 [**驗證伺服器憑證**] 選項或群組原則來驗證伺服器憑證。 

當伺服器憑證符合下列需求時，用戶端電腦會接受伺服器的驗證嘗試：

- [主體名稱] 包含一個值。 如果您將憑證發行至執行網路原則伺服器（NPS）的伺服器，但其主體名稱空白，則憑證無法用來驗證您的 NPS。 若要設定具有主體名稱的憑證範本：

    1. 開啟 [憑證範本]。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您要變更的憑證範本，然後按一下 [**屬性**]。
    3. 按一下 [**主體名稱**] 索引標籤，然後**從這個 Active Directory 資訊中**按一下 [建立]。
    4. 在 [**主體名稱格式**] 中，選取 [**無**] 以外的值。

- 伺服器上的電腦憑證會連結至受信任的根憑證授權單位（CA），而且不會讓 CryptoAPI 執行的任何檢查失敗，以及遠端存取原則或網路原則中指定的。

- NPS 或 VPN 伺服器的電腦憑證是以「擴充金鑰使用方法」（EKU）延伸模組中的「伺服器驗證」目的來設定。 （伺服器驗證的物件識別碼是1.3.6.1.5.5.7.3.1）。

- 使用必要的密碼編譯設定來設定伺服器憑證：

    1. 開啟 [憑證範本]。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您要變更的憑證範本，然後按一下 [**屬性**]。
    3. 按一下 [**密碼**編譯] 索引標籤，並務必設定下列各項：
       - **提供者分類：** 金鑰儲存提供者
       - **演算法名稱：** RSA
       - **都會**Microsoft 平臺密碼編譯提供者
       - **最小金鑰大小：** 2048
       - **雜湊演算法：** SHA2
    4. 按一下 [下一步]。

- 主體別名（SubjectAltName）延伸模組（如果使用的話）必須包含伺服器的 DNS 名稱。 若要使用註冊伺服器的網域名稱系統（DNS）名稱來設定憑證範本： 

    1. 開啟 [憑證範本]。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您要變更的憑證範本，然後按一下 [**屬性**]。
    3. 按一下 [**主體名稱**] 索引標籤，然後**從這個 Active Directory 資訊中**按一下 [建立]。
    4. 在 [將**這項資訊包含在替代主體名稱**] 中，選取 [ **DNS 名稱**]。

使用 PEAP 和 EAP-TLS 時，Nps 會顯示 [電腦] 憑證存放區中所有已安裝憑證的清單，但有下列例外狀況：

- 不會顯示不包含 EKU 延伸中伺服器驗證目的的憑證。

- 不會顯示不包含主體名稱的憑證。

- 不會顯示以登錄為基礎的智慧卡登入憑證。

如需詳細資訊，請參閱[部署 802.1 x 有線和無線部署的伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="minimum-client-certificate-requirements"></a>最低用戶端憑證需求

如果使用 EAP-TLS 或 PEAP-TLS，當憑證符合下列需求時，伺服器會接受用戶端驗證嘗試：

- 用戶端憑證是由企業 CA 發行，或對應至 Active Directory Domain Services 中的使用者或電腦帳戶 \(AD DS @ no__t-1。

- 用戶端上的使用者或電腦憑證會連結至信任的根 CA，包括 EKU 延伸中的用戶端驗證目的 @no__t-用戶端驗證的0the 物件識別碼是 1.3.6.1.5.5.7.3.2 @ no__t-1，而且不會使由 CryptoAPI 執行，而且是在遠端存取原則或網路原則中指定，也是在 NPS 網路原則中指定的憑證物件識別碼檢查。

- 802.1 X 用戶端不會使用以登錄為基礎的憑證，也就是智慧卡登入或受密碼保護的憑證。

- 針對使用者憑證，憑證中的主體別名 \(SubjectAltName @ no__t-1 extension 包含使用者主體名稱 \(UPN @ no__t-3。 若要在憑證範本中設定 UPN：

    1. 開啟 [憑證範本]。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您要變更的憑證範本，然後按一下 [**屬性**]。
    3. 按一下 [**主體名稱**] 索引標籤，然後**從這個 Active Directory 資訊中**按一下 [建立]。
    4. 在 [將**這項資訊包含在替代主體名稱**] 中，選取 [**使用者主體名稱] \(UPN @ no__t-3**。

- 針對電腦憑證，憑證中的主體別名 \(SubjectAltName @ no__t-1 延伸模組必須包含用戶端的完整功能變數名稱 \(FQDN @ no__t-3，這也稱為*DNS 名稱*。 若要在憑證範本中設定此名稱：

    1. 開啟 [憑證範本]。
    2. 在詳細資料窗格中，以滑鼠右鍵按一下您要變更的憑證範本，然後按一下 [**屬性**]。
    3. 按一下 [**主體名稱**] 索引標籤，然後**從這個 Active Directory 資訊中**按一下 [建立]。
    4. 在 [將**這項資訊包含在替代主體名稱**] 中，選取 [ **DNS 名稱**]。

使用 PEAP @ no__t-0TLS 和 EAP @ no__t 1TLS 時，用戶端會在 [憑證] 嵌入式管理單元中顯示所有已安裝的憑證清單，但有下列例外狀況：

- 無線用戶端不會顯示以登錄為基礎的智慧卡登入憑證。 

- 無線用戶端和 VPN 用戶端不會顯示受密碼保護的憑證。 

- 不會顯示不包含 EKU 延伸中用戶端驗證用途的憑證。


如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
