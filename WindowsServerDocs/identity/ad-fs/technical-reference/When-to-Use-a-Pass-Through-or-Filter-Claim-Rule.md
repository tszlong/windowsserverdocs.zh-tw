---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: "使用通過或篩選理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa205c46bf67dc25a55232b799bdd39fee4ac3c6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>使用通過或篩選理賠要求規則
在 Active Directory 同盟服務時，您需要需要特定傳入理賠要求類型，然後再套用會判斷何種輸出應該就會發生的動作 \(AD FS\) 根據連入宣告值，您可以使用此規則。 當您使用此規則時，您通過或篩選符合下列表格，根據您設定的規則選項規則邏輯操作任何主張。  
  
|規則選項|邏輯規則|  
|---------------|--------------|  
|通過所有宣告值|如果輸入宣告類型等於*指定宣告類型*和值等*的任何值*，然後傳遞透過理賠要求|  
|只有在特定取得值通過|如果輸入宣告類型等於*指定宣告類型*和值等*指定宣告值*，然後傳遞透過宣告|  
|通過符合尾碼值特定 e\ 電子郵件宣告值|如果輸入宣告類型等於*指定宣告類型*和值等*指定尾碼值*，然後傳遞透過理賠要求|  
|通過的 [開始] 的特定值宣告值|如果傳入宣告類型等於*指定宣告類型*，並以開頭值*指定宣告值*，然後傳遞透過理賠要求|  
  
下列章節提供基本簡介取得規則，並提供時使用此規則有關的進一步詳細資料。  
  
## <a name="about-claim-rules"></a>關於理賠要求規則  
宣告規則表示商務邏輯操作，將需要連入宣告、 適用於條件的執行個體 \ （如果 x 然後 y\） 和產生傳出宣告依據條件的參數。 下列清單輪廓重要您進一步讀取之前，您必須知道的相關的提示取得規則本主題中：  
  
-   AD FS snap\ 中管理，在理賠要求規則只能建立使用理賠要求規則範本  
  
-   宣告規則處理程序傳入宣告直接從宣告提供者 \ （例如 Active Directory 或其他聯盟 Service\） 或接受的輸出從轉換宣告提供者信任規則。  
  
-   宣告發行引擎順序特定的規則集中處理理賠要求規則。 藉由設定優先順序規則，可以進一步改善或篩選宣告專特定的規則設定中的上一個規則。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則相同宣告類型處理多個理賠要求值。  
  
