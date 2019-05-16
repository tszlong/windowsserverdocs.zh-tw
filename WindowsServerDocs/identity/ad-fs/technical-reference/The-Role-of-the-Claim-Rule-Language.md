---
title: 宣告規則語言的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 05728f04f6fb924cf3793bc843df3832c7c383f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855689"
---
>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

# <a name="the-role-of-the-claim-rule-language"></a>宣告規則語言的角色
Active Directory Federation Services (AD FS) 宣告規則語言做為系統管理的建置組塊，行為之傳入和傳出宣告，而宣告引擎做為宣告規則語言中的邏輯的處理引擎，定義自訂規則。 如需有關如何由宣告引擎處理所有規則，請參閱 < [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>使用宣告規則語言建立自訂宣告規則  
AD FS 提供系統管理員下列選項來定義自訂規則，它們可以用來判斷身分識別宣告與宣告規則語言的行為。 您可以使用本主題中的宣告規則語言語法範例來建立自訂規則，以便列舉、加入、刪除和修改宣告以符合您的組織需求。 您可以在 [使用自訂宣告傳送宣告] 規則範本中鍵入宣告規則語言語法，以建立自訂的規則。  
  
規則會以分號分隔彼此。  
  
如需使用自訂規則時機的詳細資訊，請參閱[使用自訂宣告規則的時機](When-to-Use-a-Custom-Claim-Rule.md)。  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>使用宣告規則範本來了解宣告規則語言語法  
AD FS 也會提供一組預先定義的宣告發佈和接受規則範本，可用來實作常見宣告規則的宣告。 在給定信任的 [編輯宣告規則] 對話方塊中，您可以建立預先定義的規則，並檢視組成該規則的宣告規則語言語法，方法是按一下該規則的 [檢視規則語言] 索引標籤。 使用本節的資訊和 [檢視規則語言] 技術可深入了解如何建構您自己的自訂規則。  
  
如需詳細的宣告規則和宣告規則範本的詳細資訊，請參閱[規則的角色宣告](The-Role-of-Claim-Rules.md)。  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解宣告規則語言的元件  
宣告規則語言包含下列元件，以分隔"= >"運算子：  
  
-   條件  
  
-   發佈陳述式  
  
### <a name="conditions"></a>條件  
您可以使用規則中的條件來檢查輸入宣告，並判斷是否應該執行規則的發佈陳述式。 條件表示必須評估為 true 來執行規則主體部分的邏輯運算式。 如果遺失此組件會假設邏輯為 true，也就是一律執行規則的主體。 條件組件包含一份組合了結合邏輯運算子的條件 ("& &")。 清單中的所有條件必須都評估為 true，才會將整個條件組件評估為 true。 條件可以是宣告選取運算子或彙總函式呼叫。 這兩個條件是互斥的，這表示宣告選取器和彙總函式不能合併在單一規則條件組件中。  
  
條件在規則中為選用。 例如，下列規則沒有條件：  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
有三種類型的條件：  
  
-   單一條件—這是最簡單的條件形式。 會針對只有一個運算式進行檢查例如，windows account name = 網域使用者。  
  
-   多個條件 — 這種情況需要額外的檢查，來處理規則主體; 中的多個運算式例如，windows account name = 網域使用者和群組 = contosopurchasers。  
  
> [!NOTE]  
> 存在另一個條件，但它是單一條件或多個條件的子集。 它被指做為規則運算式 (Regex) 條件。 它是用來接受輸入運算式，並比對該運算式與指定的模式。 以下顯示其用法範例。  
  
下列範例會示範幾個語法建構 (根據條件類型)，可讓您用來建立自訂規則。  
  
#### <a name="single--condition-examples"></a>只需條件範例  
只需在下表會說明運算式的條件。 它們是建構成只檢查含指定宣告類型的宣告，或含指定宣告類型宣告值的宣告。  
  
|條件描述|條件語法範例|  
|-------------------------|----------------------------|  
|此規則有一個條件來檢查含指定的宣告類型的輸入宣告 ("http://test/name」)。 如果符合的宣告位於輸入宣告，則規則會將符合的宣告複製到輸出宣告集。|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|此規則有一個條件來檢查含指定的宣告類型的輸入宣告 (「 http://test/name") 和宣告值 ("Terry")。 如果符合的宣告位於輸入宣告，則規則會將符合的宣告複製到輸出宣告集。|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
-複雜的條件會顯示在下一步 區段中，包括用來檢查有多個宣告、 檢查簽發者的宣告，條件和條件來檢查有符合規則運算式模式的值。  
  
#### <a name="multiple--condition-examples"></a>多個-條件範例  
下表提供範例的多重集的運算式條件。  
  
|條件描述|條件語法範例|  
|-------------------------|----------------------------|  
|此規則有一個條件來檢查是否有兩個輸入宣告，每個都有指定的宣告類型 ("http://test/name" 和 "http://test/email」)。 如果兩個符合的宣告位於輸入宣告中，則規則會將名稱宣告複製到輸出宣告集。|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>一般-條件範例  
下表提供一般運算式的範例為基礎的條件。  
  
|條件描述|條件語法範例|  
|-------------------------|----------------------------|  
|此規則有條件使用規則運算式來檢查電子-郵件宣告結尾為"@fabrikam.com」。 如果符合的宣告位於輸入宣告中，則規則會將符合的宣告複製到輸出宣告集。|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>發佈陳述式  
自訂規則的處理為基礎的發佈陳述式 (*問題*或是*新增*) 您藉由程式設計納入宣告規則。 視所要的結果，可將 issue 陳述式或 add 陳述式寫入規則中，以填入輸入宣告集或輸出宣告集。 使用 add 陳述式的自訂規則只會將宣告值明確地填入輸入宣告集，而使用 issue 陳述式的自訂宣告規則會在輸入宣告集和輸出宣告集中填入宣告值。 當宣告值僅適用於宣告規則集內未來的規則使用時，這項功能可能很有用。  
  
例如在下圖中，會將傳入宣告加入至宣告發佈引擎所設定的輸入宣告。 當第一個自訂宣告規則執行，並符合準則的網域使用者，宣告發行引擎處理規則使用 add 陳述式和 windows 7 中的邏輯**編輯器**新增至輸入的宣告集。 由於 Editor 的值會出現在輸入宣告集合中，因此規則 2 可在其邏輯中成功處理 issue 陳述式，並產生 **Hello** 的新值，系統會其新增到輸出宣告集和輸入宣告集，供規則集內的下一個規則使用。 規則 3 現在可將輸入宣告集內出現的所有值做為輸入來處理其邏輯。  
  
![AD FS 角色](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>宣告發佈動作  
規則主體表示宣告發佈動作。 語言可辨識兩種宣告發佈動作：  
  
-   **Issue 陳述式：** issue 陳述式建立會移至輸入和輸出宣告集的宣告。 例如，下列陳述式會根據其輸入宣告集發佈新宣告：  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Add 陳述式：** add 陳述式會建立只加入至輸入宣告集合中的新宣告。 例如，下列陳述式會將新宣告加入至輸入宣告集：  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
規則的發佈陳述式定義在符合條件時，規則將發佈的宣告。 有兩種與引數和陳述式行為相關的發佈陳述式：  
  
-   **Normal**—Normal 發佈陳述式可使用規則中的常值或符合條件的宣告中值，以發出宣告。 normal 發佈陳述式可以包含一或兩個下列格式：  
  
    -   *宣告複製*：宣告複製會在輸出宣告集中建立現有宣告的副本。 只有與 "issue" 發佈陳述式結合時，此發佈形式才有意義。 若是與 “add” 發佈陳述式結合，則它沒有任何作用。  
  
    -   *新宣告*：此格式會建立新的宣告，提供各種宣告屬性的值。 必須指定 Claim.Type，所有其他宣告屬性是選擇性的。 會忽略此格式的引數順序。  
  
-   **屬性存放區**—這種格式會建立宣告，包含從屬性存放區擷取的值。 您可利用單一發佈陳述式，這一點很重要的屬性擷取期間進行網路或磁碟輸入/輸出 (I/O) 作業的屬性存放區中建立多個宣告類型。 因此，最好限制原則引擎與屬性存放區之間的往返次數。 也可合法對指定的宣告類型建立多個宣告。 當屬性存放區傳回指定宣告類型的多個值時，發佈陳述式會自動為每個傳回的宣告值建立一個宣告。 屬性存放區實作使用 param 引數，使用這些引數所提供的值來替代查詢引數中的預留位置。 預留位置使用.NET String.Format （） 函式相同的語法 (例如{1}，{2}等等)。 此發佈格式的引數順序很重要，而且必須是下列文法中規定的順序。  
  
下表為宣告規則中兩種發佈陳述式描述一些常見的語法建構。  
  
|發佈陳述式類型|發佈陳述式描述|發佈陳述式語法範例|  
|---------------------------|----------------------------------|-------------------------------------|  
|一般|每當使用者具有指定的宣告類型和值，下列規則永遠會發出相同的宣告：|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|一般|下列規則會將一種宣告類型轉換成另一種。 請注意，符合條件 "c" 的宣告值會用於發佈陳述式中。|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|屬性存放區|下列規則使用傳入宣告的值來查詢 Active Directory 屬性存放區：|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|屬性存放區|下列規則使用傳入宣告的值，來查詢先前設定的結構化查詢語言 (SQL) 屬性存放區：|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>運算式  
運算式是用於宣告選取器條件約束和發佈陳述式參數的右側。 有各種語言支援的運算式。 語言中的所有運算式都是以字串為基礎，這表示它們會採用字串做為輸入和產生字串。 數字或其他資料類型 (例如日期/時間) 在運算式中不受支援。 以下是語言支援的運算式類型：  
  
-   字串常值：字串值，這兩端的引號 （"） 字元分隔。  
  
-   運算式的字串串連：會得出由左值和右值串連所產生的字串。  
  
-   函式呼叫：函式由識別項，並將參數傳遞為逗號-分隔的清單，在方括號括住的運算式 ("（)")。  
  
-   宣告的屬性存取格式為「變數名稱.屬性名稱」：適用於指定變數求值之識別宣告的屬性值結果。 變數必須先繫結到宣告選擇器，才能以這種方式使用。 如果變數繫結至宣告選取器的條件約束內部該相同的宣告選擇器，則不可使用該變數。  
  
下列宣告屬性可供存取：  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[屬性\_名稱\]（這個屬性之宣告的屬性集合中找不到屬性名稱 （_n） 時傳回空字串。 )  
  
您可以在運算式中使用要呼叫的 RegexReplace 函式。 這個函式接受輸入運算式，並將其與指定的模式比對。 如果模式符合，則會以取代值來取代符合的輸出。  
  
#### <a name="exists-functions"></a>Exists 函式  
Exists 函式可用於條件中，評估輸入宣告集內是否有符合條件的宣告。 如果存在任何相符的宣告，只會呼叫發佈陳述式一次。 在下列範例中，如果宣告集合中至少有一個宣告將簽發者設為 "MSFT"，“origin” 宣告只會發佈一次 (無論有多少宣告將簽發者設為 "MSFT")。 使用此函式可避免發佈重複的宣告。  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>規則主體  
規則主體只能包含單一發佈陳述式。 如果不使用 Exists 函數來使用條件，則每當條件組件符合時，會執行一次規則主體。  
  
## <a name="additional-references"></a>其他參考資料  
[建立規則，以使用自訂規則傳送宣告](https://technet.microsoft.com/library/dd807049.aspx)  
  

