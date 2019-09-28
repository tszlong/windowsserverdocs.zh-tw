---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: Deploy Automatic File Classification (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7b8d613653bc2effdae155d34a1a94a820bae3aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357595"
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Deploy Automatic File Classification (Demonstration Steps)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這個主題說明如何啟用 Active Directory 中的資源內容、在檔案伺服器上建立分類規則，然後將值指派給檔案伺服器上檔案的資源內容。 針對這個範例會建立下列分類規則：  
  
-   一種內容分類規則，可搜尋一組檔案中是否有字串「Contoso 機密」。 如果檔案中找到這個字串，檔案上的 [影響] 資源內容就會設為 [高]。  
  
-   利用規則運算式搜尋一組檔案，尋找一個檔案中至少有 10 次比對到身分證號碼的內容分類規則。 如果找到這個模式，則檔案會分類為包含個人識別資訊，且 [個人識別資訊] 資源內容會設為 [高]。  
  
**本檔中的**  
  
-   [步驟 1：建立資源屬性定義 @ no__t-0  
  
-   [步驟 2：建立字串內容分類規則 @ no__t-0  
  
-   [步驟 3：建立正則運算式內容分類規則 @ no__t-0  
  
-   [步驟 4：確認檔案已分類 @ no__t-0  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>步驟1：建立資源內容定義  
[影響] 和 [個人識別資訊] 資源內容已啟用，所以檔案分類基礎結構可以使用這些資源內容來標記網路共用資料夾上掃描到的檔案。  
  
[使用 Windows PowerShell 執行此步驟](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>建立資源內容定義  
  
1.  在網域控制站上，以 Domain Admins 安全性群組成員的身分登入伺服器。  
  
2.  開啟「Active Directory 管理中心」。 在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [Active Directory 管理中心]。  
  
3.  展開 [動態存取控制]，然後按一下 [資源內容]。  
  
4.  用滑鼠右鍵按一下 [影響]，然後按一下 [啟用]。  
  
5.  在 [個人識別資訊] 上按一下滑鼠右鍵，然後按一下 [啟用]。  
  
@no__t 0solution 指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>步驟2：建立字串內容分類規則  
字串內容分類規則會掃描檔案是否包含特定字串。 如果找到字串，則可以設定資源內容的值。 在此範例中，我們會掃描網路共用資料夾上的每個檔案，並尋找「Contoso 機密」字串。 如果找到字串，關聯的檔案會被分類為具有高商業影響。  
  
[使用 Windows PowerShell 執行此步驟](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>建立字串內容分類規則  
  
1.  以本機 Administrators 安全性群組成員的身分登入檔案伺服器。  
  
2.  在 Windows PowerShell 命令提示字元中，輸入 **Update-FsrmClassificationPropertyDefinition** ，然後按下 ENTER。 這會將網域控制站上建立的內容定義與檔案伺服器同步。  
  
3.  開啟檔案伺服器資源管理員。 在 [伺服器管理員] 中按一下 [工具]，然後按一下 [檔案伺服器資源管理員]。  
  
4.  展開 [分類管理]，以滑鼠右鍵按一下 [分類規則]，然後按一下 [設定分類排程]。  
  
5.  選取 [啟用固定的排程] 核取方塊，選取 [允許對新檔案進行連續分類] 核取方塊，選擇每週中執行分類的日子，然後按一下 [確定]。  
  
6.  在 [分類規則]上按一下滑鼠右鍵，然後按一下 [建立分類規則]。  
  
7.  在 [一般] 索引標籤的 [規則名稱] 方塊中，輸入規則的名稱，例如 **Contoso Confidential**。  
  
8.  在 [範圍] 索引標籤上，按一下 [新增]，選擇這個規則中應該包含的資料夾，例如 D:\Finance Documents。  
  
    > [!NOTE]  
    > 您也可以為範圍選擇動態命名空間。 如需分類規則的動態命名空間的詳細資訊，請參閱[Windows server 2012 中檔案伺服器 Resource Manager 的新功能 \[redirected @ no__t-2](assetId:///d53c603e-6217-4b98-8508-e8e492d16083)。  
  
9. 在 [分類] 索引標籤中，設定下列選項：  
  
    -   在 [選擇將內容指派給檔案的方法] 方塊中，確定已選取 [內容分類器]。  
  
    -   在 [選擇要指派給檔案的內容] 方塊中，按一下 [影響]。  
  
    -   在 [指定值] 方塊中，按一下 [高]。  
  
10. 在 [參數] 標題之下，按一下 [設定]。  
  
11. 在 [運算式類型] 欄中，選取 [字串]。  
  
12. 在 [運算式] 欄中，輸入 **Contoso Confidential**，然後按一下 [確定]。  
  
13. 在 [評估類型] 索引標籤上，選取 [重新評估現有的內容值] 核取方塊，按一下 [覆寫現有的值]，然後按一下 [確定]。  
  
@no__t 0solution 指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>步驟3：建立規則運算式內容分類規則  
規則運算式分類規則會掃描檔案，尋找符合規則運算式的模式。 如果找到符合規則運算式的字串，則可以設定資源內容的值。 在這個範例中，我們會掃描網路共用資料夾上的每個檔案，尋找符合身份證號碼 (XXX-XX-XXXX) 模式的字串。 如果找到這種模式，關聯的檔案會被分類為包含個人識別資訊。  
  
[使用 Windows PowerShell 執行此步驟](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>建立規則運算式內容分類規則  
  
1.  以本機 Administrators 安全性群組成員的身分登入檔案伺服器。  
  
2.  在 Windows PowerShell 命令提示字元中，輸入 **Update-FsrmClassificationPropertyDefinition**，然後按下 ENTER。 這會將網域控制站上建立的內容定義與檔案伺服器同步。  
  
3.  開啟檔案伺服器資源管理員。 在 [伺服器管理員] 中按一下 [工具]，然後按一下 [檔案伺服器資源管理員]。  
  
4.  在 [分類規則]上按一下滑鼠右鍵，然後按一下 [建立分類規則]。  
  
5.  在 [一般] 索引標籤上的 [規則名稱] 方塊中，輸入分類規則的名稱，例如「PII 規則」。  
  
6.  在 [範圍] 索引標籤上，按一下 [新增]，選擇這個規則中應該包含的資料夾，例如 D:\Finance Documents。  
  
7.  在 [分類] 索引標籤中，設定下列選項：  
  
    -   在 [選擇將內容指派給檔案的方法] 方塊中，確定已選取 [內容分類器]。  
  
    -   在 [選擇要指派給檔案的內容] 方塊中，按一下 [個人識別資訊]。  
  
    -   在 [指定值] 方塊中，按一下 [高]。  
  
8.  在 [參數] 標題之下，按一下 [設定]。  
  
9. 在 [運算式類型] 欄中，選取 [規則運算式]。  
  
10. 在 [**運算式**] 資料行中，輸入 **^ （？！000）（[0-7] \d @ no__t-2 | 7 （[0-7] \d | 7 [012]））（[-]？）(?!00） \d\d\3 （？！0000） \d @ no__t-3 $**  
  
11. 在 [發生次數下限] 欄中，輸入 **10**，然後按一下 [確定]。  
  
12. 在 [評估類型] 索引標籤上，選取 [重新評估現有的內容值] 核取方塊，按一下 [覆寫現有的值]，然後按一下 [確定]。  
  
@no__t 0solution 指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>步驟4：確認檔案正確分類  
您可以檢視分類規則中指定的資料夾中建立的檔案的內容，確認檔案已正確分類。  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>確認檔案正確分類  
  
1.  在檔案伺服器上，使用 [檔案伺服器資源管理員] 執行分類規則。  
  
    1.  按一下 [分類管理]，以滑鼠右鍵按一下 [分類規則]，然後按一下 [立即使用所有規則執行分類]。  
  
    2.  按一下 [等待分類完成] 選項，然後再按一下 [確定]。  
  
    3.  關閉自動分類報告。  
  
    4.  您可以使用 Windows PowerShell 的下列命令執行這個作業：**開始-Start-fsrmclassification ' "RunDuration 0-Confirm： $false**  
  
2.  瀏覽至分類規則中指定的資料夾，例如 D:\Finance Documents。  
  
3.  以滑鼠右鍵按一下該資料夾中的檔案，然後按一下 [內容]。  
  
4.  按一下 [分類] 索引標籤，並確認已正確分類檔案。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [案例：使用分類深入了解您的資料](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [自動檔案分類的規劃](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/jj574209(v%3dws.11))  

  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

