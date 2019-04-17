---
title: 在 CA1 設定 CDP 及 AIA 擴充功能
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80f6d68816a58b2ac30010e0917ce0f816a30234
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 設定 CDP 及 AIA 擴充功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序，設定憑證撤銷清單 (CRL) Distribution 點 (CDP) 和授權的存取 (AIA) 設定在 CA1。  
  
若要執行此程序，您必須網域系統管理員」的成員。  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>若要 CA1 上設定 CDP 和 AIA 擴充功能  
  
1.  在伺服器管理員中，按一下**工具**，然後按一下 [**憑證授權單位**。  
  
2.  憑證授權單位主機樹上，以滑鼠右鍵按一下**corp CA1 CA**，然後按**屬性**。  
  
    > [!NOTE]  
    > 如果您不為電腦 CA1，且您的網域名稱不同在此範例中，您的 CA 的名稱是不同。 CA 名稱的格式是*網域*-*CAComputerName*CA。  
  
3.  按一下**的擴充功能**索引標籤，請確定**選取 [擴充功能**設定為 [ **CRL Distribution 點 (CDP)**，在指定的使用者可以取得憑證撤銷清單 (CRL)，執行下列動作：  
  
    1.  選取的項目`file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下 [**移除**。 在**確認移除**，按一下 [**是**。  
  
    2.  選取的項目`http:\/\/<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下 [**移除**。 在**確認移除**，按一下 [**是**。  
  
    3.  選取的項目開頭路徑`ldap:\/\/CN\=<CATruncatedName><CRLNameSuffix>,CN\=<ServerShortName>`，然後按一下 [**移除**。 在**確認移除**，按一下 [**是**。  
  
4.  在**指定的使用者可以取得憑證撤銷清單消位置**，按一下 [**新增]**。 **加入位置**對話方塊。  
  
5.  在**加入位置**，請在**位置**，輸入`http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下 [ **[確定]**。 這會讓您回到 [CA 屬性對話方塊。  
  
6.  在**的擴充功能**索引標籤上，選取下列核取方塊：  
  
    -   **包含的 Crl。 戶端這個尋找 Delta CRL 位置**  
  
    -   **包含在發行憑證的 CDP 擴充功能**  
  
7.  在**指定的使用者可以取得憑證撤銷清單消位置**，按一下 [**新增]**。 **加入位置**對話方塊。  
  
8.  在**加入位置**，請在**位置**，輸入`file:\/\/\\\\pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然後按一下 [ **[確定]**。 這會讓您回到 [CA 屬性對話方塊。  
  
9. 在**的擴充功能**索引標籤上，選取下列核取方塊：  
  
    -   **到此處發行 Crl**  
  
    -   **到此處發行 Delta Crl**  
  
10. 變更**選取擴充功能**以**授權的存取 (AIA)**，在**指定的位置使用者可以獲得憑證撤銷清單 (CRL)**，執行下列動作：  
  
    1.  選取的項目開頭路徑`ldap:\/\/CN\=<CATruncatedName>,CN\=AIA,CN\=Public Key Services`，然後按一下 [**移除**。 在**確認移除**，按一下 [**是**。  
  
    2.  選取的項目`http:\/\/<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然後按一下 [**移除**。 在**確認移除**，按一下 [**是**。  
  
    3.  選取的項目`file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然後按一下 [**移除**。 在**確認移除**，按一下 [**是**。  
  
11. 在**指定的使用者可以取得憑證撤銷清單消位置**，按一下 [**新增]**。 **加入位置**對話方塊。  
  
12. 在**加入位置**，請在**位置**，輸入`http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然後按一下 [ **[確定]**。 這會讓您回到 [CA 屬性對話方塊。  
  
13. 在**的擴充功能**索引標籤，選取**加入 AIA 發行憑證的**。  
  
14. 在**加入位置**，請在**位置**，輸入`file:\/\/\\\\pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然後按一下 [ **[確定]**。 這會讓您回到 [CA 屬性對話方塊。  
  
    > [!IMPORTANT]  
    > 確認**加入 AIA 擴充功能發行憑證的**未選取。  
  
15. 重新 Active Directory 憑證服務提示，請按一下**否]**。 您稍後將會重新開機服務。  
  


