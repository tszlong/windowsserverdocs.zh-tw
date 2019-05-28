---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: 使用 SAML 2.0 改進互通性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4148614ba35ce29f567edb08b94e115d3f9152e9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189097"
---
# <a name="improved-interoperability-with-saml-20"></a>使用 SAML 2.0 改進互通性



  
Windows Server 2016 中的 AD FS 包含其他 SAML 通訊協定支援，包括支援匯入包含多個實體的中繼資料為基礎的信任。  這可讓您設定 AD FS 以參與 confederations InCommon 同盟等其他符合 eGov 2.0 標準的實作。   
  
新的功能為基礎的信賴憑證者的合作對象 」 或 「 宣告提供者信任群組。 每個群組是 EntitiesDescriptor (< md:EntitiesDescriptor >) 元素指定在 eGov 2.0 設定檔，其中包含一或多個 EntityDescriptor 項目。  群組有共同的授權規則，並可以修改所有其他屬性，如同個別的信任物件。  
  
一旦信任群組會匯入到 AD FS 中，AD FS 會自動更新信任為基礎的中繼資料文件的群組。  
  
啟用這些案例是只要使用新的 PowerShell cmdlet，新增和移除 AdfsClaimsProviderTrustsGroup 和 AdfsRelyingPartyTrustsGroup 物件。 這可以使用中繼資料 URL 或檔案，如下列範例所示。  
  
此外，AD FS 2016 都有支援範圍的參數區段 3.4.1.2 SAML Core 規格中所述。 這個項目可讓信賴憑證者合作對象來指定一個或多個身分識別提供者進行驗證要求。  
  
## <a name="examples"></a>範例  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>參考  
  
可以找到 eGov 2.0 設定檔[這裡。](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
您可以找到 SAML Core 規格[這裡。](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