如需詳細資訊理賠要求規則及宣告規則集合，請查看[的角色的取得規則](The-Role-of-Claim-Rules.md)。 如需規則的處理方式的相關資訊，請查看[的角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。 宣告規則集合的處理方式的相關資訊，請查看[的角色宣告管線的](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="pass-through-all-claim-values"></a>通過所有宣告值  
當使用此動作，指定的宣告類型的所有傳入宣告值會傳送傳出宣告。 例如時收到宣告類型指定為角色宣告類型，所有傳入理賠要求值是排列複製到新的角色傳出宣告類型的傳出宣告。  
  
## <a name="filtering-a-claim"></a>篩選理賠要求  
AD FS、 字詞在*宣告篩選*代表篩選或限制傳入取得值，使傳遞或傳送到傳出宣告特定的值。 這是**傳遞透過或篩選連入宣告**規則範本可讓這個功能。 在此規則的屬性，您可以設定篩選傳入的值，只有符合您指定的條件值通過條件。  
  
例如，您可以使用此規則只通過宣告符合宣告值的購買時連入取得宣告類型的角色，或是您可能想要發行只相關的使用者名稱宣告輸入相符項目，但不是包含的使用者身分證號碼宣告。  
  
當您使用篩選器有此規則時，所有連入宣告會檢查以判斷哪一個宣告符合規則所設定的條件。 所有其他宣告忽略這樣也會通過符合選取的宣告類型指定理賠要求值。  
  
例如如下所示，規則設定與條件篩選以 UPN 貼近只連入宣告時宣告類型且也結束的@fabrikam.com，除非它們符合此條件忽略所有其他傳入宣告。 這包含連入宣告宣告類型 E\-電子郵件地址，但它宣告值結束在@fabrikam.com。 在此案例，其中包含的值理賠要求Nick@fabrikam.com會傳送至信賴。  
  
![使用 pass 透過](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>宣告提供者信任上設定此規則  
當您使用信任宣告提供者時，可以設定此規則通過只有傳入宣告宣告提供者符合特定限制。 例如，可能會想要只接受 e\-電子郵件宣告宣告提供者。因此，您會使用此規則範本接受結束宣告提供者的網域名稱系統 \(DNS\) 名稱 e\ 郵件宣告並類型。  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>信賴的派對信任上設定此規則  
當您使用信賴的派對信任時，可以設定此規則為通過或篩選傳出宣告將會被傳送至信賴的。 某些信賴派對可能不了解特定宣告類型，或特定宣告可能包含應該不會傳送給特定信賴的機密資訊。 此規則範本可以執行的原則，針對特定信賴廠商信任幫助。  
  
## <a name="how-to-create-this-rule"></a>如何建立本規則  
您建立可以使用此規則理賠要求規則語言或傳遞透過使用或篩選 snap\ 中 AD FS 管理傳入取得規則範本。 此規則範本提供下列設定選項：  
  
-   指定名稱理賠要求規則  
  
-   指定傳入宣告類型  
  
-   通過所有宣告值  
  
-   只有在特定取得值通過  
  
-   通過符合尾碼值特定 e\ 電子郵件宣告值  
  
-   通過的 [開始] 的特定值宣告值  
  
如需如何建立此範本後續的指示操作，[建立傳遞透過規則或篩選連入宣告](https://technet.microsoft.com/library/dd807060.aspx)中的 AD FS 部署。  
  
## <a name="using-the-claim-rule-language"></a>使用語言理賠要求規則  
如果宣告值符合自訂模式時，才應傳送理賠要求，您必須使用 [自訂規則。 如需詳細資訊，查看何時使用自訂規則。  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>如何建構通過或篩選規則語法的範例  
簡單的篩選規則會篩選宣告根據其中一個上述屬性。 例如，下列規則會通過所有 e\-電子郵件宣告：  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
篩選器可以邏輯 AND\-ed 在一起。 例如，下列規則可接受值所有 e\-電子郵件的宣告johndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
在上面範例篩選永遠使用相等電信業者。 宣告規則語言支援下列電信業者：  
  
-   \ = \ = \-等於 \(case\-sensitive\)  
  
-   \ ！ \ = \-不等於 \(case\-sensitive\)  
  
-   \ = ~ \-運算式相符項目  
  
-   \ ！ ~ \-運算式 non\ 比對  
  
例如，下列規則可接受所有 e\-電子郵件宣告不是本機聯盟伺服器發行的 boeing.com 尾碼已：  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>建立自訂規則最佳做法  
下表中所述篩選會套用到一或多個每個宣告，請的屬性。  
  
|取得屬性|描述|  
|------------------|---------------|  
|輸入|宣告類型 \ （通常為 Uri\ 表示） 反映隱含的協議何種資訊表達宣告中相關聯盟合作夥伴。 例如的類型 http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress 宣告會包含使用者 e\-電子郵件地址。|  
|值。|宣告值。 例如，輸入 http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress 的理賠要求可能會有的值johndoe@fabrikam.com|  
|值鍵入|值鍵入代表解譯是理賠要求值中所包含的資訊。 值鍵入通常會設 http:///\/ www.w3.org \/2001\/XMLSchema\#string，但宣告值可能會包含 Base64Binary 編碼資料 \ (例如，image\) 或日期、 布林值，等等。|  
|發行者|發行者代表上次發行有關使用者宣告派對。 如果宣告取得宣告提供者聯盟伺服器的所有宣告發行者移至 [設定為 [本機授權單位 」。 如果宣告收到了聯盟提供者聯盟伺服器，宣告的發行者會將設定宣告提供者已權杖宣告提供者。 因此，當處理規則宣告收到宣告提供者上的所有宣告發行者會設定為相同的值。 當信賴的製作規則，發行者屬性可用來區分宣告來自不同宣告提供者。|  
|OriginalIssuer|這個理賠要求屬性是要代表 microsoft 提供的聯盟伺服器初次發行理賠要求。 宣告的發行者屬性設定的最後一個聯盟伺服器預付碼簽章，因為原始發行者適用於案例，其中理賠要求有流量透過多個聯盟伺服器 \ (例如，從聯盟提供者聯盟伺服器接收權杖信賴可能會有興趣的特定宣告提供者聯盟伺服器驗證使用者 \)|  
|屬性|除了上述五個屬性，每個宣告也有命名的屬性儲存屬性包。 這些屬性權杖中不會序列化以只感知器 」 的元件宣告發行管線單一聯盟伺服器的範圍中之間傳送資訊。 例如，設定屬性期間宣告提供者規則處理並信賴廠商規則參考到。|  
  

