---
title: 在 CA1 上設定 CDP 及 AIA 延伸模組
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 2bdd03636d1cbbcf5b0355358cbedeb63f97af1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834219"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 上設定 CDP 及 AIA 延伸模組

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來設定憑證撤銷清單 (CRL) 發佈點 (CDP) 和授權資訊存取 (AIA) 設定 CA1 上。  
  
若要執行此程序，您必須是 Domain Admins 的成員。  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>CA1 上設定的 CDP 和 AIA 延伸模組  
  
1.  在 [伺服器管理員] 按一下 [工具]  ，然後按一下 [憑證授權單位] 。  
  
2.  在 憑證授權單位主控台樹狀目錄中，以滑鼠右鍵按一下**corp CA1 CA**，然後按一下**屬性**。  
  
    > [!NOTE]  
    > 如果您未命名 CA1 之電腦，而且您的網域名稱不同於在此範例中，您 CA 的名稱會不同。 CA 名稱的格式*網域*-*CAComputerName*-CA。  
  
3.  按一下 [延伸] 索引標籤。請確認**選取延伸模組**設為**CRL 發佈點 (CDP)**，然後在**指定使用者可以獲得憑證撤銷清單 (CRL) 的位置**，執行下列作業：  
  
    1.  選取的項目`file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下**移除**。 在 **確認移除**，按一下**是**。  
  
    2.  選取的項目`https://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下**移除**。 在 **確認移除**，按一下**是**。  
  
    3.  選取的項目路徑的開頭`ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`，然後按一下**移除**。 在 **確認移除**，按一下**是**。  
  
4.  在 **指定使用者可以獲得憑證撤銷清單 (CRL) 的位置**，按一下**新增**。 **新增位置**對話方塊隨即開啟。  
  
5.  在 **新增位置**，請在**位置**，型別`https://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下  **確定**。 這會讓您回到 [CA 內容] 對話方塊。  
  
6.  在 **延伸模組**索引標籤上，選取下列核取方塊：  
  
    -   **包含在 Crl 中。用戶端用此來尋找 Delta CRL 的位置**  
  
    -   **包含在簽發之憑證的 CDP 延伸中**  
  
7.  在 **指定使用者可以獲得憑證撤銷清單 (CRL) 的位置**，按一下**新增**。 **新增位置**對話方塊隨即開啟。  
  
8.  在 **新增位置**，請在**位置**，型別`file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下  **確定**。 這會讓您回到 [CA 內容] 對話方塊。  
  
9. 在 **延伸模組**索引標籤上，選取下列核取方塊：  
  
    -   **將 Crl 發佈到這個位置**  
  
    -   **將 Delta Crl 發佈到這個位置**  
  
10. 變更**選取延伸模組**要**授權資訊存取 (AIA)**，然後在**指定使用者可以獲得憑證撤銷清單 (CRL) 的位置**，執行下列步驟：  
  
    1.  選取的項目路徑的開頭`ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`，然後按一下**移除**。 在 **確認移除**，按一下**是**。  
  
    2.  選取的項目`https://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`，然後按一下**移除**。 在 **確認移除**，按一下**是**。  
  
    3.  選取的項目`file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`，然後按一下**移除**。 在 **確認移除**，按一下**是**。  
  
11. 在 **指定使用者可從其中取得憑證的 這個 CA 的位置**，按一下**新增**。 **新增位置**對話方塊隨即開啟。  
  
12. 在 **新增位置**，請在**位置**，型別`https://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`，然後按一下  **確定**。 這會讓您回到 [CA 內容] 對話方塊。  
  
13. 在 **延伸模組**索引標籤上，選取**包含在簽發之憑證的 AIA**。  
  
14. 當系統提示您重新啟動 Active Directory 憑證服務，按一下**No**。 您稍後將會重新啟動服務。  
  

