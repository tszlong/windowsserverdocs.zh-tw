---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: "部署加密的 Office 檔案（示範步驟）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 529000c60a80ee33fc2aa7d09370d8ac1e06311c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>部署加密的 Office 檔案（示範步驟）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Contoso 財經部門有許多檔案伺服器可儲存他們的文件。 這些文件可以一般的文件或他們可能會有高商務影響 (HBI)。 例如，包含機密資訊任何文件已被視為、，以 Contoso，以影響高商務。 Contoso 想要確保他們的文件有最少的保護，而且限制適當的人員為他們 HBI 文件。 若要完成此動作，以 Contoso 瀏覽使用檔案分類基礎結構 (FCI) 以及 AD RMS，可在 Windows Server 2012 中。 使用 FCI，以 Contoso 將可根據 content，其檔案伺服器上的文件中的所有，並使用套用的適當權限原則 AD RMS。  
  
在本案例中，您將會執行下列步驟：  
  
|工作|描述|  
|--------|---------------|  
|[讓資源屬性](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|讓**影響**和**個人辨識資訊**資源屬性。|  
|[建立分類規則](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|建立下列分類規則：**HBI 分類規則**和**PII 分類規則**。|  
|[使用檔案管理工作自動保護 AD RMS 文件](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|建立自動保護高個人資訊 (PII) 文件用 AD RMS 管理工作檔案。 僅限群組成員的 FinanceAdmin 將可以存取的文件，包含高 PII。|  
|[檢視結果](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|檢查分類的文件，並觀察如何變更當您變更文件中的 content。 也驗證，AD RMS 的文件取得如何保護。|  
|[檢查 AD RMS 保護](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|檢查 AD RMS 受文件。|  
|||  
  
## <a name="BKMK_1.1"></a>步驟 1：讓資源屬性  
  
#### <a name="to-enable-resource-properties"></a>若要讓資源屬性  
  
1.  在 [HYPER-V 管理員連接到伺服器 ID_AD_DC1。 Contoso\Administrator 使用密碼登入伺服器**pass@word1**。  
  
2.  開放 Active Directory 管理中心，然後按一下**樹檢視**。  
  
3.  展開**動態存取控制**，然後選取 [**資源屬性**。  
  
4.  向下捲動**影響**中的屬性**顯示名稱**欄。 以滑鼠右鍵按一下**影響**，然後按**可讓**。  
  
5.  向下捲動**個人辨識資訊**中的屬性**顯示名稱**欄。 以滑鼠右鍵按一下**個人辨識資訊**，然後按一下 [**可讓**。  
  
6.  若要發行中的資源屬性**全球資源的清單**，在左窗格中，按一下 [**清單的資源屬性**，，然後按兩下 [**全球的資源屬性清單**。  
  
7.  按一下**新增**，然後向下捲動並按一下 [**影響**，將它新增到清單。 執行相同**個人辨識資訊**。 按一下**[確定]**兩次，才能完成。  
  
![方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>步驟 2：建立分類規則  
這個步驟將告訴您如何建立**高影響**分類規則。 此規則會搜尋 content 的文件，如果找到字串」Contoso 機密」，它會可為高商務影響這份文件。 此分類會覆寫低商務影響的任何先前指派的分類。  
  
您也會建立**高 PII**規則。 此規則搜尋 content 的文件，如果找不到社會安全，它會分類遇到高 PII 的文件。  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>若要建立的影響分類規則  
  
1.  在 [HYPER-V 管理員連接到伺服器 ID_AD_FILE1。 Contoso\Administrator 使用密碼登入伺服器**pass@word1**。  
  
2.  您需要重新整理全球 Active directory 資源屬性。 打開 Windows PowerShell 並輸入：`Update-FSRMClassificationPropertyDefinition`，然後按 ENTER 鍵。 關閉 Windows PowerShell。  
  
3.  打開檔案伺服器資源管理員。 若要打開檔案伺服器資源管理員中，按一下 [ **[開始]**，輸入**檔案伺服器資源管理員**，然後按一下 [**檔案伺服器資源管理員**。  
  
4.  在左窗格檔案伺服器資源管理員中，展開**分類管理**，然後選取 [**分類規則**。  
  
5.  在**動作**窗格中，按**設定分類排程**。 在**自動分類**索引標籤，選取**可以修正的排程**、選取**星期幾**，，然後選取 [**允許連續分類的新檔案**核取方塊。 按一下**[確定]**。  
  
6.  在**動作**窗格中，按**建立分類規則**。 這樣**建立分類規則**對話方塊。  
  
7.  在**規則名稱**方塊中，輸入**高商務影響**。  
  
8.  在**描述**方塊中，輸入**判斷是否為「Contoso 機密「字串是否有高企業影響文件 **  
  
9. 在**範圍**索引標籤上，按一下 [**設定資料夾管理屬性**，請選取**資料夾使用量**，按一下**新增**，然後按一下 [**瀏覽**，瀏覽至路徑 D:\Finance 文件，按一下 [ **[確定]**，]，然後選擇 [屬性的值，名稱為**群組檔案**，按一下 [**關閉**。 一旦管理屬性的設定，請在**規則範圍**索引標籤上選取**群組中的檔案**。  
  
10. 按一下**分類**索引標籤。  在**選擇指派給屬性檔案的方法**、**內容器**從下拉式清單。  
  
11. 在**選擇屬性指定的檔案以**，請選取**影響**從下拉式清單。  
  
12. 在**指定值**、**高**從下拉式清單。  
  
13. 按一下**設定**在**參數**。  在**分類參數**對話方塊中，在**運算式輸入**清單中，選取**字串**。 在**運算式**方塊中，輸入：**Contoso 機密**，然後按一下 [ **[確定]**。  
  
14. 按一下**評估類型**索引標籤。  按一下**重新評估現有屬性的值**，按一下 [**覆寫**的值，然後按一下 [ **[確定]**來完成。  
  
![方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>若要建立高-PII 分類規則  
  
1.  在 [HYPER-V 管理員連接到伺服器 ID_AD_FILE1。 Contoso\Administrator 使用密碼登入伺服器**pass@word1**。  
  
2.  在桌面上，打開資料夾名為**規則運算式**，然後打開名為文字文件和**RegEx-SSN**。 反白，然後將下列運算式字串複製：**^（！000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-] 嗎？)(?!00) \d\d\3（？！\d {4}$ 0000)**。 在稍後將會使用此字串此步驟，讓它在您的剪貼簿。  
  
3.  打開檔案伺服器資源管理員。 若要打開檔案伺服器資源管理員中，按一下 [ **[開始]**，輸入**檔案伺服器資源管理員**，然後按一下 [**檔案伺服器資源管理員**。  
  
4.  在左窗格檔案伺服器資源管理員中，展開**分類管理**，然後選取 [**分類規則**。  
  
5.  在**動作**窗格中，按**設定分類排程**。 在**自動分類**索引標籤，選取**可以修正的排程**、選取**星期幾**，，然後選取 [**允許連續分類的新檔案**核取方塊。 按一下 \ [確定 \]。  
  
6.  在**規則名稱**方塊中，輸入**高 PII**。 在**描述**方塊中，輸入**則的文件高決定 PII 根據卡的身分證號碼。**  
  
7.  按一下**範圍**索引標籤，選取**群組中的檔案**核取方塊。  
  
8.  按一下**分類**索引標籤。  在**選擇指派給屬性檔案的方法**、**內容器**從下拉式清單。  
  
9. 在**選擇屬性指定的檔案以**，請選取**個人辨識資訊**從下拉式清單。  
  
10. 在**指定值**、**高**從下拉式清單。  
  
11. 按一下**設定**在**參數**。   
    在**分類參數**視窗中，請在**運算式輸入**清單中，選取**運算式**。 在**運算式**方塊中，文字從您的剪貼簿貼上：**^（！000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-] 嗎？)(?!00) \d\d\3（？！\d {4}$ 0000)**，然後按一下 [ **[確定]**。  
  
    > [!NOTE]  
    > 這個運算式可讓無效的身分證安全性數字。 這可讓我們在展示使用虛構身分證號碼。  
  
12. 按一下**評估類型**索引標籤。  選取 [**重新評估現有屬性的值**，**覆寫**的值，然後按一下 [ **[確定]**來完成。  
  
![方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
您現在應該會有兩個分類的規則：  
  
-   高企業影響  
  
-   高 PII  
  
## <a name="BKMK_3"></a>步驟 3：使用檔案管理工作自動保護 AD RMS 文件  
既然您已經建立規則自動分類 content 為基礎的文件下, 一步就是建立自動保護特定根據其分類的文件使用 AD RMS 管理工作檔案。 在此步驟，您會自動保護高 PII 任何文件將檔案管理工作建立。 僅限群組成員的 FinanceAdmin 將可以存取的文件，包含高 PII。  
  
#### <a name="to-protect-documents-with-ad-rms"></a>保護 AD RMS 文件  
  
1.  在 [HYPER-V 管理員連接到伺服器 ID_AD_FILE1。 Contoso\Administrator 使用密碼登入伺服器**pass@word1**。  
  
2.  打開檔案伺服器資源管理員。 若要打開檔案伺服器資源管理員中，按一下 [ **[開始]**，輸入**檔案伺服器資源管理員**，然後按一下 [**檔案伺服器資源管理員**。  
  
3.  在左窗格中，選取 [**檔案管理工作**。 在**動作**窗格中，選取**建立檔案管理工作**。  
  
4.  在**任務名稱：**欄位中，輸入**高 PII**。 在**描述**欄位中，輸入**自動 RMS 保護高 PII 文件中的**。  
  
5.  按一下**範圍**索引標籤，選取**群組中的檔案**核取方塊。  
  
6.  按一下**動作**索引標籤。 在**輸入**、**RMS 加密**。 按一下**瀏覽]**來選取範本，然後選取**以 Contoso 財經管理員只**範本。  
  
7.  按一下**條件**索引標籤，然後按一下 [**新增]**。 在**屬性**、**個人辨識資訊**。 在**電信業者**、**等**。 在**值**、**高**。 按一下**[確定]**。  
  
8.  按一下**排程**索引標籤。 在**排程**區段中，按一下 [**週**，然後選取**星期日**。 執行此工作一次為星期將可確保您的捕捉可能因為服務中斷或其他受到干擾的事件遺漏任何文件。  
  
9. 在**繼續操作**區段中，選取**執行工作持續在新的檔案**，然後按一下 [ **[確定]**。 您現在應該會有名為高 PII 檔案管理工作。  
  
![方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>步驟 4：檢視結果  
它是在重要訊息中看看您的新自動分類和 AD RMS 保護規則。 在此步驟，您將檢查分類的文件，並觀察如何變更當您變更文件中的 content。  
  
#### <a name="to-view-the-results"></a>若要檢視結果  
  
1.  在 [HYPER-V 管理員連接到伺服器 ID_AD_FILE1。 Contoso\Administrator 使用密碼登入伺服器**pass@word1**。  
  
2.  在 [Windows 檔案總管] 瀏覽至 D:\Finance 文件。  
  
3.  以滑鼠右鍵按一下財經備文件，然後按一下**屬性**。按一下**分類**索引標籤，然後通知影響屬性，目前有不值。 按一下**取消**。  
  
4.  以滑鼠右鍵按一下**申請核准雇用文件以**，然後選取 [**屬性**。  
  
5.  按一下**分類**索引標籤，並注意，**的個人辨識資訊屬性**目前有不值。 按一下**取消**。  
  
6.  切換至 CLIENT1。 關閉任何登入的使用者登入並再登入為 Contoso\MReid 密碼**pass@word1**。  
  
7.  從桌面，請打開**財經文件**共用的資料夾。  
  
8.  開放**財經備**的文件。 靠近底部的 [文件，您將會看到該文字**機密**。 修改朗讀：**以 Contoso 機密**。 將文件，並將它關閉。  
  
9. 開放**申請核准雇用以**的文件。 在**證 #:**區段中，輸入：777-77-7777。 將文件，並將它關閉。  
  
    > [!NOTE]  
    > 您可能需要等待分類發生 30 秒。  
  
10. 切換回 ID_AD_FILE1。 在 [Windows 檔案總管] 瀏覽至 D:\Finance 文件。  
  
11. 財經備文件，以滑鼠右鍵按一下，按**屬性**。 按一下**分類**索引標籤。 請注意，**影響**屬性現在已設定為**高**。 按一下**取消**。  
  
12. 以雇用文件，並按一下滑鼠右鍵按一下 \ [核准要求**屬性**。  
  
13. . 按一下**分類**索引標籤。 請注意，**個人辨識資訊**屬性現在已設定為**高**。 按一下**取消**。  
  
## <a name="BKMK_5"></a>步驟 5：檢查 AD RMS 的保護  
  
#### <a name="to-verify-that-the-document-is-protected"></a>若要確認受到文件  
  
1.  切換回 ID_AD_CLIENT1。  
  
2.  開放**申請核准雇用以**的文件。  
  
3.  按一下**[確定]**允許連接到您的 AD RMS 伺服器的文件。  
  
4.  您現在可以查看的文件已受到 AD RMS 因為它包含社會安全。  
  

