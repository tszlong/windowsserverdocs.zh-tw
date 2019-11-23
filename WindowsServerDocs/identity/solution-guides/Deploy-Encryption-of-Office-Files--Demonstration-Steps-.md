---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: Deploy Encryption of Office Files (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 05da1b7df2e3242c9b68bd7858c824f91e81a563
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407109"
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Deploy Encryption of Office Files (Demonstration Steps)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Contoso 的財務部門有許多儲存其檔的檔案伺服器。 這些文件可以是一般的文件或具有高商業影響 (HBI) 的文件。 例如，Contoso 將包含機密資訊的任何文件視為具有高商業影響。 Contoso 想確定他們的文件有最基本的保護，且其 HBI 文件只限制給適當的人員使用。 為了達成此目的，Contoso 正在使用 Windows Server 2012 中提供的檔案分類基礎結構（FCI）和 AD RMS 來進行探索。 藉著使用 FCI，Contoso 將根據文件內容來分類檔案伺服器上所有的文件，然後使用 AD RMS 套用適當的權限原則。  
  
在此案例中，您將執行下列步驟：  
  
|工作|描述|  
|--------|---------------|  
|[啟用資源屬性](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|啟用 [影響] 和 [個人識別資訊] 資源內容。|  
|[建立分類規則](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|建立下列分類規則：[HBI 分類規則] 和 [PII 分類規則]。|  
|[使用檔案管理工作自動保護具有 AD RMS 的檔](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|建立一個檔案管理工作，它會自動使用 AD RMS 來保護具有高個人識別資訊 (PII) 的文件。 只有 FinanceAdmin 群組的成員可以存取包含高 PII 的文件。|  
|[查看結果](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|檢查文件的分類，觀察當您變更文件中的內容時，文件如何隨之變更。 同時確認 AD RMS 保護文件的方式。|  
|[確認 AD RMS 保護](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|確認文件受到 AD RMS 的保護。|  
|||  
  
## <a name="BKMK_1.1"></a>步驟1：啟用資源屬性  
  
#### <a name="to-enable-resource-properties"></a>啟用資源內容  
  
1. 在 [Hyper-V 管理員] 中，連線到伺服器 ID_AD_DC1。 使用 Contoso\Administrator 與密碼<strong>pass@word1</strong>來登入伺服器。  
  
2. 開啟 Active Directory 管理中心，然後按一下 [樹狀檢視]。  
  
3. 展開 [動態存取控制]，然後選取 [資源內容]。  
  
4. 向下捲動到 [顯示名稱] 欄中的 [影響] 內容。 用滑鼠右鍵按一下 [影響]，然後按一下 [啟用]。  
  
5. 向下捲動到 [顯示名稱] 欄中的 [個人識別資訊] 內容。 在 [個人識別資訊] 上按一下滑鼠右鍵，然後按一下 [啟用]。  
  
6. 若要將資源內容發佈到 [全域資源清單]，請在左窗格中按一下 [資源內容清單]，再按兩下 [全域資源內容清單]。  
  
7. 按一下 [新增]，然後向下捲動到 [影響] 並按一下以新增至清單。 為 [個人識別資訊]執行同樣的動作。 按兩次 [確定] 以完成。  
  
![解決方案引導](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>步驟2：建立分類規則  
此步驟說明如何建立 [高影響] 分類規則。 此規則會搜尋檔的內容，如果找到字串「Contoso 機密」，它會將此檔分類為具有高商業影響。 這種分類會覆寫之前指派的低商業影響分類。  
  
您也會建立 [高 PII] 規則。 這個規則會搜尋文件的內容，如果找到身分證號碼，它會將此文件分類為具有高 PII。  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>建立高影響分類規則  
  
1. 在 [Hyper-V 管理員] 中，連線到伺服器 ID_AD_FILE1。 使用 Contoso\Administrator 與密碼<strong>pass@word1</strong>來登入伺服器。  
  
2. 您需要重新整理 Active Directory 的全域資源內容。 開啟 Windows PowerShell 並輸入： `Update-FSRMClassificationPropertyDefinition`，然後按 ENTER 鍵。 關閉 Windows PowerShell。  
  
3. 開啟檔案伺服器資源管理員。 若要開啟檔案伺服器資源管理員，按一下 [開始]，輸入 **檔案伺服器資源管理員**，然後按一下 [檔案伺服器資源管理員]。  
  
4. 在 [檔案伺服器資源管理員] 的左窗格中，展開 [分類管理]，然後選取 [分類規則]。  
  
5. 在 [動作] 窗格中，按一下 [設定分類排程]。 在 [自動分類] 索引標籤上，選取 [啟用固定的排程]，選取 [星期幾]，然後選取 [允許對新檔案進行連續分類] 核取方塊。 按一下 **\[確定\]** 。  
  
6. 在 [動作] 窗格中，按一下 [建立分類規則]。 如此會開啟 [建立分類規則] 對話方塊。  
  
7. 在 [規則名稱] 方塊中，輸入**高商業影響**。  
  
8. 在 [**描述**] 方塊中，輸入**根據字串「Contoso 機密」是否存在，判斷檔是否具有高商業影響**。  
  
9. 在 [範圍] 索引標籤上，按一下 [設定資料夾管理內容]，選取 [資料夾使用方式]，按一下 [新增]，然後按一下 [瀏覽]，瀏覽至 D:\Finance Documents 作為路徑，按一下 [確定]，然後選擇名稱為 [群組檔案] 的內容值，再按一下 [關閉]。 設定好管理內容之後，在 [規則範圍] 索引標籤上選取 [群組檔案]。  
  
10. 按一下 [**分類**] 索引標籤。 在 **[選擇要將屬性指派**給檔案的方法] 下，從下拉式清單中選取 [**內容分類器**]。  
  
11. 在 [選擇要指派給檔案的內容] 下，從下拉式清單選取 [影響]。  
  
12. 在 [指定值]下，從下拉式清單選取 [高] 。  
  
13. 按一下 [參數] 下的 [設定]。  在 [分類參數] 對話方塊中，從 [運算式類型] 清單中選取 [字串]。 在 [運算式] 方塊中輸入： **Contoso 公司機密**，然後按一下 [確定]。  
  
14. 按一下 [**評估類型**] 索引標籤。 按一下 [**重新評估現有的屬性值**]，按一下 [**覆寫**現有的值]，然後按一下 **[確定**] 完成。  
  
![解決方案引導](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>建立高 PII 分類規則  
  
1. 在 [Hyper-V 管理員] 中，連線到伺服器 ID_AD_FILE1。 使用 Contoso\Administrator 與密碼<strong>pass@word1</strong>來登入伺服器。  
  
2. 在桌面上，開啟名為 [規則運算式] 的資料夾，然後開啟名為 [RegEx SSN] 的文件。 反白顯示並複製下列正則運算式字串： **^ （？！000）（[0-7] \d{2}| 7 （[0-7] \d | 7 [012]））（[-]？）(?!00） \d\d\3 （？！0000） \d{4}$** 。 此步驟中稍後會用到此字串，所以將它保留在剪貼簿上。  
  
3. 開啟檔案伺服器資源管理員。 若要開啟檔案伺服器資源管理員，按一下 [開始]，輸入 **檔案伺服器資源管理員**，然後按一下 [檔案伺服器資源管理員]。  
  
4. 在 [檔案伺服器資源管理員] 的左窗格中，展開 [分類管理]，然後選取 [分類規則]。  
  
5. 在 [動作] 窗格中，按一下 [設定分類排程]。 在 [自動分類] 索引標籤上，選取 [啟用固定的排程]，選取 [星期幾]，然後選取 [允許對新檔案進行連續分類] 核取方塊。 按一下 [確定]。  
  
6. 在 [規則名稱] 方塊中，輸入 **高 PII**。 在 [描述] 方塊中，輸入 **根據是否出現身分證號碼判斷文件是否具有高 PII**。  
  
7. 按一下 [範圍] 索引標籤，選取 [群組檔案] 核取方塊。  
  
8. 按一下 [**分類**] 索引標籤。 在 **[選擇要將屬性指派**給檔案的方法] 下，從下拉式清單中選取 [**內容分類器**]。  
  
9. 在 [選擇要指派給檔案的內容] 下，從下拉式清單選取 [個人識別資訊]。  
  
10. 在 [指定值]下，從下拉式清單選取 [高] 。  
  
11. 按一下 [參數] 下的 [設定]。   
    在 [**分類參數**] 視窗的 [**運算式類型**] 清單中，選取 [**正則運算式**]。 在 [**運算式**] 方塊中，貼上剪貼簿中的文字： **^ （？！000）（[0-7] \d{2}| 7 （[0-7] \d | 7 [012]））（[-]？）(?!00） \d\d\3 （？！0000） \d{4}$** ，然後按一下 **[確定]** 。  
  
    > [!NOTE]  
    > 這個運算式會允許無效身分證號碼。 這可讓我們在示範中使用虛構的身分證號碼。  
  
12. 按一下 [**評估類型**] 索引標籤。 選取 [**重新評估現有的屬性值**]，**覆寫**現有的值，然後按一下 **[確定**] 以完成。  
  
![解決方案引導](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
現在，您應該有兩個分類規則：  
  
-   高商業影響  
  
-   高 PII  
  
## <a name="BKMK_3"></a>步驟3：使用檔案管理工作自動使用 AD RMS 保護檔  
既然您已建立自動根據內容分類檔的規則，下一步是建立檔案管理工作，使用 AD RMS 自動根據其分類來保護特定檔。 在此步驟中，您將建立自動保護具有高 PII 的任何文件的檔案管理工作。 只有 FinanceAdmin 群組的成員可以存取包含高 PII 的文件。  
  
#### <a name="to-protect-documents-with-ad-rms"></a>使用 AD RMS 保護文件  
  
1. 在 [Hyper-V 管理員] 中，連線到伺服器 ID_AD_FILE1。 使用 Contoso\Administrator 與密碼<strong>pass@word1</strong>來登入伺服器。  
  
2. 開啟檔案伺服器資源管理員。 若要開啟檔案伺服器資源管理員，按一下 [開始]，輸入 **檔案伺服器資源管理員**，然後按一下 [檔案伺服器資源管理員]。  
  
3. 在左窗格中，選取 [檔案管理工作]。 在 [動作] 窗格中，選取 [建立檔案管理工作]。  
  
4. 在 [工作名稱:] 欄位中，輸入 **高 PII**。 在 [描述] 欄位中，輸入**高 PII 文件的自動 RMS 保護**。  
  
5. 按一下 [範圍] 索引標籤，選取 [群組檔案] 核取方塊。  
  
6. 按一下 [**動作**] 索引標籤。在 [**類型**] 底下，選取 [ **RMS 加密**]。 按一下 [瀏覽] 以選取範本，然後選取 [只限 Contoso 財務管理人員] 範本。  
  
7. 按一下 [條件] 索引標籤，然後按一下 [新增]。 在 [內容] 下，選取 [個人識別資訊]。 在 [運算子] 下，選取 [等於]。 在 [值] 下，選取 [高]。 按一下 **\[確定\]** 。  
  
8. 按一下 [**排程**] 索引標籤。在 [**排程**] 區段中，按一下 [**每週**]，然後選取 [**星期日**]。 每週執行一次工作可確保您找到任何可能因服務中斷或其他破壞性事件遺漏的文件。  
  
9. 在 [連續操作] 區段中，選取 [持續在新檔案上執行工作]，然後按一下 [確定]。 現在您應該有名為高 PII 的檔案管理工作。  
  
![解決方案引導](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>步驟4：查看結果  
現在就來看看您新的自動分類和 AD RMS 保護規則的實際運作。 在這個步驟中，您會檢查文件的分類，觀察當您變更文件中的內容時，文件如何隨之變更。  
  
#### <a name="to-view-the-results"></a>檢視結果  
  
1. 在 [Hyper-V 管理員] 中，連線到伺服器 ID_AD_FILE1。 使用 Contoso\Administrator 與密碼<strong>pass@word1</strong>來登入伺服器。  
  
2. 在 [Windows 檔案總管] 中瀏覽至 D:\Finance Documents。  
  
3. 以滑鼠右鍵按一下 [財務備忘] 文件，按一下 [內容]。按一下 [分類] 索引標籤，請注意 [影響] 內容目前沒有任何值。 按一下 [取消]。  
  
4. 以滑鼠右鍵按一下 [雇用核准要求文件]，然後選取 [內容]。  
  
5. 按一下 [分類] 索引標籤，請注意 [個人識別資訊內容] 目前沒有任何值。 按一下 [取消]。  
  
6. 切換至 CLIENT1。 登出已登入的任何使用者，然後使用<strong>pass@word1</strong>的密碼登入為使用 contoso\mreid 身分。  
  
7. 從桌面開啟 [財務文件] 共用資料夾。  
  
8. 開啟 [財務備忘] 文件。 靠近文件的底部，您會看到 **機密**兩個字。 將它修改為： **Contoso 公司機密**。 儲存文件，並將它關閉。  
  
9. 開啟 [雇用核准要求]文件。 在 [身分證號碼:] 區段中，輸入：777-77-7777。 儲存文件，並將它關閉。  
  
    > [!NOTE]  
    > 您可能需要等待 30 秒鐘的時間以進行分類。  
  
10. 切換回 ID_AD_FILE1。 在 [Windows 檔案總管] 中瀏覽至 D:\Finance Documents。  
  
11. 以滑鼠右鍵按一下 [財經備忘] 文件，按一下 [內容]。 按一下 [**分類**] 索引標籤。請注意，[**影響**] 屬性現在已設定為 [**高**]。 按一下 [取消]。  
  
12. 以滑鼠右鍵按一下 [雇用核准要求] 文件，按一下 [內容]。  
  
13. . 按一下 [**分類**] 索引標籤。請注意，[**個人識別資訊**] 屬性現在已設定為 [**高**]。 按一下 [取消]。  
  
## <a name="BKMK_5"></a>步驟5：使用 AD RMS 驗證保護  
  
#### <a name="to-verify-that-the-document-is-protected"></a>確認文件受到保護  
  
1.  切換回 ID_AD_CLIENT1。  
  
2.  開啟 [雇用核准要求]文件。  
  
3.  按一下 [確定] ，允許文件連線到 AD RMS 伺服器。  
  
4.  您現在可以看到文件受到 AD RMS 的保護，因為它包含身份證號碼。  
  

