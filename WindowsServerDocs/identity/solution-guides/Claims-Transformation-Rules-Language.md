---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: "宣告轉換規則語言"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="claims-transformation-rules-language"></a>宣告轉換規則語言

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

跨樹系宣告轉換功能可讓您橋接器宣告動態存取控制跨樹系邊界跨樹系信任上設定宣告轉換原則。 主要的所有原則元件是規則宣告轉換規則語言中所撰寫。 本主題提供這種語言詳細資料，並提供關於製作宣告轉換規則指導方針。  
  
跨樹系轉換原則的 Windows PowerShell cmdlet 信任選項，將簡單原則設定需要共同案例。 下列 cmdlet 原則和宣告轉換規則語言，規則的使用者輸入的翻譯，然後規定的格式將它們儲存在 Active Directory 中。 如需宣告轉換 cmdlet 的詳細資訊，請查看[適用於動態存取控制 AD DS Cmdlet](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
根據您的宣告設定和放在您的 Active Directory 森林中的跨樹系信任的需求，宣告轉換原則可能會比 Windows PowerShell cmdlet Active Directory 的支援原則更複雜。 有效撰寫此類原則，請務必宣告轉換規則語言語法和語意了解。 這宣告 Active Directory 中的轉換規則語言（「語言」）是使用的語言子集[Active Directory 同盟服務](https://go.microsoft.com/fwlink/?LinkId=243982)類似的用途，並具有非常相似語法和語意的。 但是，有較少的作業，允許和其他語法限制位於的語言 Active Directory 版本。  
  
本主題短暫解釋語法和語意的 Active Directory 和考量製作原則時所宣告轉換規則語言。 它會提供多組規則範例可協助您開始使用，並正確語法和他們產生，可協助您解密的錯誤訊息，當您撰寫規則訊息的範例。  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>工具製作宣告轉換原則  
**Windows PowerShell cmdlet Active Directory 的**：這是撰寫和轉換原則設定宣告慣用與建議的方法。 這些 cmdlet 提供簡單原則切換，並確認規則複雜的原則設定。  
  
**LDAP**：宣告轉換原則可以在 Active Directory 透過輕量型 Directory 存取通訊協定 (LDAP) 編輯。 不過，建議您不要原則會有數個複雜元件，因為它寫入 Active Directory 之前，您所使用的工具可能不驗證原則。 接下來，這可能需要大量的時間來診斷的問題。  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Active Directory 宣告轉換規則語言  
  
### <a name="syntax-overview"></a>語法概觀  
以下是概觀語法語意的語言：  
  
-   宣告轉換規則組合包含或多個規則。 每個規則具有兩個使用的部分：**選擇條件清單**和**規則動作**。 如果**選擇條件清單**為 TRUE，會執行規則對應的動作。  
  
-   **條件] 清單中選取 [**已經零或更多**選取條件**。 所有的**選取條件**為 TRUE 必須評估**清單中選取條件**為 TRUE 評估。  
  
-   每個**選擇條件**有一組零或更多的**符合的條件**。 所有**符合的條件**設為 TRUE 選取條件為 TRUE 評估必須評估。 所有的條件被評估單一理賠要求。 理賠要求符合**選取條件**可以標記，**識別碼**中，**規則動作**。  
  
-   每個**符合的條件**指定要符合的條件**輸入**或**值**或**值鍵入**理賠要求使用不同的**條件電信業者**和**字串文字**。  
  
    -   當您指定**符合的條件**的**值**，您還必須指定**符合的條件**特定**值鍵入**，反之亦然。 這些條件語法必須彼此旁邊。  
  
    -   **值鍵入**符合的條件必須使用特定**值鍵入**只文字。  
  
-   A**規則動作**可以複製一個宣告標記的**識別碼**或發行一個宣告根據標記的識別字理賠要求和（或）提供字串文字。  
  
**範例規則**  
  
此範例可用於翻譯輸入宣告之間兩個樹系，提供，他們使用的相同宣告 Valuetype 並具有相同解譯為此類型宣告值規則。 規則有一個符合的條件，並使用字串文字和符合宣告參考問題聲明。  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>執行階段作業  
請務必以了解執行階段操作宣告轉換撰寫規則有效。 執行階段作業使用宣告的三個設定：  
  
1.  **輸入宣告設定**：索賠項目，會提供給宣告轉換操作輸入的設定。  
  
2.  **工作宣告設定**：中等從讀取和寫入宣告轉換期間宣告。  
  
3.  **輸出宣告設定**：宣告轉換作業的輸出。  
  
以下是執行階段宣告轉換操作簡短的概觀：  
  
1.  宣告轉換輸入的宣告用於初始化運作宣告設定。  
  
    1.  當處理每個規則，工作宣告集適用於輸入宣告。  
  
    2.  選擇在規則條件清單符合所有可能集，從 [工作宣告集索賠項目。  
  
    3.  每個設定的符合宣告用來執行動作該規則。  
  
    4.  執行一個宣告規則動作結果，這附加至輸出宣告設定和工作主張。 因此，從規則輸出做為規則集中後續規則輸入。  
  
2.  順序從的第一個規則的處理規則集中規則。  
  
3.  移除重複宣告處理整個規則設定時，處理輸出宣告設定，以及 issues.The 結果宣告是宣告轉換程序的輸出。  
  
就可以撰寫複雜宣告轉換根據之前執行階段的行為。  
  
**範例：執行階段作業**  
  
此範例中顯示使用兩規則宣告轉換執行階段的作業。  
  
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
  
### <a name="special-rules-semantics"></a>語意特殊規則  
以下是特殊語法的規則：  
  
1.  清空規則設定 = 不輸出宣告  
  
2.  清空清單中選取條件 = 條件清單選取 [每次理賠要求相符項目  
  
    **範例：空白選取條件清單**  
  
    下列規則符合工作集中每個理賠要求。  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  清空選取相符清單 = 選取條件清單中的每個理賠要求相符項目  
  
    **範例：空白符合的條件**  
  
    下列規則符合工作集中每個理賠要求。 如果單獨使用，是基本 [允許所有」規則。  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>安全性考量  
**輸入樹系的宣告**  
  
需要完全檢查以確保我們允許或發出只正確宣告宣告出示傳入的樹系的原則。 不當宣告可能會造成損害的樹系安全性，並製作宣告輸入樹系的轉換原則時，這應該是最考量。  
  
Active Directory 具有避免宣告輸入樹系的錯誤下列功能：  
  
-   如果您信任的樹系已設定的樹系基於安全性考量，請輸入宣告不宣告轉換原則 Active Directory 卸除輸入樹系的所有主體宣告。  
  
-   設定宣告進入宣告不定義森林中的樹系結果規則執行的是，如果定義的宣告會卸除從輸出主張。  
  
**離開樹系的宣告**  
  
離開樹系的宣告顯示較少安全性考量，樹系比輸入樹系宣告。 宣告允許離開樹系-甚至時，不對應宣告轉換原則中的位置。 它也可發行宣告未定義的樹系的轉換宣告離開樹系的一部分。 這是輕鬆跨樹系信任宣告的設定。 系統管理員可以判斷若宣告輸入樹系的轉換及設定適當的原則。 例如，是否需要隱藏避免資訊洩漏理賠要求系統管理員的身分可能會設定原則。  
  
**宣告轉換規則語法錯誤**  
  
如果指定的宣告轉換原則語法不正確的規則集合，或是否有其他語法或儲存空間問題，請原則會被視為無效。 這不同預設條件先前所提及都會被視為。  
  
Active Directory 無法判斷意圖這種情形下，並且會進入防止失敗的模式，其中不輸出宣告專信任 + 周遊方向上。 若要修正這個問題被需要系統管理員操作。 如果 LDAP 來編輯宣告轉換原則，這可能會發生。 Active Directory 的 Windows PowerShell cmdlet 已驗證的地方，以防止撰寫語法問題的原則。  
  
## <a name="other-language-considerations"></a>其他語言的注意事項  
  
1.  有數字鍵或（稱為終端）這個語言特殊字元。 這會出現在[語言終端](Claims-Transformation-Rules-Language.md#BKMK_LT)稍後表格本主題。 錯誤訊息的澄清這些終端使用標記。  
  
2.  有時候可使用終端為字串文字。 不過，這類可能使用的語言定義衝突或已非預期的結果。 不建議這樣的使用量。  
  
3.  將規則動作無法執行任何類型轉換值，並且包含的此類規則動作規則將會被視為無效。 這會造成 [執行階段錯誤，並不輸出宣告的查看。  
  
4.  如果規則動作參考識別字規則的清單中選取條件部分未使用時，可能會是無效使用。 這會造成語法錯誤。  
  
    **範例：正確識別碼參考資料**  
    下列規則示範規則控制項目] 中所使用的正確 Id。  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>範例轉換規則  
  
-   **允許特定類型的所有宣告**  
  
    正確輸入  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **不允許的特定宣告類型**  
    正確輸入  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>規則剖析器的錯誤範例  
若要檢查語法錯誤自訂剖析器剖析宣告轉換規則。 此剖析器之前儲存規則 Active Directory 中相關的 Windows PowerShell cmdlet 來執行。 任何錯誤剖析規則，包括語法錯誤，會在主機上進行列印。 網域控制站也剖析器之前轉換宣告，使用規則和他們登入事件登入的錯誤（新增事件登入號碼）。  
  
本章節示範規則錯誤由剖析器正確語法與對應語法所撰寫的一些事情。  
  
1.  範例：  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    此範例中具有正確使用的分號來分號取代。   
    **錯誤訊息：**  
    *POLICY0002：無法剖析原則的資料。*  
    *行號:，使用 1 欄數字：2，錯誤預付碼:;。 這一行︰ ' c1;= > Issue(claim=c1);'。*  
    *剖析器錯誤: ' POLICY0030：語法錯誤、未預期 ';'，必須有下列其中一項: ':'。]*  
  
2.  範例：  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    在此範例中，識別碼標記複製發行聲明中的未定義。   
    **錯誤訊息**:   
    *POLICY0011：不在理賠要求規則條件符合的條件標記 CopyIssuanceStatement 中指定: 'c2'。*  
  
3.  範例：  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    「bool」車票的語言，並不是有效的值鍵入。 有效終端詳列於下列錯誤訊息。   
    **錯誤訊息：**  
    *POLICY0002：無法剖析原則的資料。*  
    行號:，使用 1 欄數：39，錯誤預付碼:「bool」。 一行︰ ' c1: [類型 =」x1」，值 =」1「值鍵入 =」bool」] = > Issue(claim=c1);'。   
    *剖析器錯誤: ' POLICY0030：語法錯誤，而非預期 '字串' 預期下列其中一個動作: 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFIER'*  
  
4.  範例：  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    數字**1**在此範例中權杖有效的語言，並不符合的條件不允許這類使用量。 其會包含在雙引號，讓它字串。   
    **錯誤訊息：**  
    *POLICY0002：無法剖析原則的資料。*  
    *行號:，使用 1 欄數：23 日錯誤預付碼：1。行: ' c1: [輸入「x1」，值 = = = 1 值鍵入 =」bool」] = > Issue(claim=c1);'。**剖析器錯誤: ' POLICY0029：意外的輸入。*  
  
5.  範例：  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    此範例中使用雙等號（= =），而不是一個單一等號（=）。   
    **錯誤訊息：**  
    *POLICY0002：無法剖析原則的資料。*  
    *行號:，使用 1 欄數：91，錯誤預付碼: =。 一行︰ ' c1: [輸入 =」x1」，值 =」1」，*  
    *值鍵入 =」布林值「] = > 問題 (type=c1.type、值 =」0」，值鍵入 =」布林值「);'。*  
    *剖析器錯誤: ' POLICY0030：語法錯誤、未預期 '='，必須要有下列其中一個動作: ='*  
  
6.  範例：  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    此範例中為語法與語意正確。 不過，使用「布林值「字串值繫結至，造成混淆，它應該避免使用。 如同之前所述，宣告值應該盡可能避免使用的語言終端。  
  
## <a name="BKMK_LT"></a>語言終端  
下表列出一組完整的終端字串和相關的語言終端宣告轉換規則語言中使用。 這些定義使用區分大小寫 UTF-16 字串。  
  
|字串|車票|  
|----------|------------|  
|"=>"|表示|  
|";"|分號|  
|":"|分號|  
|","|逗點|  
|"."|點|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|指派|  
|"&&"|與|  
|「問題]|問題|  
|[輸入]|輸入|  
|[值]|值。|  
|「值鍵入」|VALUE_TYPE|  
|「宣告」|宣告|  
|「[_A-Za-z][_A-Za-z0-9]*」|識別碼|  
|"\\"[^\\"\n]*\\""|字串|  
|「uint64」|UINT64_TYPE|  
|「int64」|INT64_TYPE|  
|「字串」|STRING_TYPE|  
|「布林」|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>語言語法  
ABNF 表單中指定下列宣告轉換規則語言。 此合約進行定義會使用此定義 ABNF 解析除了一個表格中所指定終端。 必須在 utf-16，編碼規則，必須為區分大小寫處理字串比較。  
  
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
  


