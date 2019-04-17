---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: "設定信任 Account 聯盟伺服器 Client 電腦"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>設定信任 Account 聯盟伺服器 Client 電腦

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這樣 client 電腦成功可以存取使用 Active Directory 同盟服務 \(AD FS\) 聯盟應用程式，您必須先設定的 Internet Explorer 設定在每個 client 的電腦上，讓瀏覽器信任 account 聯盟伺服器。 您可以手動或透過群組原則、 根據您的系統管理喜好設定，請按照下列程序。  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Internet Explorer 設定以手動方式  
您可以手動設定每個使用者的 Internet Explorer 設定，以支援聯盟透過 AD FS 使用下列程序。 如果有多個使用者使用一部電腦，請完成此程序多次-每個使用者設定檔的一次。  
  
若要執行此程序，將會存取聯盟應用程式的使用者身分登入。 這是 profile\ 特定的設定。 因此，則需要您手動特定電腦上加入每個設定檔，有此設定。  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>若要手動設定信任 account 聯盟伺服器 client 電腦  
  
1.  Client 在電腦上，[開始] Internet Explorer。  
  
2.  在**工具**功能表上，按**網際網路選項]**。  
  
3.  在**安全性**索引標籤上，按**近端**圖示，然後再按一下**網站**。  
  
4.  按一下**進階**，在**將這個網站新增到區域**，輸入 account 聯盟 server 的完整網域名稱系統 \(DNS\) 名稱 \ (例如，https:\/\/fs1.fabrikam.com\)，然後按一下 [**新增**。  
  
5.  按一下**[確定]**三次。  
  
## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>使用群組原則設定 Internet Explorer 設定  
針對大部分部署，我們建議使用群組原則推播適當的 Internet Explorer 設定，每個 client 的電腦。  
  
在成員資格**網域系統管理員**或**企業系統管理員 」**，或相當於，在 Active Directory Domain Services \(AD DS\) 的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>設定要使用群組原則信任 account 聯盟伺服器 client 電腦  
  
1.  Account 合作夥伴公司的樹系的網域控制站，在 [開始]**群組原則管理**snap\ 中。  
  
2.  尋找適當的群組原則物件 \(GPO\)，right\ 按一下它，然後再按一下**編輯**。  
  
3.  在主控台開放**使用者 Configuration\\Preferences\\Windows Settings\\Internet Explorer 維護**，然後按一下 [**安全性**。  
  
4.  在詳細資料窗格中，按一下 double\**安全性區域和內容分級**。  
  
5.  在**區域的安全性和隱私權**，按一下 [**匯入隱私權設定與目前的安全性區域**，然後按一下 [**修改設定**。  
  
6.  按一下**近端**，然後按**網站**。  
  
7.  中**這個網站新增到區域**，輸入 account 聯盟 server 的完整 DNS 名稱 \ (例如，https:\/\/fs1.fabrikam.com\)，按一下 [**新增**，然後按一下 [**關閉**。  
  
8.  按一下**[確定]**這些變更適用於群組原則來兩次。  
  
