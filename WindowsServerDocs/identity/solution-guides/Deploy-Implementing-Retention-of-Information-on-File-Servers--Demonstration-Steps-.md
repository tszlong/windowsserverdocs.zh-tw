---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: "部署實作保留的資訊檔案伺服器（示範步驟）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0f79dd72190888340144bc5c109ee31fa301937
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>部署實作保留的資訊檔案伺服器（示範步驟）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以設定保留期間的準則資料夾，並使用檔案分類基礎結構和檔案伺服器資源管理員放在法律按住檔案。  
  
**本文件**  
  
-   [必要條件](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [步驟 1：建立的資源屬性定義](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [步驟 2：設定的通知](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [步驟 3：建立檔案管理工作](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [步驟 4：手動分類檔案](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> 本主題包含範例 Windows PowerShell cmdlet 可供您將部分所述的程序。 如需詳細資訊，請查看[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="prerequisites"></a>必要條件  
本主題中的步驟操作假設您設定檔到期日通知 SMTP 伺服器。  
  
## <a name="BKMK_Step1"></a>步驟 1：建立的資源屬性定義  
在此步驟，我們會讓保留期間的可搜尋性，以及的資源屬性，使檔案分類基礎結構可以使用這些的資源屬性標記網路共用資料夾中的掃描的檔案。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>若要建立的資源屬性定義  
  
1.  網域控制站在登入伺服器網域管理安全性群組成員。  
  
2.  打開 Active Directory 系統管理員中心。 在伺服器管理員中，按一下**工具**，然後按**Active Directory 管理中心**。  
  
3.  展開**動態存取控制**，然後按**資源屬性**。  
  
4.  以滑鼠右鍵按一下**保留期間**，然後按**可讓**。  
  
5.  以滑鼠右鍵按一下**可搜尋性**，然後按**可讓**。  
  
![方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>步驟 2：設定的通知  
在此步驟，我們會使用檔案伺服器資源管理員」主控台設定 SMTP 伺服器、預設系統管理員的電子郵件地址，並從傳送報告預設電子郵件地址。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>若要設定的通知  
  
1.  以系統管理員安全性群組成員登入該檔案伺服器。  
  
2.  Windows PowerShell 命令提示字元中，輸入**更新-FsrmClassificationPropertyDefinition**，然後按 ENTER 鍵。 這將會同步處理檔案伺服器的網域控制站的屬性定義。  
  
3.  打開檔案伺服器資源管理員。 在伺服器管理員中，按一下**工具**，然後按**檔案伺服器資源管理員**。  
  
4.  以滑鼠右鍵按一下**（本機）檔案伺服器資源管理員**，然後按一下 [**設定選項**。  
  
5.  在**的電子郵件通知**索引標籤上，進行下列設定：  
  
    -   在**SMTP 伺服器名稱或 IP 位址**方塊中，輸入您的網路 SMTP 伺服器的名稱。  
  
    -   在**系統管理員收件者預設的**方塊中，輸入應該取得通知的系統管理員的電子郵件地址。  
  
    -   在**「從「電子郵件地址預設**方塊中，輸入應該用來傳送通知的電子郵件地址。  
  
6.  按一下**[確定]**。  
  
![方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>步驟 3：建立檔案管理工作  
在此步驟，我們會使用檔案伺服器資源管理員」主控台建立檔案管理工作，將會在每個月的最後一天上執行過期的任何檔案，使用下列條件：  
  
-   檔案不歸類為法律保留。  
  
-   將檔案被歸類為有長期的保留時間。  
  
-   在過去 10 年尚未修改檔案。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>若要建立檔案管理工作  
  
1.  以系統管理員安全性群組成員登入該檔案伺服器。  
  
2.  打開檔案伺服器資源管理員。 在伺服器管理員中，按一下**工具**，然後按**檔案伺服器資源管理員**。  
  
3.  以滑鼠右鍵按一下**檔案管理工作**，然後按**建立檔案管理工作**。  
  
4.  在**一般**索引標籤的**任務名稱**方塊中，輸入名稱的檔案管理工作，例如保留工作。  
  
5.  在**範圍**索引標籤上，按一下 [**新增**，並選擇應包含在此規則，例如 D:\Finance 文件中的資料夾。  
  
6.  在**動作**索引標籤的**輸入**方塊中，按一下 [**檔案到期**。 在**到期 directory**方塊中，輸入本機檔案伺服器過期的檔案將會移到資料夾的路徑。 此資料夾應該會有授與檔案伺服器管理員存取存取控制清單。  
  
7.  在**通知**索引標籤上，按**新增]**。  
  
    -   選取 [**傳送電子郵件下列系統管理員以**核取方塊。  
  
    -   選取**使用者傳送電子郵件受影響的檔案以**核取方塊，並再按**[確定]**。  
  
8.  在**條件**索引標籤上，按一下 [**新增**，然後新增下列屬性：  
  
    -   在**屬性**清單中，按**可搜尋性**。 在**電信業者**清單中，按**等於**。 在**值**清單中，按**按住**。  
  
    -   在**屬性**清單中，按**保留期間**。 在**電信業者**清單中，按**等**。 在**值**清單中，按**長期**。  
  
9. 在**條件**索引標籤，選取**天自從上次修改檔案**核取方塊，然後再將值設定為**3650**。  
  
10. 在**排程**索引標籤上，按一下 [**每月**選項，然後選取**最後一個**核取方塊。  
  
11. 按一下**[確定]**。  
  
![方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
$fmjexpiration = New-FSRMFmjAction -Type 'Expiration' -ExpirationFolder folder  
$fmjNotificationAction = New-FsrmFmjNotificationAction -Type Email -MailTo "[FileOwner],[AdminEmail]"  
$fmjNotification = New-FsrmFMJNotification -Days 10 -Action @($fmjNotificationAction)  
$fmjCondition1 = New-FSRMFmjCondition -Property 'Discoverability_MS' -Condition 'NotEqual' -Value "Hold" 
$fmjCondition2 = New-FSRMFmjCondition -Property 'RetentionPeriod_MS' -Condition 'Equal' -Value "Long-term"  
$fmjCondition3 = New-FSRMFmjCondition -Property 'File.DateLastAccessed' -Condition 'Equal' -Value 3650  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Monthly @(-1)    
$fmj1=New-FSRMFileManagementJob -Name "Retention Task" -Namespace @('D:\Finance Documents') -Action $fmjexpiration -Schedule $schedule -Notification @($fmjNotification) -Condition @( $fmjCondition1, $fmjCondition2, $fmjCondition3)  
```  
  
## <a name="BKMK_Step4"></a>步驟 4：手動分類檔案  
在此步驟，我們以手動方式可將法律保留檔案。 上層資料夾的檔案將會被歸類長期保留期間的。  
  
#### <a name="to-manually-classify-a-file"></a>若要手動分類檔案  
  
1.  以系統管理員安全性群組成員登入該檔案伺服器。  
  
2.  瀏覽至設定中的檔案管理工作建立執行「步驟 3 範圍的資料夾。  
  
3.  以滑鼠右鍵按一下資料夾，然後按一下**屬性**。  
  
4.  在**分類**索引標籤上，按一下 [**保留期間**，按一下 [**長期**，然後按一下**[確定]**。  
  
5.  以滑鼠右鍵按一下該資料夾中的檔案，然後按一下**屬性**。  
  
6.  在**分類**索引標籤上，按一下 [**可搜尋性**，按一下 [**按住**，按一下**套用**，，然後按一下 [ **[確定]**。  
  
7.  使用 [檔案伺服器資源管理員」主控台執行管理工作檔案的檔案伺服器。 檔案管理工作完成之後，請資料夾，並確定檔案不移至到期 directory。  
  
8.  以滑鼠右鍵按一下該資料夾，在同一個檔案，然後按一下**屬性**。  
  
9. 上**分類**索引標籤上，按一下 [**可搜尋性**，按一下 [**不適用**，按一下**套用**，然後按一下 [ **[確定]**。  
  
10. 再試一次使用 [檔案伺服器資源管理員」主控台執行管理工作檔案的檔案伺服器。 檔案管理任務完成後，請檢查資料夾，確定檔案已移至到期 directory。  
  
## <a name="BKMK_Links"></a>也了  
  
-   [案例︰ 檔案伺服器實作保留的資訊](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [保持檔案伺服器的詳細資訊的計劃](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

