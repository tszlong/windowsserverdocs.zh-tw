---
title: AD FS 疑難排解-宣告規則
description: 本檔說明如何使用 AD FS 針對宣告規則語法進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.openlocfilehash: 1f4ff57aa825fef008845e228e8d724a1fab9072
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781910"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>AD FS 疑難排解-宣告規則語法
「宣告」（claim）是指某個主旨本身或其他主旨的語句。  宣告是由信賴憑證者所發出，並且會提供一或多個值，然後封裝 AD FS 伺服器所發出的安全性權杖中。  本文將探討宣告語法和建立。  如需宣告發行的相關資訊，請參閱 [AD FS 疑難排解-宣告發行](ad-fs-tshoot-claims-issuance.md)。

>[!NOTE]
>您可以使用[ADFS](https://adfshelp.microsoft.com)說明網站上的[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) ，協助針對宣告問題進行疑難排解。

## <a name="how-claim-rules-are-processed"></a>宣告規則的處理方式
宣告規則會透過使用[宣告引擎](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)的[宣告管線](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)進行處理。 宣告引擎是同盟服務的邏輯元件，其會檢查使用者所提供的連入宣告集，而且將接著根據每個規則中的邏輯產生宣告的輸出集。

## <a name="how-to-create-a-claim-rule"></a>如何建立宣告規則
宣告規則是針對同盟服務內的每一個同盟的信任關聯性分別建立，不會跨多個信任共用。 您可以從「宣告 [規則」範本](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md)建立規則，方法是使用宣告 [規則語言](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) 來撰寫規則，或使用 Windows PowerShell 自訂規則，從頭開始。

## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解宣告規則語言的元件
宣告規則語言包含下列元件，並以 "=>" 運算子分隔：

- 條件-用來檢查輸入宣告，並判斷是否應執行規則的發行語句。  它代表必須評估為 true 的邏輯運算式，才能執行規則主體部分。

- 發佈陳述式

範例：

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");```

下列宣告具有下列各項：
- 條件- `c:[type == "Name", value == "domain user"] ` -評估 windows 帳戶名稱是否為網域使用者的輸入宣告
- 發行- `issue(type = "Role", value = "employee")` -如果條件為 true，則會將新的宣告新增至具有員工角色的輸入宣告。

如需宣告和語法的詳細資訊，請參閱 [宣告規則語言的角色](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md)。

## <a name="claims-rule-editor"></a>宣告規則編輯器
宣告規則編輯器會在您完成宣告後執行語法檢查，然後按一下 **[確定]**。  因此，如果您的語法不正確，編輯器會讓您知道。

![[D F S 管理] 對話方塊的螢幕擷取畫面，其中顯示指出自訂宣告規則語法不正確訊息。](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>事件記錄檔
當您嘗試使用記錄檔來針對宣告進行疑難排解時，最佳方法是尋找宣告輸出。  您可以在事件記錄檔中尋找1000和1001事件。

![[事件內容] 對話方塊的螢幕擷取畫面，其中顯示1000事件 I D 的結果。](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>建立範例應用程式
您也可以建立範例應用程式，讓您的宣告回顯。  例如，您可以使用範例應用程式，並建立具有您嘗試進行疑難排解之相同宣告的信賴憑證者，並查看應用程式是否有該宣告的任何問題。

![顯示在瀏覽器中範例應用程式的螢幕擷取畫面。](media/ad-fs-tshoot-claims/claim4.png)

這裡提供一個良好的範例 web 應用程式。  此應用程式是簡單的 web 應用程式，可回應從信賴憑證者收到的宣告。  若要使用此功能，您必須透過下列方式編輯 web.config 應用程式：
- 變更 https://app1.contoso.com/sampapp 為您將用來裝載範例應用程式的 URL
- 將 sts.contoso.com 的所有實例變更為 AD FS 同盟伺服器的起點
- 以您的指紋取代指紋

![顯示 web 設定檔 Visual Studio 的螢幕擷取畫面。](media/ad-fs-tshoot-claims/claims3.png)

下列 [blog 文章](/archive/blogs/tangent_thoughts/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp) 提供設定此功能的絕佳深入指示。

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
