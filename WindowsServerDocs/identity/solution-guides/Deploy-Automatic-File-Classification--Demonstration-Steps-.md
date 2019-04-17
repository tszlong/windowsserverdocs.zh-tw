---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: "部署自動檔案分類（示範步驟）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c5c0fa221e0d7375216426f838ba37bee852984
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>部署自動檔案分類（示範步驟）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題如何讓資源屬性 Active Directory 中的，分類規則的伺服器上建立的檔案，然後指定至該檔案伺服器上的檔案的資源屬性的值。 針對此範例中，會建立下列分類規則：  
  
-   搜尋檔案 'Contoso 機密。' 字串一組內容分類規則 如果檔案中找到字串，影響資源屬性設定為 [高檔案。  
  
-   搜尋檔案運算式符合社會安全至少 10 倍一個檔案中的一組內容分類規則。 如果找到模式時，檔案被歸類為個人資訊，以及個人辨識資訊資源屬性設為 [高。  
  
**本文件**  
  
-   [步驟 1：建立的資源屬性定義](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [步驟 2：建立字串內容分類規則](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [步驟 3：建立運算式內容分類規則](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [步驟 4：確認屬於檔案](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> 本主題包含範例 Windows PowerShell cmdlet 可供您將部分所述的程序。 如需詳細資訊，請查看[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Step1"></a>步驟 1：建立的資源屬性定義  
影響和個人辨識資訊的資源屬性的功能，讓該檔案分類基礎結構可以使用這些的資源屬性標記網路共用資料夾中的掃描的檔案。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>若要建立的資源屬性定義  
  
1.  網域控制站在登入伺服器網域管理安全性群組成員。  
  
2.  打開 Active Directory 系統管理員中心。 在伺服器管理員中，按一下**工具**，然後按**Active Directory 管理中心**。  
  
3.  展開**動態存取控制**，然後按**資源屬性**。  
  
4.  以滑鼠右鍵按一下**影響**，然後按**可讓**。  
  
5.  以滑鼠右鍵按一下**個人辨識資訊**，然後按一下 [**可讓**。  
  
![方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>步驟 2：建立字串內容分類規則  
字串內容分類規則掃描字串特定的檔案。 如果找到字串，您可以設定的資源屬性的值。 在此範例中，我們將會每個檔案，在網路共用資料夾，並尋找 'Contoso 機密。' 字串 如果找到字串，相關聯的檔案歸類為有高企業的影響。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>若要建立字串內容分類規則  
  
1.  以系統管理員安全性群組成員登入該檔案伺服器。  
  
2.  Windows PowerShell 命令提示字元中，輸入**更新-FsrmClassificationPropertyDefinition**，然後按 ENTER 鍵。 這將會同步處理檔案伺服器的網域控制站上建立的屬性定義。  
  
3.  打開檔案伺服器資源管理員。 在伺服器管理員中，按一下**工具**，然後按**檔案伺服器資源管理員**。  
  
4.  展開**分類管理**，以滑鼠右鍵按一下**分類規則**，然後按一下 [**設定分類排程**。  
  
5.  選取 [**讓修正的排程**核取方塊中，選取**允許連續分類的新檔案**核取方塊中，選擇星期幾以執行分類，然後按**[確定]**。  
  
6.  以滑鼠右鍵按一下**分類規則**，然後按**建立分類規則**。  
  
7.  在**一般**索引標籤的**規則名稱**方塊中，輸入規則的名稱，例如**以 Contoso 機密**。  
  
8.  在**範圍**索引標籤上，按一下 [**新增**，並選擇應包含在此規則，例如 D:\Finance 文件中的資料夾。  
  
    > [!NOTE]  
    > 您也可以選擇領域動態命名空間。 如需有關分類規則的動態命名空間，請查看[新檔案伺服器資源管理員」中是在 Windows Server 2012 \[redirected\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083)。  
  
9. 在**分類**索引標籤上，進行下列設定：  
  
    -   在**選擇指派屬性檔案的方法**方塊中，請確定**內容器**選取。  
  
    -   在**選擇屬性指定的檔案以**方塊中，按一下 [**影響**。  
  
    -   在**指定值**方塊中，按**高**。  
  
10. 在**參數**標頭下，按一下 [**設定**。  
  
11. 在**輸入運算式**欄中，選取**字串**。  
  
12. 在**運算式**欄中，輸入**Contoso 機密**，然後按一下 [ **[確定]**。  
  
13. 在**評估類型**索引標籤，選取**重新評估現有屬性的值**核取方塊、按一下 [**覆寫現有的值**，然後按一下 [ **[確定]**。  
  
![方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>步驟 3：建立運算式內容分類規則  
運算式分類規則掃描模式符合運算式檔案。 如果找到符合運算式字串，您可以設定的資源屬性的值。 在此範例中，我們將會每個檔案，在網路共用資料夾，並尋找字串符合社交安全性數字 (XXX-XX-XXXX) 的模式。 如果找不到此模式，相關聯的檔案會被歸類為個人資訊。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>若要建立運算式內容分類規則  
  
1.  以系統管理員安全性群組成員登入該檔案伺服器。  
  
2.  Windows PowerShell 命令提示字元中，輸入**更新-FsrmClassificationPropertyDefinition**，然後按 ENTER 鍵。 這將會同步處理檔案伺服器的網域控制站的屬性定義。  
  
3.  打開檔案伺服器資源管理員。 在伺服器管理員中，按一下**工具**，然後按**檔案伺服器資源管理員**。  
  
4.  以滑鼠右鍵按一下**分類規則**，然後按**建立分類規則**。  
  
5.  在**一般**索引標籤的**規則名稱**方塊中，輸入名稱分類規則，例如 PII 規則。  
  
6.  在**範圍**索引標籤上，按一下 [**新增**，然後選擇 [的資料夾，應該會包含在此規則，例如 D:\Finance 文件。  
  
7.  在**分類**索引標籤上，進行下列設定：  
  
    -   在**選擇指派屬性檔案的方法**方塊中，請確定**內容器**選取。  
  
    -   在**選擇屬性指定的檔案以**方塊中，按一下 [**個人辨識資訊**。  
  
    -   在**指定值**方塊中，按**高**。  
  
8.  在**參數**標頭下，按一下 [**設定**。  
  
9. 在**輸入運算式**欄中，選取**運算式**。  
  
10. 在**運算式**欄中，輸入**^（！000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-] 嗎？)(?!00) \d\d\3（？！\d {4} $ 0000)**  
  
11. 在**最小的項目**欄中，輸入**10**，然後按一下 [ **[確定]**。  
  
12. 在**評估類型**索引標籤，選取**重新評估現有屬性的值**核取方塊、按一下 [**覆寫現有的值**，然後按一下 [ **[確定]**。  
  
![方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>步驟 4：驗證，正確地歸類檔案  
您可以檢查檔案，正確歸類檢視檔案建立資料夾分類規則中所指定的屬性。  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>若要確認檔案都正確分類  
  
1.  使用檔案伺服器資源管理員執行分類規則的檔案伺服器。  
  
    1.  按一下**分類管理**，以滑鼠右鍵按一下**分類規則**，然後按一下 [**分類的所有規則立即執行**。  
  
    2.  按一下**等待完成分類**選項，然後按一下 [ **[確定]**。  
  
    3.  關閉自動分類報告。  
  
    4.  您可以使用 Windows PowerShell 使用下列命令：**開始-FSRMClassification '」RunDuration 0-確認：$false**  
  
2.  瀏覽至分類規則，例如 D:\Finance 文件中所指定的資料夾。  
  
3.  以滑鼠右鍵按一下該資料夾的檔案，然後按一下**屬性**。  
  
4.  按一下**分類**索引標籤，然後確認檔案已正確歸類。  
  
## <a name="BKMK_Links"></a>也了  
  
-   [案例︰ 使用分類取得深入了解您的資料](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [自動檔案分類計劃](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

