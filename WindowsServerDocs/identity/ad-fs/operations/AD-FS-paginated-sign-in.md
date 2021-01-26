---
title: AD FS 編頁登入
description: 本檔說明 AD FS 2019 的新登入體驗。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.openlocfilehash: f7918c6b6a0ee4f5efb9da45d722f5c49dcc9f42
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781866"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS 編頁登入


針對 Windows Server 2019 中的 AD FS，我們已重新設計登入 UI。  現在，AD FS 登入將擁有 Azure AD 的相同外觀和風格。  這會為使用者提供更一致的登入體驗，其中包含中央和編頁的使用者流程。

## <a name="whats-changing"></a>變更內容
在 Windows Server 2012 R2 和2016的 AD FS 中，您的登入畫面看起來像這樣：

![舊 [登入] 畫面的螢幕擷取畫面。](media/AD-FS-paginated-sign-in/signin1.png)

我們即將離開顯示位於畫面右邊的單一表單。

在 Windows Server 2019 AD FS 中，這些是您將會看到的主要設計變更：


- **中央的 UI**。 先前，登入 UI 會存在於畫面的右側，如上所示。 我們已移動 UI 前端和中心，以將體驗現代化。
- **分頁**。 我們已合併新的流程，而不是提供您完整的表單供您填寫，而是會逐步引導您完成登入體驗。 我們的遙測顯示，有了這種方法，我們的客戶就能成功登入。它也可讓我們更有彈性地併入各種驗證方法，例如美國電話要素驗證。

![新 [登入] 畫面的螢幕擷取畫面。](media/AD-FS-paginated-sign-in/signin2.png)

在第一頁中，系統會要求您輸入您的使用者名稱。 您也可以選取 [讓我保持登入] 選項，以減少登入提示的頻率，並在安全地登入時保持登入狀態。  (預設會停用此選項。 ) 

![新 [登入] 畫面的第一頁螢幕擷取畫面。](media/AD-FS-paginated-sign-in/signin3.png)

在第二個頁面上，您將會看到您的系統管理員所設定的驗證選項。 如果允許以主要的形式啟用外部驗證，則也會包含此功能。

![新 [登入] 畫面第二頁的螢幕擷取畫面。](media/AD-FS-paginated-sign-in/signin4.png)

在第三個頁面上，系統會要求您輸入密碼 (假設您已選取 [密碼] 作為驗證選項) 。

## <a name="how-to-get-the-new-experience"></a>如何取得新的體驗

### <a name="new-installation-of-ad-fs"></a>AD FS 的全新安裝
如果您是 AD FS 的新客戶，預設會收到新的設計。

### <a name="upgrading-a-farm"></a>升級伺服器陣列
如果您是現有的客戶 AD FS 2012 R2 或2016，在將伺服器升級至 AD FS 2019 並讓 FBL\ 至2019之後，有兩種方式可以接收新的設計。

- 允許透過 Powershell 進行新的登入。 執行下列命令以啟用分頁： ``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - 透過 Powershell 或透過 AD FS 伺服器管理員，將外部驗證啟用為主要。 啟用這項功能時，將會啟用新的編頁登入頁面。
如果您是 AD FS 的新客戶，預設會收到新的設計。 但是，如果您是 AD FS 2012 R2 或2016的現有客戶，您必須採取幾個步驟來接收新的設計： ``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>自訂
自訂的選項仍然適用于 AD FS 2019。
以下是您參考的一些其他檔連結。

•適用于不打算將伺服器升級至 AD FS 2019 但仍想要新設計的人員： [在 Active Directory 同盟服務中使用 AZURE AD UX Web 主題](azure-ux-web-theme-in-ad-fs.md)

•自訂的集中位置： [AD FS 使用者登入自訂](ad-fs-user-sign-in-customization.md)
