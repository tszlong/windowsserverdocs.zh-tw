---
title: 伺服器的憑證部署概觀
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4650c99325def63fd0df25989c661230fada3dac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-overview"></a>伺服器的憑證部署概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題包含下列各節。  
  
-   [伺服器的憑證部署元件](#bkmk_components)
  
-   [伺服器的憑證部署程序概觀](#bkmk_process)
  
## <a name="bkmk_components"></a>伺服器的憑證部署元件
安裝企業版根憑證授權單位 Active Directory 憑證 Services (AD CS) 以及註冊伺服器的憑證來執行網路原則 Server (NPS)、 路由並遠端存取的服務 (RRAS)，或 NPS 和 RRAS 的伺服器，您可以使用此指南。


如果部署 SDN 憑證式驗證時，它們的身份其他伺服器，讓他們獲得安全通訊使用伺服器的憑證需要伺服器。
  
下圖顯示伺服器的憑證部署至伺服器 SDN 基礎結構中所需的元件。
  
![伺服器的憑證部署所需的基礎結構](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> 圖、 在多部伺服器描繪： DC1、 CA1、 WEB1 和許多 SDN 伺服器。 本指南指示部署及 CA1 和 WEB1，設定和設定 DC1，本指南假設您已安裝在您的網路。 如果您未安裝您 Active Directory domain，您可以透過使用[核心網路指南](https://technet.microsoft.com/library/mt604042.aspx)Windows Server 2016。  
  
如需在每個項目上述圖所示的方法如下：  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 執行 AD CS 伺服器角色  
在本案例中，也企業根憑證授權單位是發行 CA。 CA 伺服器電腦具有正確的安全性權限新使用者註冊憑證問題的憑證。 Active Directory 憑證 Services (AD CS) 已安裝於 CA1。  
  
大網路或安全性考量，提供理由，您可以分開根 CA 及發行加拿大的角色，並部署，發行 Ca 附屬 Ca。  
  
在 [最安全的部署，企業根 CA 是拍攝離線和實體安全。   
  
#### <a name="capolicyinf"></a>CAPolicy.inf  
您安裝 AD CS 之前，您設定 CAPolicy.inf 檔案的特定設定為您的部署。  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>複製的**RAS 及 IAS 伺服器]**憑證範本  
當您部署伺服器的憑證時，您可以將一份**RAS 及 IAS 伺服器]**憑證範本]，然後本指南設定範本依據您的需求和指示操作。   
  
您可以使用一份範本，而不是原始範本，以便未來保留原始範本設定。 您設定的備份**RAS 及 IAS 伺服器]**範本，CA 可以建立伺服器的憑證問題 Active Directory 使用者與您所指定的電腦中的群組。  
  
#### <a name="additional-ca1-configuration"></a>其他 CA1 設定  
CA 發行憑證撤銷清單 (CRL) 的電腦必須檢查以確保憑證，以證明身分為他們所提出的有效的憑證，並未撤銷。 使電腦知道 CRL 尋找驗證程序期間，您必須使用 CRL 的正確的位置設定 CA。  
  
### <a name="bkmk_web1"></a>WEB1 執行 Web 服務 (IIS) 伺服器角色  
在的電腦執行的網頁伺服器 (IIS) 伺服器角色 WEB1，您必須為 CRL 和 AIA 位置使用 Windows 檔案總管] 中建立資料夾。  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>CRL 和 AIA virtual directory  
Windows 檔案總管] 中建立資料夾之後，您必須設定為 virtual directory 管理員網際網路服務 (IIS)，以及設定的 virtual directory 存取控制清單好讓電腦存取 AIA 和 CRL 那里發行之後的資料夾。  
  
### <a name="bkmk_dc1"></a>DC1 執行 AD DS 和 DNS 伺服器角色  
DC1 為網域控制站和網路上的 DNS 伺服器。  
  
#### <a name="group-policy-default-domain-policy"></a>群組原則預設網域原則  
設定 ca 憑證範本之後，您可以設定預設網域原則群組原則中，憑證會自動註冊 NPS 及遠端存取伺服器。 群組原則被設定在 AD DS DC1 伺服器上。  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS 別名 (CNAME) 資源記錄  
您必須建立別名 (CNAME) 資源記錄 Web 伺服器，以確保伺服器，以及 AIA 和 CRL 儲存在伺服器上的其他電腦，可以找到。 此外，使用別名 CNAME 資源記錄彈性，讓網頁伺服器可用於其他用途，例如裝載網頁和 FTP 網站。  
  
### <a name="bkmk_nps1"></a>NPS1 執行伺服器角色網路原則與服務存取權的網路原則伺服器角色服務  
當您在 Windows Server 2016 核心網路節目表中執行工作，所以您在這個節目表中執行工作之前，您應該已經有一或更多 NPS 伺服器安裝網路上已安裝 NPS 伺服器。  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>套用群組原則和退出伺服器的憑證  
您設定的憑證範本和自動註冊之後，您可以在所有的目標伺服器更新群組原則。 此時，伺服器註冊 CA1 來自伺服器的憑證。  
  
### <a name="bkmk_process"></a>伺服器的憑證部署程序概觀  
  
> [!NOTE]  
> 如何執行這些步驟的詳細資料一節中提供[伺服器的憑證部署](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md)。  
  
設定伺服器的憑證註冊的程序就會發生在這些階段：  
  
1.  在 WEB1，安裝網頁伺服器 (IIS) 角色。  
  
2.  DC1，在建立您的網頁伺服器，WEB1 別名 (CNAME) 記錄。  
  
3.  設定您的網頁伺服器裝載 ca CRL 然後發行 CRL 和複製新 virtual directory 企業根憑證。  
  
4.  在電腦上您打算安裝 AD CS，指定電腦靜態 IP 位址、 重新命名電腦、 將電腦加入網域，然後再登入的電腦上的網域系統管理員 」 及企業系統管理員群組成員帳號。  
  
5.  在您的計劃安裝 AD CS 的電腦上，設定 CAPolicy.inf 檔案使用您的部署特定的設定。  
  
6.  安裝 AD CS 伺服器角色，以及執行 CA 的額外的設定。  
  
7.  在網頁伺服器 WEB1 共用複製 CA1 CRL 和 CA 憑證。  
  
8.  在加拿大設定 RAS 及 IAS 伺服器的憑證範本複本。 CA 問題根據憑證範本，因此您必須設定伺服器的憑證範本之前 CA 可以發行憑證的憑證。  
  
9.  群組原則中設定伺服器認證自動授權。 當您設定自動註冊時，自動指定的 Active Directory 群組成員資格所有伺服器重新整理每個伺服器上的群組原則時，就會都收到伺服器的憑證。 如果您稍後再新增更多的伺服器，它們將會自動接收伺服器的憑證，也可以。  
  
10. 重新整理群組原則的伺服器上。 重新整理群組原則時，伺服器便會收到伺服器的憑證，以您在上一個步驟中設定範本為基礎。 將其身份 client 電腦伺服器及其他伺服器會使用此憑證驗證程序期間。  
  
    > [!NOTE]  
    > 所有成員網域的電腦自動都接收企業根 CA 憑證未自動註冊的設定。 這是憑證不同設定並使用自動註冊散發伺服器憑證。 將信任的憑證，來這個憑證授權單位發行 CA 憑證會自動安裝的所有成員網域的電腦的受信任的根憑證授權單位憑證存放區。   
  
10. 請確認 [所有伺服器，已都退出有效的伺服器的憑證。  
  


