---
title: AD FS 分頁登入
description: 本檔說明 AD FS 2019 的新登入體驗。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.openlocfilehash: 27b0232b65a3003dde9a5702ec45063781abd813
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947444"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS 分頁登入


針對 Windows Server 2019 中的 AD FS，我們已重新設計登入 UI。  現在，AD FS 登入將擁有與 Azure AD 相同的外觀與風格。  這可為使用者提供更一致的登入體驗，並併入中央化和編頁的使用者流程。

## <a name="whats-changing"></a>變更的內容
在 Windows Server 2012 R2 和2016的 AD FS 中，您的登入畫面看起來像這樣：

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

我們正從畫面右側顯示的單一表單開始移動。

在 Windows Server 2019 的 AD FS 中，這些是您將會看到的主要設計變更：


- **中央 UI**。 先前的登入 UI 會出現在畫面的右側，如上所示。 我們已將 UI 前方和中央移到現代化體驗。
- **分頁**。 我們已納入新的流程，讓您可以逐步完成登入體驗，而不是提供您要填寫的長表單。 我們的遙測顯示使用這種方法時，我們的客戶會有更成功的登入。它也提供更多彈性來整合各種驗證方法，例如美國電話要素驗證。

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

在第一個頁面上，系統會要求您輸入您的使用者名稱。 您也可以選取 [讓我保持登入] 選項，以降低登入提示的頻率，並在安全時保持登入狀態。  (預設會停用此選項。 ) 

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

在第二個頁面上，您會看到系統管理員所設定的驗證選項。 如果啟用 [外部驗證] 作為 [主要]，則也會包含這種情況。

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

在第三個頁面上，系統會要求您輸入密碼， (假設您已選取 [密碼] 做為驗證選項) 。

## <a name="how-to-get-the-new-experience"></a>如何取得新的體驗

### <a name="new-installation-of-ad-fs"></a>AD FS 的新安裝
如果您是 AD FS 的新客戶，則預設會收到新的設計。

### <a name="upgrading-a-farm"></a>升級伺服器陣列
如果您是現有的客戶 AD FS 2012 R2 或2016，在將伺服器升級至 AD FS 2019 並讓 FBL 到2019的情況下，有兩種方式可接收新的設計。

- 允許透過 Powershell 進行新的登入。 執行下列命令以啟用分頁：``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - 透過 Powershell 或 AD FS 伺服器管理員，將外部驗證啟用為主要。 啟用這項功能時，將會啟用新的編頁登入頁面。
如果您是 AD FS 的新客戶，則預設會收到新的設計。 不過，如果您是 AD FS 2012 R2 或2016的現有客戶，您必須採取幾個步驟來接收新的設計：``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>自訂
自訂的選項仍然適用于 AD FS 2019。
以下是其他檔的一些連結，供您參考。

•適用于不打算將伺服器升級至 AD FS 2019，但仍想要新設計的使用者：[在 Active Directory 同盟服務中使用 AZURE AD UX Web 主題](azure-ux-web-theme-in-ad-fs.md)

•自訂的集中位置： [AD FS 使用者登入自訂](ad-fs-user-sign-in-customization.md)
