---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: "已改善交互操作 SAML 2.0"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f55eaacec8ee0eb41e1980f1aa15c6256f8b979
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="improved-interoperability-with-saml-20"></a>已改善交互操作 SAML 2.0

>適用於：Windows Server 2016

  
在 Windows Server 2016 AD FS 包含其他 SAML 通訊協定支援，包括根據中繼資料包含多個項目信任匯入的支援。  這可讓您設定在 confederations InCommon 聯盟和其他實作 eGov 2.0 一般符合參與 AD FS。   
  
新的功能根據信賴或宣告提供者信任的群組。 每個群組是 EntitiesDescriptor (< md:EntitiesDescriptor >) 為 eGov 中指定 2.0 的個人檔案，包含一或多個 EntityDescriptor 項目。  群組有常見的授權規範，且可以修改所有其他屬性，例如個人信任物件。  
  
在信任群組匯入 AD FS，AD FS 自動更新信任為基礎的中繼資料文件群組。  
  
讓這些案例中，就像簡單使用新的 PowerShell commandlets 該新增和移除 AdfsClaimsProviderTrustsGroup AdfsRelyingPartyTrustsGroup 物件。 這可以使用 URL 中繼資料或檔案，以下的範例所示。  
  
此外，AD FS 2016 已支援的範圍參數，SAML 核心規格 3.4.1.2 一節中所述。 此項目可可以指定一方，或要求其他驗證身分提供者。  
  
## <a name="examples"></a>範例  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>資訊尋找參考資料  
  
找不到 eGov 2.0 設定檔[在此。](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
找不到 SAML 核心規格[在此。](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


