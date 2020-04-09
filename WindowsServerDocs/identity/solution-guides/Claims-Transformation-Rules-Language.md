---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: 宣告轉換規則語言
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f391c3f8ef2bb5b12f0dd15db55df4f861c05f9b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861271"
---
# <a name="claims-transformation-rules-language"></a>宣告轉換規則語言

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

跨樹系宣告轉換功能可讓您藉由設定跨樹系信任的宣告轉換原則，跨樹系界限橋接動態存取控制的宣告。 所有原則的主要元件都是以宣告轉換規則語言撰寫的規則。 本主題提供有關此語言的詳細資料，並提供撰寫宣告轉換規則的相關指引。  
  
跨樹系信任的轉換原則的 Windows PowerShell Cmdlet 有選項可設定一般案例中所需的簡單原則。 這些 Cmdlet 會將使用者輸入轉譯為宣告轉換規則語言中的原則和規則，然後以指定的格式將它們儲存在 Active Directory 中。 如需宣告轉換 Cmdlet 的詳細資訊，請參閱[動態存取控制的 AD DS Cmdlet](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
根據 Active Directory 樹系中跨樹系信任的宣告設定和需求而定，您的宣告轉換原則可能必須比 Windows PowerShell Cmdlet 針對 Active Directory 所支援的原則更複雜。 若要有效地撰寫這類原則，必須瞭解宣告轉換規則語言語法和語義。 Active Directory 中的這個宣告轉換規則語言（「語言」）是[Active Directory 同盟服務](https://go.microsoft.com/fwlink/?LinkId=243982)用於類似用途的語言子集，而且具有非常類似的語法和語義。 不過，允許的作業數目較少，且其他語法限制會放在語言的 Active Directory 版本中。  
  
本主題簡要說明 Active Directory 中宣告轉換規則語言的語法和語義，以及撰寫原則時所需的考慮。 其中提供幾組範例規則，協助您開始使用，以及不正確的語法和其產生的訊息範例，以便在您撰寫規則時，説明您解密錯誤訊息。  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>撰寫宣告轉換原則的工具  
**適用于 Active Directory 的 Windows PowerShell Cmdlet**：這是撰寫和設定宣告轉換原則的慣用和建議方式。 這些 Cmdlet 會提供簡單原則的參數，並驗證針對較複雜原則所設定的規則。  
  
**LDAP**：透過輕量型目錄存取協定（LDAP），可以在 Active Directory 中編輯宣告轉換原則。 不過，不建議您這樣做，因為原則有數個複雜的元件，而您使用的工具在將原則寫入 Active Directory 之前，可能不會驗證該原則。 這可能需要相當長的時間來診斷問題。  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Active Directory 宣告轉換規則語言  
  
### <a name="syntax-overview"></a>語法總覽  
以下是語言語法和語義的簡短總覽：  
  
-   宣告轉換規則集包含零個或多個規則。 每個規則都有兩個作用中元件： [**選取條件清單**] 和 [**規則動作**]。 如果 [**選取條件] 清單**評估為 TRUE，則會執行對應的規則動作。  
  
-   **選取條件清單**有零個或多個**選取條件**。 所有**選取條件**都必須評估為 true，[**選取條件] 清單**才會評估為 true。  
  
-   每個**選取條件**都有一組零或多個**相符的條件**。 所有**符合條件**都必須評估為 True，Select 條件才會評估為 true。 所有這些條件都會針對單一宣告進行評估。 符合**Select 條件**的宣告可以透過**識別碼**標記，並在**規則動作**中參考。  
  
-   每個比對**條件**都會使用不同的**條件運算子**和**字串常**值，指定條件以符合宣告的**類型**或**值**或**ValueType** 。  
  
    -   當您指定某個**值**的比對**條件**時，您也必須指定特定**ValueType**的比對**條件**，反之亦然。 這些條件必須在語法中彼此相鄰。  
  
    -   **Valuetype**比對條件必須只使用特定的**ValueType**常值。  
  
-   **規則動作**可以複製一個以**識別碼**標記的宣告，或根據已標記識別項和/或指定字串常值的宣告發出一個宣告。  
  
**範例規則**  
  
這個範例會顯示一個規則，可用來轉譯兩個樹系之間的宣告類型，但前提是它們使用相同的宣告 ValueTypes，而且對此類型的宣告值具有相同的解讀方式。 此規則有一個比對條件和一個使用字串常值的 Issue 語句和一個相符的宣告參考。  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>執行時間作業  
請務必瞭解宣告轉換的執行時間作業，以便有效地撰寫規則。 執行時間作業會使用三組宣告：  
  
1.  **輸入宣告集**：提供給宣告轉換作業的宣告輸入集。  
  
2.  **工作宣告集**：在宣告轉換期間讀取和寫入的中繼宣告。  
  
3.  **輸出宣告集**：宣告轉換作業的輸出。  
  
以下是執行時間宣告轉換作業的簡短總覽：  
  
1.  宣告轉換的輸入宣告是用來初始化工作宣告集。  
  
    1.  在處理每個規則時，會將工作宣告集用於輸入宣告。  
  
    2.  規則中的選取條件清單會與工作宣告集合中所有可能的宣告集合進行比對。  
  
    3.  每一組相符的宣告都會用來執行該規則中的動作。  
  
    4.  執行規則動作會產生一個宣告，並將其附加至輸出宣告集和工作宣告集。 因此，規則的輸出會用來做為規則集內後續規則的輸入。  
  
2.  規則集中的規則會依照第一個規則的順序來處理。  
  
3.  處理整個規則集時，會處理輸出宣告集，以移除重複的宣告和其他安全性問題。產生的宣告是宣告轉換程式的輸出。  
  
您可以根據先前的執行時間行為來撰寫複雜的宣告轉換。  
  
**範例：執行時間作業**  
  
這個範例會顯示使用兩個規則之宣告轉換的執行時間作業。  
  
```  
  
     C1:[Type=="EmpType", Value=="FullTime",ValueType=="string"] =>  
                Issue(Type=="EmployeeType", Value=="FullTime",ValueType=="string");  
     [Type=="EmployeeType"] =>   
               Issue(Type=="AccessType", Value=="Privileged", ValueType=="string");  
Input claims and Initial Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
After Processing Rule 1:  
 Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"), (Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  
After Processing Rule 2:  
Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
Final Output:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
```  
  
### <a name="special-rules-semantics"></a>特殊規則的語義  
以下是規則的特殊語法：  
  
1.  空白規則集 = = 沒有輸出宣告  
  
2.  空白選取條件清單 = = 每個宣告符合選取條件清單  
  
    **範例：空白的選取條件清單**  
  
    下列規則會符合工作集中的每個宣告。  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  空白選取相符清單 = = 每個宣告符合選取條件清單  
  
    **範例：空的比對條件**  
  
    下列規則會符合工作集中的每個宣告。 這是基本的「全部允許」規則（如果單獨使用的話）。  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>安全性考量  
**輸入樹系的宣告**  
  
傳入樹系的主體所呈現的宣告必須經過徹底檢查，以確保我們只允許或發行正確的宣告。 不正確的宣告可能會危害樹系安全性，而且在撰寫進入樹系的宣告的轉換原則時，這應該是最重要的考慮。  
  
Active Directory 具有下列功能，可防止輸入樹系的宣告設定錯誤：  
  
-   如果樹系信任未針對輸入樹系的宣告設定宣告轉換原則，則為了安全起見，Active Directory 會卸載所有進入樹系的主體宣告。  
  
-   如果在輸入樹系的宣告上執行規則集會產生未在樹系中定義的宣告，則會從輸出宣告中卸載未定義的宣告。  
  
**離開樹系的宣告**  
  
離開樹系的宣告與進入樹系的宣告相比，呈現的樹系安全性考慮較低。 即使沒有任何對應的宣告轉換原則，也允許宣告讓樹系保持原狀。 您也可以發出未在樹系中定義的宣告，做為離開樹系的轉換宣告的一部分。 這可讓您輕鬆地設定宣告的跨樹系信任。 系統管理員可以判斷輸入樹系的宣告是否需要轉換，並設定適當的原則。 例如，如果需要隱藏宣告以防止資訊洩漏，系統管理員可以設定原則。  
  
**宣告轉換規則中的語法錯誤**  
  
如果指定的宣告轉換原則具有語法不正確的規則集，或如果有其他語法或儲存問題，原則就會被視為無效。 其處理方式不同于先前所述的預設條件。  
  
Active Directory 無法判斷在此情況下的意圖，並進入不安全的模式，因為在該信任上不會產生任何輸出宣告，而是要進行遍歷的方向。 需要系統管理員介入才能更正此問題。 如果使用 LDAP 來編輯宣告轉換原則，就可能發生這種情況。 適用于 Active Directory 的 Windows PowerShell Cmdlet 已進行驗證，以避免寫入具有語法問題的原則。  
  
## <a name="other-language-considerations"></a>其他語言考慮  
  
1.  這種語言有幾個特殊的關鍵字或字元（稱為「終端機」）。 這些會在本主題稍後的[語言終端](Claims-Transformation-Rules-Language.md#BKMK_LT)表中提供。 這些錯誤訊息會使用這些終端機的標記來進行混淆。  
  
2.  終端機有時可以用來做為字串常值。 不過，這類使用方式可能會與語言定義衝突，或產生非預期的結果。 不建議使用這種用法。  
  
3.  規則動作無法在宣告值上執行任何類型轉換，且包含這類規則動作的規則集會被視為無效。 這會造成執行階段錯誤，而且不會產生任何輸出宣告。  
  
4.  如果規則動作參考到規則的 [選取條件] 清單部分中未使用的識別碼，則這是不正確使用方式。 這會導致語法錯誤。  
  
    **範例：不正確的識別碼參考**  
    下列規則說明規則動作中使用的不正確識別碼。  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>範例轉換規則  
  
-   **允許特定類型的所有宣告**  
  
    精確類型  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **不允許特定的宣告類型**  
    精確類型  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>規則剖析器錯誤的範例  
宣告轉換規則會由自訂剖析器剖析，以檢查語法錯誤。 此剖析器是由相關的 Windows PowerShell Cmdlet 執行，然後才將規則儲存在 Active Directory 中。 剖析規則時的任何錯誤（包括語法錯誤）都會列印在主控台上。 網域控制站也會在使用規則來轉換宣告之前執行剖析器，並將錯誤記錄在事件記錄檔中（新增事件記錄檔編號）。  
  
本節說明以不正確的語法撰寫的一些規則範例，以及剖析器所產生的對應語法錯誤。  
  
1. 範例：  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   這個範例使用分號來取代冒號。   
   **錯誤訊息：**  
   *POLICY0002：無法剖析原則資料。*  
   *行號：1，欄號：2，錯誤標記：;。行： ' c1;[] = > 問題（宣告 = c1）; '。*  
   *剖析器錯誤： ' POLICY0030：語法錯誤，非預期的 '; '，應為下列其中一項： '： '。 '*  
  
2. 範例：  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   在此範例中，複製發行語句中的 Identifier 標記是未定義的。   
   **錯誤訊息**：   
   *POLICY0011：宣告規則中沒有符合 CopyIssuanceStatement： ' c2 ' 中指定之條件標記的條件。*  
  
3. 範例：  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool" 不是語言中的終端機，而且不是有效的 ValueType。 下列錯誤訊息中會列出有效的終端機。   
   **錯誤訊息：**  
   *POLICY0002：無法剖析原則資料。*  
   行號：1，欄號：39，錯誤標記： "bool"。 行： ' c1： [type = = "x1"，value = = "1"，valuetype = = "bool"] = > Issue （宣告 = c1）; '。   
   *剖析器錯誤： ' POLICY0030：語法錯誤，非預期的 ' 字串 '，應為下列其中一項： ' INT64_TYPE ' ' UINT64_TYPE ' ' STRING_TYPE ' ' BOOLEAN_TYPE ' ' IDENTIFIER '*  
  
4. 範例：  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   此範例中的數位**1**不是語言中的有效權杖，因此在比對條件中不允許這種用法。 必須以雙引號括住，使其成為字串。   
   **錯誤訊息：**  
   *POLICY0002：無法剖析原則資料。*  
   *行號：1，欄號：23，錯誤標記：1。行： ' c1： [類型 = = "x1"，值 = = 1，valuetype = = "bool"] = > 問題（宣告 = c1）; '。* 剖析器<em>錯誤： ' POLICY0029：非預期的輸入。</em>  
  
5. 範例：  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   這個範例使用了雙等號（= =），而不是單一等號（=）。   
   **錯誤訊息：**  
   *POLICY0002：無法剖析原則資料。*  
   *行號：1，欄號：91，錯誤標記： = =。行： ' c1： [type = = "x1"，value = = "1"，*  
   *valuetype = = "boolean"] = > 問題（類型 = c1. 類型，值 = "0"，valuetype = = "boolean"）; '。*  
   *剖析器錯誤： ' POLICY0030：語法錯誤，非預期的 ' = = '，應為下列其中一項： ' = '*  
  
6. 範例：  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   這個範例在語法上和語義正確。 不過，將 "boolean" 當做字串值使用會造成混淆，因此應該避免。 如先前所述，應盡可能避免使用語言終端機作為宣告值。  
  
## <a name="language-terminals"></a><a name="BKMK_LT"></a>語言終端機  
下表列出宣告轉換規則語言中所使用的一組完整的終端字串和相關聯的語言終端機。 這些定義會使用不區分大小寫的 UTF-16 字串。  
  
|String|終端機|  
|----------|------------|  
|"= >"|兩者|  
|";"|均|  
|":"|加|  
|","|千|  
|"."|網點|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|均衡|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|值賦|  
|「& &」|和|  
|問題|問題|  
|型|TYPE|  
|value|VALUE|  
|valuetype|VALUE_TYPE|  
|報銷|報銷|  
|"[_A-Za-z] [_A-Za-z0-9] *"|標識|  
|"\\" [^\\"\n] *\\" "|字串|  
|uint64|UINT64_TYPE|  
|int64|INT64_TYPE|  
|"字串"|STRING_TYPE|  
|true|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>語言語法  
下列宣告轉換規則語言是在 ABNF 表單中指定。 除了此處定義的 ABNF 生產以外，此定義還會使用上表中所指定的終端機。 規則必須以 UTF-16 編碼，而且字串比較必須視為不區分大小寫。  
  
```  
Rule_set        = ;/*Empty*/  
             / Rules  
Rules         = Rule  
             / Rule Rules  
Rule          = Rule_body  
Rule_body       = (Conditions IMPLY Rule_action SEMICOLON)  
Conditions       = ;/*Empty*/  
             / Sel_condition_list  
Sel_condition_list   = Sel_condition  
             / (Sel_condition_list AND Sel_condition)  
Sel_condition     = Sel_condition_body  
             / (IDENTIFIER COLON Sel_condition_body)  
Sel_condition_body   = O_SQ_BRACKET Opt_cond_list C_SQ_BRACKET  
Opt_cond_list     = /*Empty*/  
             / Cond_list  
Cond_list       = Cond  
             / (Cond_list COMMA Cond)  
Cond          = Value_cond  
             / Type_cond  
Type_cond       = TYPE Cond_oper Literal_expr  
Value_cond       = (Val_cond COMMA Val_type_cond)  
             /(Val_type_cond COMMA Val_cond)  
Val_cond        = VALUE Cond_oper Literal_expr  
Val_type_cond     = VALUE_TYPE Cond_oper Value_type_literal  
claim_prop       = TYPE  
             / VALUE  
Cond_oper       = EQ  
             / NEQ  
             / REGEXP_MATCH  
             / REGEXP_NOT_MATCH  
Literal_expr      = Literal  
             / Value_type_literal  
  
Expr          = Literal  
             / Value_type_expr  
             / (IDENTIFIER DOT claim_prop)  
Value_type_expr    = Value_type_literal  
             /(IDENTIFIER DOT VALUE_TYPE)  
Value_type_literal   = INT64_TYPE  
             / UINT64_TYPE  
             / STRING_TYPE  
             / BOOLEAN_TYPE  
Literal        = STRING  
Rule_action      = ISSUE O_BRACKET Issue_params C_BRACKET  
Issue_params      = claim_copy  
             / claim_new  
claim_copy       = CLAIM ASSIGN IDENTIFIER  
claim_new       = claim_prop_assign_list  
claim_prop_assign_list = (claim_value_assign COMMA claim_type_assign)  
             /(claim_type_assign COMMA claim_value_assign)  
claim_value_assign   = (claim_val_assign COMMA claim_val_type_assign)  
             /(claim_val_type_assign COMMA claim_val_assign)  
claim_val_assign    = VALUE ASSIGN Expr  
claim_val_type_assign = VALUE_TYPE ASSIGN Value_type_expr  
Claim_type_assign   = TYPE ASSIGN Expr  
  
```  
  


