---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: 宣告管線的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6aafa37b06599f4114cf076e87415fece128fb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407336"
---
# <a name="the-role-of-the-claims-pipeline"></a>宣告管線的角色
Active Directory 同盟服務\( ADFS\)中的宣告管線表示宣告必須遵循同盟服務的路徑，然後才可以發出。 此同盟服務會管理整個端\-對\-端程式，以透過宣告管線的各個階段來流動宣告，其中也包含由宣告規則引擎處理宣告規則的流程。  
  
如需宣告規則的詳細資訊，請參閱宣告[規則的角色](The-Role-of-Claim-Rules.md)。 如需宣告規則引擎如何處理規則的詳細資訊，請參閱 [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  
下列章節會更詳細地討論同盟服務監視的程序。  
  
## <a name="claims-pipeline-process"></a>宣告管線處理程序  
宣告管線進程是由三個高階\-階段所組成。 此程序的每個階段會初始化宣告規則引擎來處理該階段的特定宣告規則。 這些階段包含\(在其發生\)的順序中：  
  
1.  接受連入宣告 - 宣告管線中的這個階段用來從權杖擷取連入宣告，並排除未預期或未受信任的宣告。 在擷取之後，組成宣告提供者信任的接受轉換規則集的接受規則即會執行。 這些規則可以用來傳遞或加入之後可以在宣告管線的後續階段中使用的新宣告。 這個階段的輸出可做為第二個和第三個階段的輸入。  
  
2.  授權宣告要求者 - 宣告引擎會使用這個階段依據權杖要求者是否被允許取得指定信賴憑證者的權杖，來發出允許或拒絕宣告。 不過，在發生此情況之前，會執行構成發行授權規則集或信賴憑證者信任的委派授權規則集的授權規則。  
  
3.  發出連出宣告 - 這個階段是用來發出連出宣告並將它們沿著將被封裝為安全性權杖的管線傳送。 不過，在發生此情況之前，會執行組成信賴憑證者信任的發行轉換規則集，這將會決定哪些宣告會發出為連出宣告。  
  
上述所有三個階段會執行宣告規則處理，但使用不同的規則集。 如上所述，每個階段都有一組相關聯的規則，這是根據傳入宣告\(的簽發者，接受規則\)或 claimincludes 正在發出\(授權的目標服務，發行規則\)。  
  
宣告與權杖\-無關，但會透過封裝在安全性權杖中的網路傳輸。 宣告規則會對宣告運作，而不論傳入或傳出的安全性權杖的格式為何。  
  
宣告規則包含系統管理員\-定義的邏輯，宣告引擎將接受傳入宣告、根據要求者的身分識別授權宣告，以及發出信賴憑證者所需的宣告。 最後，在宣告行經宣告管線之後，宣告引擎會決定將移到將發出的安全性權杖的宣告。  
  
如下圖所示，宣告管線會負責整個端\-對\-端程式，以透過各種管線階段來流動宣告，最後再以已發行的宣告傳送給信賴憑證合作物件信任。 圖中的連出宣告表示發出的宣告。  
  
![AD FS 角色](media/adfs2_pipeline.gif)  
  
雖然未在圖例中顯示，它是在每個階段執行規則的實際處理的宣告引擎。 如需詳細資訊，請參閱 [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  

