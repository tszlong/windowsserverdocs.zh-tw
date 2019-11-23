---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: Deploy Implementing Retention of Information on File Servers (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 994eadfa205b62c5a512ab130c71fa6c22d1cff6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357539"
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Deploy Implementing Retention of Information on File Servers (Demonstration Steps)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用檔案分類基礎結構與檔案伺服器資源管理員，為資料夾設定保留期間，以及將檔案設定為適用法務保存措施。  
  
**本檔中的**  
  
-   [必要條件](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [步驟1：建立資源屬性定義](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [步驟2：設定通知](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [步驟3：建立檔案管理工作](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [步驟4：手動分類檔案](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>必要條件  
本主題中的步驟假設您已經設定 SMTP 伺服器執行檔案到期通知。  
  
## <a name="BKMK_Step1"></a>步驟1：建立資源屬性定義  
在這個步驟中，將會啟用 [保留期間] 和 [發現性] 資源內容，如此檔案分類基礎結構便可以使用這些資源內容，標記在網路共用資料夾中掃描的檔案。  
  
[使用 Windows PowerShell 執行此步驟](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>建立資源內容定義  
  
1.  在網域控制站上，以 Domain Admins 安全性群組成員的身分登入伺服器。  
  
2.  開啟「Active Directory 管理中心」。 在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [Active Directory 管理中心]。  
  
3.  展開 [動態存取控制]，然後按一下 [資源內容]。  
  
4.  以滑鼠右鍵按一下 [保留期間]，然後按一下 [啟用]。  
  
5.  用滑鼠右鍵按一下 [發現性]，然後按一下 [啟用]。  
  
![解決方案引導](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>步驟2：設定通知  
在這個步驟中，可以使用 [檔案伺服器資源管理員] 主控台來設定 SMTP 伺服器、預設系統管理員的電子郵件地址，以及要傳送報告的預設電子郵件地址。  
  
[使用 Windows PowerShell 執行此步驟](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>設定通知  
  
1.  以本機 Administrators 安全性群組成員的身分登入檔案伺服器。  
  
2.  在 Windows PowerShell 命令提示字元中，輸入 **Update-FsrmClassificationPropertyDefinition**，然後按下 ENTER。 這會將網域控制站上建立的內容定義與檔案伺服器同步。  
  
3.  開啟檔案伺服器資源管理員。 在 [伺服器管理員] 中按一下 [工具]，然後按一下 [檔案伺服器資源管理員]。  
  
4.  以滑鼠右鍵按一下 [檔案伺服器資源管理員 (本機)]，然後按一下 [設定選項]。  
  
5.  在 [電子郵件通知] 索引標籤中，設定下列選項：  
  
    -   在 [SMTP 伺服器名稱或 IP 位址] 方塊中，輸入網路上 SMTP 伺服器的名稱。  
  
    -   在 [預設系統管理員收件者] 方塊中，輸入應該收到通知的系統管理員電子郵件地址。  
  
    -   在預設的 [**寄件者電子郵件地址]** 方塊中，輸入要用來傳送通知的電子郵件地址。  
  
6.  按一下 **\[確定\]** 。  
  
![解決方案引導](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>步驟3：建立檔案管理工作  
在這個步驟中，可以使用 [檔案伺服器資源管理員] 主控台來建立檔案管理工作，它會在每月的最後一天執行，並將符合下列條件的檔案視為已到期：  
  
-   檔案未分類為適用法務保存措施。  
  
-   檔案被分類為具有長期的保留期間。  
  
-   檔案在過去 10 年未曾修改過。  
  
[使用 Windows PowerShell 執行此步驟](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>建立檔案管理工作  
  
1.  以本機 Administrators 安全性群組成員的身分登入檔案伺服器。  
  
2.  開啟檔案伺服器資源管理員。 在 [伺服器管理員] 中按一下 [工具]，然後按一下 [檔案伺服器資源管理員]。  
  
3.  使用滑鼠右鍵按一下 [檔案管理工作]，然後按一下 [建立檔案管理工作]。  
  
4.  在 [一般] 索引標籤上的 [工作名稱] 方塊中，輸入檔案管理工作的名稱，例如「保留工作」。  
  
5.  在 [範圍] 索引標籤上，按一下 [新增]，選擇這個規則中應該包含的資料夾，例如 D:\Finance Documents。  
  
6.  在 [動作] 索引標籤的 [類型] 方塊中，按一下 [檔案到期]。 在 [到期目錄] 方塊中，輸入本機檔案伺服器上已過期的檔案將會移過去的資料夾路徑。 這個資料夾應該具有存取控制清單，只授與檔案伺服器系統管理員存取權。  
  
7.  在 [通知] 索引標籤上，按一下 [新增]。  
  
    -   選取 [將電子郵件傳送給下列系統管理員] 核取方塊。  
  
    -   選取 [傳送電子郵件給擁有受影響檔案的使用者] 核取方塊，然後按一下 [確定]。  
  
8.  在 [條件] 索引標籤上，按一下 [新增]，然後新增下列內容：  
  
    -   在 [內容] 清單中，按一下 [發現性]。 在 [運算子] 清單中，按一下 [不等於]。 在 [值] 清單中，按一下 [保存]。  
  
    -   在 [內容] 清單中，按一下 [保留期間]。 在 [運算子] 清單中，按一下 [等於]。 在 [值] 清單中，按一下 [長期]。  
  
9. 在 [條件] 索引標籤上，選取 [自上次修改檔案以來的天數] 核取方塊，並將值設為 **3650**。  
  
10. 在 [排程] 索引標籤上，按一下 [每月] 選項，然後選取 [上次] 核取方塊。  
  
11. 按一下 **\[確定\]** 。  
  
![解決方案引導](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
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
  
## <a name="BKMK_Step4"></a>步驟4：手動分類檔案  
在這個步驟中，將會手動分類適用法務保存措施的檔案。 這個檔案的上層資料夾將分類為長期的保留期間。  
  
#### <a name="to-manually-classify-a-file"></a>手動分類檔案  
  
1.  以本機 Administrators 安全性群組成員的身分登入檔案伺服器。  
  
2.  瀏覽到步驟 3 中建立的檔案管理工作的範圍內設定的資料夾。  
  
3.  在資料夾上按一下滑鼠右鍵，然後按一下 [內容]。  
  
4.  在 [分類] 索引標籤上，按一下 [保留期間]，按一下 [長期]，然後按一下 [確定]。  
  
5.  用滑鼠右鍵按一下該資料夾中的檔案，然後按一下 [內容]。  
  
6.  在 [分類] 索引標籤上，依序按一下 [發現性]、[保存]、[套用]，然後按一下 [確定]。  
  
7.  在檔案伺服器上，使用 [檔案伺服器資源管理員] 主控台執行檔案管理工作。 檔案管理工作完成後，請檢查資料夾，確定檔案並未移動至到期目錄。  
  
8.  用滑鼠右鍵按一下該資料夾中同一個檔案，然後按一下 [內容]。  
  
9. 在 [分類] 索引標籤上，依序按一下 [發現性]、[不適用]、[套用]，然後按一下 [確定]。  
  
10. 在檔案伺服器上，再次使用 [檔案伺服器資源管理員] 主控台執行檔案管理工作。 檔案管理工作完成後，請檢查資料夾，確定檔案已移動至到期目錄。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [案例：在檔案伺服器上執行資訊的保留](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [規劃檔案伺服器上的資訊保留](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [動態存取控制：案例總覽](Dynamic-Access-Control--Scenario-Overview.md)  
  

