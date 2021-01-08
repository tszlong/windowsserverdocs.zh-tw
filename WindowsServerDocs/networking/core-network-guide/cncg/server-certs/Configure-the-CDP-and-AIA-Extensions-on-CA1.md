---
title: 在 CA1 上設定 CDP 及 AIA 延伸模組
description: 瞭解如何設定憑證撤銷清單 (CRL) 發佈點 (CDP) 以及 CA1 上的授權資訊存取 (AIA) 設定。
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.author: lizross
author: eross-msft
ms.date: 07/26/2018
ms.openlocfilehash: 5e8ac26965bdeae2f46551478561be339bfb0bfc
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038738"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 上設定 CDP 及 AIA 延伸模組

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來設定憑證撤銷清單 (CRL) 發佈點 (CDP) 以及 CA1 上的授權資訊存取 (AIA) 設定。

若要執行此程式，您必須是 Domain Admins 的成員。

#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 上設定 CDP 和 AIA 擴充功能

1.  在 [伺服器管理員] 按一下 [工具]，然後按一下 [憑證授權單位]。

2.  在 [憑證授權單位單位] 主控台樹中，以滑鼠右鍵按一下 [ **corp-CA1-CA**]， **然後按一下 [** 內容]。

    > [!NOTE]
    > 如果您未將電腦命名為 CA1，而您的功能變數名稱與此範例中的功能變數名稱不同，則您的 CA 名稱會有所不同。 Ca 名稱的格式為 *網域* - *CAComputerName*-ca。

3.  按一下 [ **擴充** 功能] 索引標籤。確定 **選取 [延伸** 模組] 設定為 [ **crl 發佈點 (CDP)**]，然後在 [ **指定使用者可從哪些位置取得憑證撤銷清單 (CRL)**]，執行下列動作：

    1.  選取專案 `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` ，然後按一下 [ **移除**]。 在 [ **確認移除**] 中，按一下 **[是]**。

    2.  選取專案 `http://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` ，然後按一下 [ **移除**]。 在 [ **確認移除**] 中，按一下 **[是]**。

    3.  選取以路徑開頭的專案 `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>` ，然後按一下 [ **移除**]。 在 [ **確認移除**] 中，按一下 **[是]**。

4.  在 [ **指定使用者可從中取得憑證撤銷清單的位置] (CRL)** 中，按一下 [ **新增**]。 [ **新增位置** ] 對話方塊隨即開啟。

5.  在 [ **新增位置**] 的 [ **位置**] 中，輸入 `http://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` ，然後按一下 **[確定]**。 這會返回 [CA 屬性] 對話方塊。

6.  在 [ **擴充** 功能] 索引標籤上，選取下列核取方塊：

    -   **包含在 Crl 中。用戶端會使用此項來尋找 Delta CRL 位置**

    -   **包含在已頒發證書的 CDP 延伸中**

7.  在 [ **指定使用者可從中取得憑證撤銷清單的位置] (CRL)** 中，按一下 [ **新增**]。 [ **新增位置** ] 對話方塊隨即開啟。

8.  在 [ **新增位置**] 的 [ **位置**] 中，輸入 `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` ，然後按一下 **[確定]**。 這會返回 [CA 屬性] 對話方塊。

9. 在 [ **擴充** 功能] 索引標籤上，選取下列核取方塊：

    -   **將 CRL 發佈到這個位置**

    -   **將 Delta CRL 發佈到這個位置**

10. 將 [ **選取延伸** ] 變更為 [ **授權資訊存取 (AIA])**，然後在 [ **指定使用者可從哪些位置取得憑證撤銷清單 (CRL)**] 中，執行下列動作：

    1.  選取以路徑開頭的專案 `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services` ，然後按一下 [ **移除**]。 在 [ **確認移除**] 中，按一下 **[是]**。

    2.  選取專案 `http://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt` ，然後按一下 [ **移除**]。 在 [ **確認移除**] 中，按一下 **[是]**。

    3.  選取專案 `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt` ，然後按一下 [ **移除**]。 在 [ **確認移除**] 中，按一下 **[是]**。

11. 在 [ **指定使用者可從中取得此 CA 憑證的位置**] 中，按一下 [ **新增**]。 [ **新增位置** ] 對話方塊隨即開啟。

12. 在 [ **新增位置**] 的 [ **位置**] 中，輸入 `http://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt` ，然後按一下 **[確定]**。 這會返回 [CA 屬性] 對話方塊。

13. 在 [ **擴充** 功能] 索引標籤上，選取 **[包含在已發行的憑證 AIA 中**]。

14. 當系統提示您重新開機 Active Directory 憑證服務時，請按一下 [ **否**]。 您稍後將重新開機服務。


