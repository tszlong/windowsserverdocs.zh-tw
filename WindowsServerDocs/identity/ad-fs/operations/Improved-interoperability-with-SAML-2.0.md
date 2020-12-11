---
description: 深入瞭解：使用 SAML 2.0 改善互通性
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: 使用 SAML 2.0 改進互通性
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 741e3370b2957bfaa4f5e908617f6c35210f4a22
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039817"
---
# <a name="improved-interoperability-with-saml-20"></a>使用 SAML 2.0 改進互通性




Windows Server 2016 中的 AD FS 包含額外的 SAML 通訊協定支援，包括以包含多個實體的中繼資料為基礎的匯入信任支援。  這可讓您設定 AD FS 以參與同盟 (例如 InCommon 同盟) 和符合 eGov 2.0 標準的其他實作。

這項新功能是根據信賴憑證者或宣告提供者信任的群組。 每個群組都是 EntitiesDescriptor ( # B0 md： EntitiesDescriptor>) 元素，如 eGov 2.0 設定檔中所指定，其中包含一或多個 EntityDescriptor 元素。  這些群組具有一般授權規則，而且可以修改所有其他屬性，就像個別的信任物件一樣。

一旦將信任群組匯入 AD FS，AD FS 就會根據元資料檔案，自動將信任更新為群組。

啟用這些案例就像使用新增和移除 AdfsClaimsProviderTrustsGroup 和 AdfsRelyingPartyTrustsGroup 物件的新 PowerShell commandlet 一樣簡單。 這可以使用中繼資料 URL 或檔案來完成，如下列範例所示。

此外，AD FS 2016 支援範圍參數，如 SAML 核心規格的3.4.1.2 一節中所述。 此元素可讓信賴憑證者指定驗證要求的一或多個身分識別提供者。

## <a name="examples"></a>範例

```
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"
```



```
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"
```

## <a name="references"></a>參考資料

您可以在這裡找到 eGov 2.0 設定檔 [。](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)

您可以在這裡找到 SAML Core 規格 [。](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)


