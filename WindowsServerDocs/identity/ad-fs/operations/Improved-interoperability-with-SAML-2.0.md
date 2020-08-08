---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: 使用 SAML 2.0 改進互通性
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d92859a7f8ae37f847a68dae9ca7fd0245308b6a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954224"
---
# <a name="improved-interoperability-with-saml-20"></a>使用 SAML 2.0 改進互通性




Windows Server 2016 中的 AD FS 包含額外的 SAML 通訊協定支援，包括以包含多個實體的中繼資料為基礎的匯入信任支援。  這可讓您設定 AD FS 以參與同盟 (例如 InCommon 同盟) 和符合 eGov 2.0 標準的其他實作。

這項新功能是以信賴憑證者或宣告提供者信任的群組為基礎。 每個群組都是 eGov 2.0 設定檔中所指定的 EntitiesDescriptor ( # B0 md： EntitiesDescriptor>) 元素，其中包含一或多個 EntityDescriptor 元素。  這些群組具有一般的授權規則，而且所有其他屬性都可以像個別的信任物件一樣進行修改。

一旦信任群組匯入 AD FS，AD FS 就會根據元資料檔案自動將信任更新為群組。

啟用這些案例就像使用新增和移除 AdfsClaimsProviderTrustsGroup 和 AdfsRelyingPartyTrustsGroup 物件的新 PowerShell commandlet 一樣簡單。 這可以使用中繼資料 URL 或檔案來完成，如下列範例所示。

此外，AD FS 2016 支援範圍參數，如 SAML 核心規格3.4.1.2 一節所述。 此元素可讓信賴憑證者針對驗證要求指定一個或多個身分識別提供者。

## <a name="examples"></a>範例

```
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"
```



```
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"
```

## <a name="references"></a>參考

您可以在這裡找到 eGov 2.0 設定檔[。](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)

您可以在這裡找到 SAML 核心規格[。](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)


