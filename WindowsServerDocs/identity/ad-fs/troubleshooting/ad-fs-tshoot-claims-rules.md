---
title: AD FS 疑難排解-宣告規則
description: 本檔說明如何使用 AD FS 對宣告規則語法進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 94e841282963e2b2b6ada552b54c7732d965b6b6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955970"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>AD FS 疑難排解-宣告規則語法
「宣告」（claim）是一個主體對自己或其他主旨的相關語句。  宣告是由信賴憑證者所發出，而且會指定一個或多個值，然後封裝在 AD FS 伺服器所發出的安全性權杖中。  本文會說明宣告語法和建立。  如需宣告發行的詳細資訊，請參閱[AD FS 疑難排解-宣告發行](ad-fs-tshoot-claims-issuance.md)。

>[!NOTE]  
>您可以在[ADFS](https://adfshelp.microsoft.com)說明網站上使用[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) ，協助針對宣告問題進行疑難排解。   

## <a name="how-claim-rules-are-processed"></a>宣告規則的處理方式
宣告規則會透過使用[宣告引擎](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)的[宣告管線](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)來處理。 宣告引擎是同盟服務的邏輯元件，其會檢查使用者所提供的連入宣告集，而且將接著根據每個規則中的邏輯產生宣告的輸出集。

## <a name="how-to-create-a-claim-rule"></a>如何建立宣告規則
宣告規則是針對同盟服務內的每一個同盟的信任關聯性分別建立，不會跨多個信任共用。 您可以從宣告[規則範本](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md)建立規則、使用宣告[規則語言](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md)撰寫規則，或使用 Windows PowerShell 自訂規則，從頭開始。

## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解宣告規則語言的元件
宣告規則語言包含下列元件，並以 "=>" 運算子分隔：

- 條件-用來檢查輸入宣告，並判斷是否應該執行規則的發行語句。  它代表必須評估為 true 才能執行規則主體部分的邏輯運算式。

- 發佈陳述式

範例：

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

下列宣告具有下列各項：
- 條件- `c:[type == "Name", value == "domain user"] ` -評估 windows 帳戶名稱是否為網域使用者的輸入宣告
- 發行- `issue(type = "Role", value = "employee")` -如果條件為 true，則會將新的宣告加入具有 employee 角色的輸入宣告。

如需宣告和語法的詳細資訊，請參閱[宣告規則語言的角色](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md)。

## <a name="claims-rule-editor"></a>宣告規則編輯器
當您完成宣告並按一下 **[確定]** 之後，[宣告規則編輯器] 就會執行語法檢查。  因此，如果您的語法不正確，則編輯器會讓您知道。

![claims](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>事件記錄檔
當您嘗試使用記錄檔進行宣告的疑難排解時，最好的方法是尋找宣告輸出。  您可以在事件記錄檔中尋找1000和1001事件。

![claims](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>建立範例應用程式
您也可以建立回應宣告的範例應用程式。  例如，您可以使用範例應用程式，並建立具有您嘗試進行疑難排解之相同宣告的信賴憑證者，並查看應用程式是否有該宣告的任何問題。

![claims](media/ad-fs-tshoot-claims/claim4.png)

這裡提供一個良好的範例 web 應用程式。  此應用程式是簡單的 web 應用程式，可回應它從信賴憑證者收到的宣告。  若要使用此功能，您必須藉由下列方式編輯 web.config 應用程式：
- 變更 https://app1.contoso.com/sampapp 為您將用來裝載 sampapp 的 URL
- 變更 sts.contoso.com 的所有實例，以指向您 AD FS 的同盟伺服器
- 以您的指紋取代指紋

![claims](media/ad-fs-tshoot-claims/claims3.png)

下列的[blog 文章](/archive/blogs/tangent_thoughts/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp)有絕佳的深入指示，可讓您進行這項設定。

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
