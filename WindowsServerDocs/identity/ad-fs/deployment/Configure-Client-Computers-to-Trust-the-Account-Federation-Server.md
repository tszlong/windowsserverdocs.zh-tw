---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: 設定用戶端電腦信任帳戶同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e28d050a9aa40c015af16a665e90535cb810b4ff
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192326"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>設定用戶端電腦信任帳戶同盟伺服器

讓用戶端電腦可以成功地存取同盟應用程式使用 Active Directory Federation Services \(AD FS\)，您必須先設定 Internet Explorer 設定每個用戶端電腦上，讓瀏覽器信任帳戶同盟伺服器。 您可以手動或透過群組原則，根據您的系統管理喜好設定，藉由完成下列程序的其中一個。  
  
## <a name="configuring-internet-explorer-settings-manually"></a>設定 Internet Explorer 手動  
若要手動設定每個使用者的 Internet Explorer 設定，以支援透過 AD FS 的同盟，您可以使用下列程序。 如果多個使用者使用單一電腦，完成此程序多次，每個使用者設定檔一次。  
  
若要執行此程序，將用來存取同盟應用程式的使用者身分登入。 這是設定檔\-特定設定。 因此，它需要特定的電腦上手動新增每個設定檔存在於此設定。  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>若要手動設定 信任帳戶同盟伺服器的用戶端電腦  
  
1.  在用戶端電腦上啟動 Internet Explorer。  
  
2.  在 **工具**功能表上，按一下**網際網路選項**。  
  
3.  在 **安全性**索引標籤上，按一下**近端內部網路**圖示，，然後按一下**站台**。  
  
4.  按一下 **進階**，然後在**將這個網站新增到區域**，輸入完整網域名稱系統\(DNS\)帳戶同盟伺服器的名稱\(比方說，https:\/\/fs1.fabrikam.com\)，然後按一下**新增**。  
  
5.  按 [確定]  三次。  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>使用群組原則來設定 Internet Explorer 設定  
大部分的部署，我們建議您若要將適當的 Internet Explorer 設定推送到每部用戶端電腦使用群組原則。  
  
中的成員資格**Domain Admins**或是**Enterprise Admins**，或同等權限，在 Active Directory 網域服務中\(AD DS\) ，至少需要完成此程序。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>若要設定信任的帳戶同盟伺服器使用群組原則的用戶端電腦  
  
1.  帳戶夥伴組織的樹系中的網域控制站，啟動**群組原則管理**貼齊\-中。  
  
2.  找不到適當的群組原則物件\(GPO\)，以滑鼠右鍵\-，按一下，然後按一下**編輯**。  
  
3.  在主控台樹狀目錄中，開啟**使用者設定\\喜好設定\\Windows 設定\\Internet Explorer 維護**，然後按一下**安全性**。  
  
4.  在 詳細資料 窗格中，按兩下\-按一下 **安全性區域與內容分級**。  
  
5.  底下**安全性區域與隱私權**，按一下**匯入目前的安全性區域和隱私權設定**，然後按一下**修改設定**。  
  
6.  按一下 **近端**，然後按一下**站台**。  
  
7.  在**將這個網站新增至區域**，輸入帳戶同盟伺服器的完整 DNS 名稱\(例如，https:\/\/fs1.fabrikam.com\)，按一下 **新增**，然後按一下**關閉**。  
  
8.  按一下 **確定**將這些變更套用到群組原則的兩倍。  
  
