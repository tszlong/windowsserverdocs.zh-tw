---
description: 深入瞭解：設定用戶端電腦以信任帳戶同盟伺服器
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: 設定用戶端電腦以信任帳戶同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 9b76fd4bd7ae09e9940dc7501136c5b0bb3449f6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050296"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>設定用戶端電腦以信任帳戶同盟伺服器

為了讓用戶端電腦可以使用 Active Directory 同盟服務 AD FS 來成功存取同盟應用程式 \( \) ，您必須先在每部用戶端電腦上設定 Internet Explorer 設定，讓瀏覽器信任帳戶同盟伺服器。 您可以藉由完成下列其中一項程式，以手動方式或透過群組原則（視您的系統管理喜好設定而定）。

## <a name="configuring-internet-explorer-settings-manually"></a>手動設定 Internet Explorer 設定
您可以使用下列程式，手動設定每個使用者的 Internet Explorer 設定，以支援透過 AD FS 的同盟。 如果有多個使用者使用單一電腦，請多次完成此程式，每個使用者設定檔一次。

若要執行此程式，請以將存取同盟應用程式的使用者身分登入。 這是設定檔 \- 特有的設定。 因此，它需要您針對存在於特定電腦上的每個設定檔，手動新增此設定。

#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>手動設定用戶端電腦以信任帳戶同盟伺服器

1.  在用戶端電腦上，啟動 Internet Explorer。

2.  按一下 [ **工具** ] 功能表上的 [ **網際網路選項**]。

3.  在 [ **安全性** ] 索引標籤上，按一下 [近端 **內部** 網路] 圖示，然後按一下 [ **網站**]。

4.  按一下 [ **Advanced**]，然後在 **[將這個網站新增到區域**] 中，輸入帳戶同盟伺服器的完整網域名稱系統 \( DNS \) 名稱 \( ，例如 Https： \/ \/ fs1.fabrikam.com \) ，然後按一下 [**新增**]。

5.  按 [確定] 三次。

## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>使用群組原則設定 Internet Explorer 設定
在大部分的部署中，我們建議您使用群組原則將適當的 Internet Explorer 設定推送到每部用戶端電腦。

  \( \) 若要完成此程式，至少需要 Domain admins 或 Enterprise admins 的成員資格或 Active Directory Domain Services AD DS 中的同等許可權。  請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ 使用適當帳戶和群組成員資格的詳細資料：LinkId \= 83477 \) 。

#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>使用群組原則將用戶端電腦設定為信任帳戶同盟伺服器

1.  在帳戶夥伴組織樹系中的網域控制站上，啟動 **群組原則管理** ] 嵌入式管理單元 \- 。

2.  找出適當的群組原則物件 \( GPO \) ，以滑鼠右鍵 \- 按一下它，然後按一下 [ **編輯**]。

3.  在主控台樹中，開啟 [ **使用者 \\ \\ 設定喜好設定 Windows 設定 \\ Internet Explorer 維護**]，然後按一下 [ **安全性**]。

4.  在詳細資料窗格中，按兩下 [ \- **安全性區域和內容分級**]。

5.  在 [ **安全性區域和隱私權**] 之下，按一下 **[匯入目前的安全性區域和隱私權設定**]，然後按一下 [ **修改設定**]。

6.  按一下 [近端 **內部** 網路]，然後按一下 [ **網站**]。

7.  在 **[將這個網站新增到區域**] 中，輸入帳戶同盟伺服器的完整 DNS 名稱， \( 例如 [HTTPs： \/ \/ Fs1.fabrikam.com] \) ，按一下 [**新增**]，然後按一下 [**關閉**]。

8.  按一下 **[確定]** 兩次，將這些變更套用至群組原則。

