---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: "宣告管線的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5076a686b5d0b9a539f6cad8594aaf84dccc3edb
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>宣告管線的角色
在 Active Directory 同盟服務 \(AD FS\) 宣告管線代表宣告必須遵守透過同盟服務，可發行之前的路徑。 聯盟服務管理整個 end\ to\ 結束處理程序，透過不同的宣告管線，也會包括理賠要求規則處理理賠要求規則引擎階段流暢索賠項目。  
  
如需理賠要求規則的詳細資訊，請查看[的角色的取得規則](The-Role-of-Claim-Rules.md)。 如需有關如何理賠要求規則引擎處理規則的詳細資訊，請查看[的角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。  
  
下一節討論同盟服務監督在更多詳細資料的程序。  
  
## <a name="claims-pipeline-process"></a>宣告管線程序  
三個 high\ 層級階段所組成宣告管線程序。 此程序的每個階段初始化理賠要求規則引擎階段特定的處理程序理賠要求規則。 這些階段包含 \ (工作順序，它們 occur\):  
  
1.  接受傳入宣告-宣告管線在此階段用來從權杖解壓縮連入宣告並能排除宣告不會如預期般或受信任的。 它們擷取之後，組成接受轉換規則接受規則宣告提供者信任執行設定。 本規則可用通過或新增新宣告，則可在後續宣告管線的階段。 使用這個階段的輸出為第二個和第三個階段輸入。  
  
2.  授權宣告要求-宣告引擎會使用這個階段發行允許或拒絕根據是否允許權杖要求者的特定信賴取得預付碼，或不主張。 不過，這可能是組成發行授權規則授權規則設定或設定委派授權規則之前的在信賴的派對信任。  
  
3.  發行傳出宣告 — 發出傳出宣告，並將它們傳送沿著管線它們封裝安全性權杖到使用這個階段。 不過，這可能會在發行規則組成信賴的派對信任的發行轉換規則之前，請將會判斷功能宣告為傳出宣告將會發行。  
  
所有的三個階段上述執行宣告規則處理，但是使用另一組規則。 每個階段如上文所述，已收到宣告的發行者規則的相關的設定 \(the acceptance rules\) 或目標服務發行 claimincludes \ (授權及發行 rules\)。  
  
宣告 token\ 無關但的安全性權杖中封裝網路傳輸。 無論傳入或傳出的安全性權杖格式宣告操作理賠要求規則。  
  
宣告規則包含 administrator\ 定義的邏輯操作，宣告引擎會接受傳入宣告、授權宣告根據要求者的身分以及發出宣告所需的信賴。 最後則判斷宣告將會移至後宣告已被流量宣告管線會發出的安全性權杖宣告引擎。  
  
如下所示，宣告管線負責整個 end\ to\ 高階程序的傳送到不同的管線階段理賠要求為了最後會傳送到信賴的派對信任發行理賠要求。 傳出宣告圖代表發行理賠要求。  
  
![AD FS 角色](media/adfs2_pipeline.gif)  
  
雖然這不所示，就會執行規則的每個階段的實際處理宣告引擎。 如需詳細資訊，請查看[的角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。  
  

