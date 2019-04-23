---
title: AD FS 疑難排解-宣告規則
description: 本文件說明如何疑難排解與 AD FS 宣告規則語法
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 027b2afc9e580253ec820e7e5be14419387ddd44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837919"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>AD FS 疑難排解-宣告規則語法
宣告是陳述式某主體對於本身或另一個主旨。  信賴憑證者的合作對象所發出的宣告，而且它們會指定一個或多個值，然後封裝在 AD FS 伺服器所發出的安全性權杖。  本文說明的宣告語法和建立。  如需宣告發行請參閱[AD FS 疑難排解-宣告發行](ad-fs-tshoot-claims-issuance.md)。

>[!NOTE]  
>您可以使用[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)上[ADFS 協助](https://adfshelp.microsoft.com)站台，以協助疑難排解問題的宣告。   

## <a name="how-claim-rules-are-processed"></a>宣告規則的處理方式
宣告規則透過處理[宣告管線](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)使用[宣告引擎](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。 宣告引擎是同盟服務的邏輯元件，其會檢查使用者所提供的連入宣告集，而且將接著根據每個規則中的邏輯產生宣告的輸出集。

## <a name="how-to-create-a-claim-rule"></a>如何建立宣告規則
宣告規則是針對同盟服務內的每一個同盟的信任關聯性分別建立，不會跨多個信任共用。 您可以建立規則，以從[宣告規則範本](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md)，從頭開始撰寫規則使用[宣告規則語言](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md)或使用 Windows PowerShell 自訂規則。

## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解宣告規則語言的元件
宣告規則語言包含下列元件，以分隔"= >"運算子：

- 條件-用來檢查輸入的宣告，並判斷是否應該執行這項規則的發佈陳述式。  它代表邏輯的運算式必須評估為 true 來執行規則主體部分。

- 發佈陳述式

範例：

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

下列宣告具有下列項目：
- 條件- `c:[type == "Name", value == "domain user"] ` -評估 windows 帳戶名稱是否為網域使用者的輸入的宣告
- 發行- `issue(type = "Role", value = "employee")` -如果條件為 true，將新的宣告至輸入宣告，具有角色的員工。

如需宣告和語法的詳細資訊，請參閱[The Role of 宣告規則語言](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md)。

## <a name="claims-rule-editor"></a>宣告規則編輯器
語法檢查由宣告規則編輯器中執行，一旦您已完成宣告，然後按**確定**。  因此如果您有不正確的語法然後編輯器可讓您知道。

![宣告](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>事件記錄檔
尋找嘗試解決功能使用的記錄檔的宣告時最好的方法就是尋找宣告輸出。  您可以尋找事件記錄檔中 1000年和 1001年事件。

![宣告](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>建立範例應用程式
您也可以建立範例應用程式回應您的宣告。  例如，您可以使用範例應用程式，並建立具有相同的宣告您嘗試進行疑難排解，並查看應用程式是否有任何問題，宣告的信賴憑證者合作對象。

![宣告](media/ad-fs-tshoot-claims/claim4.png)

此處可良好的範例 web 應用程式。  此應用程式是簡單的 web 應用程式，回應來自信賴憑證者的合作對象接收的宣告。  若要使用此您需要編輯 web.config 應用程式：
- 變更 https://app1.contoso.com/sampapp至 URL，您將使用來裝載 sampapp
- 變更 sts.contoso.com，為您指點 AD FS 同盟伺服器的所有執行個體
- 憑證指紋取代為您的指紋

![宣告](media/ad-fs-tshoot-claims/claims3.png)

下列[部落格文章](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/)絕佳且深入的說明，設定此功能的。

## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)