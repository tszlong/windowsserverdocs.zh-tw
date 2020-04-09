---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: 將用戶端電腦設定為信任帳戶同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5022be852ab72f57c90d957cb1111f10e9b03a90
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854941"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>將用戶端電腦設定為信任帳戶同盟伺服器

為了讓用戶端電腦可以使用 Active Directory 同盟服務 \(AD FS\)，成功存取同盟應用程式，您必須先在每部用戶端電腦上設定 Internet Explorer 設定，讓瀏覽器信任帳戶同盟伺服器。 您可以手動執行此動作，或透過群組原則，視您的系統管理喜好設定而定，完成下列其中一項程式。  
  
## <a name="configuring-internet-explorer-settings-manually"></a>手動設定 Internet Explorer 設定  
您可以使用下列程式，手動設定每個使用者的 Internet Explorer 設定，以支援透過 AD FS 的同盟。 如果有多個使用者使用單一電腦，請多次完成此程式，每個使用者設定檔一次。  
  
若要執行此程式，請以將存取同盟應用程式的使用者身分登入。 這是\-特定設定的設定檔。 因此，您必須針對存在於特定電腦上的每個設定檔，手動新增這項設定。  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>手動將用戶端電腦設定為信任帳戶同盟伺服器  
  
1.  在用戶端電腦上，啟動 Internet Explorer。  
  
2.  在 [**工具**] 功能表上，按一下 [**網際網路選項**]。  
  
3.  在 [**安全性**] 索引標籤上，按一下 [近端**內部**網路] 圖示，然後按一下 [**網站**]。  
  
4.  按一下  **Advanced**，然後在**將這個網站新增到區域** 中，輸入帳戶同盟伺服器的完整網域名稱系統 \(DNS\) 名稱 \(例如 HTTPs：\/\/fs1.fabrikam.com\)，然後按一下 **新增**。  
  
5.  按 [確定] 三次。  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>使用群組原則進行 Internet Explorer 設定  
對於大部分的部署，建議您使用群組原則將適當的 Internet Explorer 設定推送到每部用戶端電腦。  
  
若要完成此程式，至少需要**Domain admins**或**Enterprise Admins**的成員資格或同等許可權，Active Directory Domain Services \(AD DS\)。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\(HTTP：\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>若要將用戶端電腦設定為信任帳戶同盟伺服器，請使用群組原則  
  
1.  在帳戶夥伴組織樹系中的網域控制站上，啟動中的 [**群組原則管理**] 嵌入式管理單元\-。  
  
2.  尋找適當的群組原則物件 \(GPO\)，以滑鼠\-按右鍵它，然後按一下 [**編輯**]。  
  
3.  在主控台樹中，開啟 **使用者設定\\喜好設定\\Windows 設定\\Internet Explorer 維護**，然後按一下 **安全性**。  
  
4.  在詳細資料窗格中，按兩下 [**安全性區域和內容分級**]\-。  
  
5.  在 [**安全性區域和隱私權**] 底下，按一下 **[匯入目前的安全性區域和隱私權設定**]，然後按一下 [**修改設定**]。  
  
6.  按一下 [近端**內部**網路]，然後按一下 [**網站**]。  
  
7.  在 **[將這個網站新增到區域**] 中，輸入帳戶同盟伺服器的完整 DNS 名稱 \(例如，HTTPs：\/\/fs1.fabrikam.com\)，按一下 [**新增**]，然後按一下 [**關閉**]。  
  
8.  按兩次 **[確定]** ，將這些變更套用至群組原則。  
  
