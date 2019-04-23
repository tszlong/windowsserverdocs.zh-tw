---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: 宣告管線的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5076a686b5d0b9a539f6cad8594aaf84dccc3edb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887059"
---
>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>宣告管線的角色
宣告管線中 Active Directory Federation Services \(AD FS\)表示宣告必須行經同盟服務才可發行的路徑。 Federation Service 會管理整個端\-至\-結束處理程序宣告行經宣告管線，其中也包括宣告規則引擎的 宣告規則處理的各個階段。  
  
如需宣告規則的詳細資訊，請參閱[規則的角色宣告](The-Role-of-Claim-Rules.md)。 如需宣告規則引擎如何處理規則的詳細資訊，請參閱 [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  
下列章節會更詳細地討論同盟服務監視的程序。  
  
## <a name="claims-pipeline-process"></a>宣告管線處理程序  
宣告管線處理程序包含三個高\-層級階段。 此程序的每個階段會初始化宣告規則引擎來處理該階段的特定宣告規則。 這些階段包含\(它們出現的順序\):  
  
1.  接受連入宣告 - 宣告管線中的這個階段用來從權杖擷取連入宣告，並排除未預期或未受信任的宣告。 在擷取之後，組成宣告提供者信任的接受轉換規則集的接受規則即會執行。 這些規則可以用來傳遞或加入之後可以在宣告管線的後續階段中使用的新宣告。 這個階段的輸出可做為第二個和第三個階段的輸入。  
  
2.  授權宣告要求者 - 宣告引擎會使用這個階段依據權杖要求者是否被允許取得指定信賴憑證者的權杖，來發出允許或拒絕宣告。 不過，在發生此情況之前，會執行構成發行授權規則集或信賴憑證者信任的委派授權規則集的授權規則。  
  
3.  發出連出宣告 - 這個階段是用來發出連出宣告並將它們沿著將被封裝為安全性權杖的管線傳送。 不過，在發生此情況之前，會執行組成信賴憑證者信任的發行轉換規則集，這將會決定哪些宣告會發出為連出宣告。  
  
上述所有三個階段會執行宣告規則處理，但使用不同的規則集。 每個階段如上面所述，有一組相關聯的規則，根據連入宣告的發行者\(接受規則\)，或是目標服務發行 claimincludes\(授權和發佈規則\)。  
  
宣告是權杖\-無從驗證，但封裝在安全性權杖在網路傳輸。 宣告規則會對宣告運作，而不論傳入或傳出的安全性權杖的格式為何。  
  
宣告規則包含系統管理員\-用宣告引擎會接受連入宣告、 授權宣告要求者的身分識別為基礎，並發出宣告的已定義的邏輯所需的信賴憑證者的合作對象。 最後，在宣告行經宣告管線之後，宣告引擎會決定將移到將發出的安全性權杖的宣告。  
  
下圖所示，宣告管線負責整個端\-至\-結束處理程序的讓宣告行經各個管線階段，以最終獲得將透過信賴憑證者傳送的發出宣告信任。 圖中的連出宣告表示發出的宣告。  
  
![AD FS 角色](media/adfs2_pipeline.gif)  
  
雖然未在圖例中顯示，它是在每個階段執行規則的實際處理的宣告引擎。 如需詳細資訊，請參閱 [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  

