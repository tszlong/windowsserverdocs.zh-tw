---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: 宣告轉換規則語言
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a1f5c724d041a9f64c3b2697a8b5acd17a2a7bd9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445816"
---
# <a name="claims-transformation-rules-language"></a>宣告轉換規則語言

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

跨樹系宣告轉換功能可讓您將橋接器宣告動態存取控制的跨樹系界限的跨樹系信任上設定的宣告轉換原則。 所有原則的主要元件是以宣告轉換規則語言所撰寫的規則。 本主題提供有關此語言的詳細資料，並提供撰寫宣告轉換規則的相關指引。  
  
跨樹系的轉換原則的 Windows PowerShell cmdlet 會信任選項來設定簡單的原則，需要在一般案例。 這些 cmdlet 將轉譯的原則和規則，在宣告轉換規則語言中，將使用者輸入，然後將它們儲存在 Active Directory，以指定的格式。 如需有關適用於宣告轉換 cmdlet 的詳細資訊，請參閱[動態存取控制的 AD DS Cmdlet](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
根據宣告設定和放在您的 Active Directory 樹系中的跨樹系信任上的需求，您的宣告轉換原則可能是更複雜與 active Directory 的 Windows PowerShell 指令程式支援的原則目錄。 若要有效地撰寫這類原則，請務必了解宣告轉換規則語言語法和語意。 此宣告在 Active Directory 中的轉換規則語言 ("language") 是由語言的子集[Active Directory Federation Services](https://go.microsoft.com/fwlink/?LinkId=243982)類似的目的，並有非常類似的語法和語意。 不過，有較少的作業，而且其他的語法限制會放在 Active Directory 版本的語言。  
  
本主題簡要說明的語法和語意的宣告轉換規則語言，在 Active Directory 與考量，撰寫原則時進行。 它提供多組的範例規則，以協助您入門，並不正確的語法與它們產生，可協助您解讀錯誤訊息，當您編寫規則的訊息的範例。  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>用於撰寫宣告轉換原則的工具  
**適用於 Active Directory Windows PowerShell cmdlet**:這是慣用和建議的方式來撰寫及設定宣告轉換原則。 這些 cmdlet 會提供簡單的原則參數，並確認已針對更複雜的原則設定的規則。  
  
**LDAP**:宣告轉換原則可以在 Active Directory 透過輕量型目錄存取通訊協定 (LDAP) 中進行編輯。 不過，建議您不要因為原則不會有幾個複雜元件，而且您使用的工具可能不會驗證原則之前寫入 Active Directory。 接下來您可能需要相當長的時間來診斷問題。  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Active Directory 宣告轉換規則語言  
  
### <a name="syntax-overview"></a>語法概觀  
以下是語法的簡要概觀和語言的語意：  
  
-   宣告轉換規則集是由零或多個規則所組成。 每個規則有兩個使用中的部分：**選取 條件清單**並**規則動作**。 如果**選取 條件清單**評估為 TRUE，會執行對應的規則動作。  
  
-   **選取 條件清單**有零個或多個**選取條件**。 所有**選取條件**必須評估為 TRUE**選取 條件清單**評估為 TRUE。  
  
-   每個**選取條件**都有一組零或多個**比對條件**。 所有**比對條件**必須評估為 TRUE，選取 條件評估為 TRUE。 所有這些條件會評估針對單一宣告。 比對的宣告**選取條件**可以加以標記**識別項**中所參考並**規則動作**。  
  
-   每個**比對條件**指定要符合的條件**型別**或是**值**或**ValueType**的宣告，以使用不同的**條件運算子**並**字串常值**。  
  
    -   當您指定**比對條件**for**值**，您也必須指定**比對條件**特定**ValueType**和反之亦然。 在語法中，這些條件必須為彼此相鄰。  
  
    -   **ValueType**比對條件必須使用特定**ValueType**只有常值。  
  
-   A**規則動作**可以複製標記的一個宣告**識別項**或發出一個宣告，根據標記識別項的宣告和/或指定的字串常值。  
  
**範例規則**  
  
此範例會顯示可用來將轉譯的宣告型別之間兩個樹系，前提是它們使用相同的宣告 Valuetype 和有這種類型的宣告值相同的解譯的規則。 此規則有一個比對條件和問題陳述式會使用字串常值和比對的宣告參考。  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>執行階段作業  
請務必了解執行階段作業的宣告轉換，以便有效地編寫規則。 執行階段作業會使用三個宣告集：  
  
1.  **輸入宣告集**:宣告來宣告轉換作業指定輸入的集。  
  
2.  **使用宣告集**:讀取，並在轉換期間寫入的中繼宣告。  
  
3.  **輸出宣告集**:宣告轉換作業的輸出。  
  
以下是執行階段宣告轉換作業的簡短概觀：  
  
1.  宣告轉換的輸入的宣告用來初始化工作宣告集。  
  
    1.  在處理每個規則時，會將工作組宣告用於輸入宣告。  
  
    2.  在規則中的 [選取條件] 清單比對所有可能從使用中的宣告集的宣告集。  
  
    3.  每一組比對宣告用來執行該規則中的動作。  
  
    4.  在一個宣告中執行的規則動作結果，這會附加至輸出宣告集和工作的宣告集。 因此，規則的輸出用於做為輸入後續規則集中的規則。  
  
2.  規則集中規則會從第一個規則開始的循序處理。  
  
3.  輸出宣告集處理整個規則集時，會處理以移除重複的宣告和其他安全性問題。產生的宣告會宣告轉換程序的輸出。  
  
您可撰寫複雜的宣告轉換根據先前的執行階段行為。  
  
**範例：執行階段作業**  
  
這個範例示範會使用兩個規則的宣告轉換的執行階段作業。  
  
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
  
### <a name="special-rules-semantics"></a>特殊規則的語意  
以下是特殊的語法規則：  
  
1.  空的規則集 = = 沒有輸出宣告  
  
2.  選取條件 清單中的空白 = = 每個宣告比對選取的 條件 清單  
  
    **範例：選取空白的條件清單**  
  
    下列規則會比對每個宣告中的工作集。  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  空的 Select 比對清單 = = 選取的 [條件] 清單的每個宣告相符項目  
  
    **範例：空白比對條件**  
  
    下列規則會比對每個宣告中的工作集。 如果單獨使用，這是基本 「 允許 」 規則。  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>安全性考量  
**輸入樹系的宣告**  
  
提供的內送至樹系的主體的宣告必須徹底檢查，以確保我們允許，或發出正確的宣告。 不適當的宣告可能危及樹系安全性，並撰寫輸入樹系的宣告轉換原則時，這應該是主要的考量。  
  
Active Directory 有下列功能，可避免設定不正確的輸入樹系的宣告：  
  
-   如果樹系信任的宣告，輸入樹系，基於安全考量，設定任何宣告轉換原則 Active Directory 就會捨棄輸入樹系的所有主體宣告。  
  
-   如果執行樹系中未定義的宣告中，輸入樹系結果的規則集宣告，未定義的宣告會卸除從輸出宣告。  
  
**離開樹系的宣告**  
  
離開樹系的宣告會提供較小者安全性考量，樹系比輸入樹系的宣告。 宣告可以保留與樹系-即使沒有任何對應的宣告轉換原則就地是。 它也可發出一部分轉換宣告離開樹系的樹系中未定義的宣告。 這可輕鬆地設定跨樹系信任的宣告。 系統管理員可以判斷如果輸入樹系的宣告需要轉換，並設定適當的原則。 比方說，系統管理員可以設定原則，如果沒有需要隱藏的宣告，以避免資訊洩漏。  
  
**宣告轉換規則中有語法錯誤**  
  
如果指定的宣告轉換原則的語法不正確的規則集，或有其他語法或儲存體的問題，原則會被視為無效。 此處理方式不同於先前所述的預設條件。  
  
Active Directory 無法在此情況下判斷意圖，並且會進入保全的模式，該信任 + 周遊方向產生任何輸出宣告的位置。 系統管理員介入才能修正此問題。 如果 LDAP 用來編輯的宣告轉換原則，則會發生此問題。 適用於 Active Directory Windows PowerShell cmdlet 已備妥，若要避免撰寫語法問題的原則驗證。  
  
## <a name="other-language-considerations"></a>其他語言的考量  
  
1.  有數個關鍵單字或特殊 （又稱為終端機） 這個語言中的字元。 這些按照[語言終端機](Claims-Transformation-Rules-Language.md#BKMK_LT)本主題稍後的資料表。 錯誤訊息會用於這些用於去除混淆的終端機中的標記。  
  
2.  終端機有時可用來當做字串常值。 不過，這類使用量可能與語言定義相衝突，或有非預期的結果。 不建議這種使用方式。  
  
3.  規則動作無法執行任何類型轉換上宣告的值，和規則集，其中包含這類的規則動作會被視為無效。 這會導致執行階段錯誤，並會產生任何輸出宣告。  
  
4.  如果規則動作是指規則的清單中選取條件部分中未使用的識別項，則使用方式無效。 這會導致語法錯誤。  
  
    **範例：不正確的識別項參考**  
    下列規則說明規則動作中使用不正確的識別碼。  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>範例轉換規則  
  
-   **允許特定類型的所有宣告**  
  
    實際的型別  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **不允許特定的宣告類型**  
    實際的型別  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>規則的剖析器錯誤的範例  
宣告轉換規則會檢查有語法錯誤的自訂剖析器剖析。 此剖析器會儲存在 Active Directory 中的規則之前執行相關的 Windows PowerShell cmdlet。 在主控台上列印中剖析的規則，包括語法錯誤的任何錯誤。 網域控制站也執行剖析器之前使用的規則來轉換宣告，以及這些事件記錄檔中記錄錯誤 （新增事件記錄檔的數字）。  
  
本章節將說明一些規則由剖析器所產生的錯誤不正確的語法和對應的語法所撰寫的範例。  
  
1. 範例：  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   此範例中有不正確地使用的分號來取代冒號。   
   **錯誤訊息：**  
   *POLICY0002:無法剖析原則資料。*  
   *行號：1，資料行數目：2，錯誤的語彙基元:;。Line: 'c1;[]=>Issue(claim=c1);'.*  
   *剖析器錯誤：' POLICY0030:未預期的語法錯誤 ';'，必須是下列其中一項: ':'。 '*  
  
2. 範例：  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   在此範例中，複製發佈陳述式的識別碼標記未定義。   
   **錯誤訊息**:   
   *POLICY0011:宣告規則中的任何條件符合 CopyIssuanceStatement 中指定的條件標記: 'c2'。*  
  
3. 範例：  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool"不是在語言中，終端機並不是有效的 ValueType。 有效的終端機會列在下列的錯誤訊息。   
   **錯誤訊息：**  
   *POLICY0002:無法剖析原則資料。*  
   行號：1，資料行數目：39，token 時發生錯誤:"bool"。 Line: 'c1:[type=="x1", value=="1",valuetype=="bool"]=>Issue(claim=c1);'.   
   *剖析器錯誤：' POLICY0030:語法錯誤，未預期 'STRING'，必須是下列其中之一：'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFIER'*  
  
4. 範例：  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   數字**1**在此範例中不是有效的語彙基元，在語言中，並比對條件中不允許這類使用量。 它必須括在雙引號括住，以便使它的字串。   
   **錯誤訊息：**  
   *POLICY0002:無法剖析原則資料。*  
   *行號：1，資料行數目：23，token 時發生錯誤：1.Line: 'c1:[type=="x1", value==1, valuetype=="bool"]=>Issue(claim=c1);'.* <em>Parser error:' POLICY0029:非預期的輸入。</em>  
  
5. 範例：  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   此範例會使用雙等號 （= =），而不是單一等號 （=）。   
   **錯誤訊息：**  
   *POLICY0002:無法剖析原則資料。*  
   *行號：1，資料行數目：91，token 時發生錯誤: = =。Line: 'c1:[type=="x1", value=="1",*  
   *valuetype=="boolean"]=>Issue(type=c1.type, value="0", valuetype=="boolean");'.*  
   *剖析器錯誤：' POLICY0030:語法錯誤、 意外 '= ='，必須是下列其中一項: '='*  
  
6. 範例：  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   此範例中是語法和語意正確的。 不過，使用"boolean"的字串值繫結至，造成混淆，而且應該避免使用它。 如先前所述，宣告值應該盡可能避免使用語言終端機。  
  
## <a name="BKMK_LT"></a>語言終端機  
下表列出一組完整的終端機的字串和宣告轉換規則語言中使用的相關聯的語言終端機。 這些定義使用不區分大小寫的 utf-16 字串。  
  
|字串|終端機|  
|----------|------------|  
|"=>"|表示|  
|";"|分號|  
|":"|冒號|  
|","|逗號|  
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
|"&&"|和|  
|"issue"|問題|  
|"type"|TYPE|  
|"value"|值|  
|"valuetype"|VALUE_TYPE|  
|"claim"|CLAIM|  
|"[_A-Za-z][_A-Za-z0-9]*"|識別項|  
|"\\"[^\\"\n]*\\""|字串|  
|「 uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|「 字串 」|STRING_TYPE|  
|"boolean"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>語言語法  
下列的宣告轉換規則語言指定於 ABNF 表單中。 此定義會使用此處定義 ABNF 生產除了上表中所指定的終端機。 規則必須編碼 utf-16，並將字串比較必須視為為不區分大小寫。  
  
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
  


