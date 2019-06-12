---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: Deploy Access-Denied Assistance (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 441dad92611e1a4a1135bd15bbcdfd05f38c1be3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445824"
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Deploy Access-Denied Assistance (Demonstration Steps)

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

這個主題說明如何設定拒絕存取時的協助，並確認它可以正常運作。  
  
**在這份文件**  
  
-   [步驟 1：設定拒絕存取時的協助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [步驟 2：設定電子郵件通知設定](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [步驟 3：確認已正確設定拒絕存取時的協助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>步驟 1:設定拒絕存取時的協助  
您可以使用群組原則設定網域內拒絕存取時的協助，或者您可以使用 [檔案伺服器資源管理員] 主控台在每個檔案伺服器上個別設定協助。 您也可以變更檔案伺服器上特定共用資料夾的拒絕存取訊息。  
  
您可以使用群組原則設定網域拒絕存取時的協助，如下所示：  
  
[執行此步驟中使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>使用群組原則設定拒絕存取時的協助  
  
1.  開啟 [群組原則管理]。 在 [伺服器管理員] 按一下 [工具]  ，然後按一下 [群組原則管理]  。  
  
2.  以滑鼠右鍵按一下適當的群組原則，然後按一下 [編輯]  。  
  
3.  依序按一下 [電腦設定]  、[原則]  、[系統管理範本]  、[系統]  ，然後按一下 [拒絕存取時的協助]  。  
  
4.  以滑鼠右鍵按一下 [自訂拒絕存取錯誤訊息]  ，然後按一下 [編輯]  。  
  
5.  選取 [啟用]  選項。  
  
6.  設定下列選項：  
  
    1.  在 [對存取時遭到拒絕的使用者顯示下列訊息]  方塊中，輸入使用者被拒絕存取檔案或資料夾時看到的訊息。  
  
        您可以將巨集新增到訊息，它會插入自訂文字。 巨集包括：  
  
        -   **[原始檔案路徑]** 使用者存取的原始檔案路徑。  
  
        -   **[原始檔案路徑資料夾]** 使用者存取的原始檔案路徑的上層資料夾。  
  
        -   **[系統管理員電子郵件]** 系統管理員的電子郵件收件者清單。  
  
        -   **[資料擁有者電子郵件]** 資料擁有者的電子郵件收件者清單。  
  
    2.  選取 [允許使用者請求協助]  核取方塊。  
  
    3.  保留其餘的預設設定。  
  
![解決方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName AllowEmailRequests -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName GenerateLog -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeDeviceClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeUserClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutAdminOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutDataOwnerOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName ErrorMessage -Type MultiString -value "Type the text that the user will see in the error message dialog box."  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName Enabled -Type DWORD -value 1 
  
```  
  
或者，您可以使用 [檔案伺服器資源管理員] 主控台，在每個檔案伺服器上個別設定拒絕存取時的協助。  
  
[執行此步驟中使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>使用檔案伺服器資源管理員設定拒絕存取時的協助  
  
1.  開啟檔案伺服器資源管理員。 在 [伺服器管理員] 中按一下 [工具]  ，然後按一下 [檔案伺服器資源管理員]  。  
  
2.  以滑鼠右鍵按一下 [檔案伺服器資源管理員 (本機)]  ，然後按一下 [設定選項]  。  
  
3.  按一下 [拒絕存取時的協助]  索引標籤。  
  
4.  選取 [啟用拒絕存取時的協助]  核取方塊。  
  
5.  在 [對存取資料夾或檔案時遭到拒絕的使用者顯示下列訊息]  方塊中，輸入使用者被拒絕存取檔案或資料夾時看到的訊息。  
  
    您可以將巨集新增到訊息，它會插入自訂文字。 巨集包括：  
  
    -   **[原始檔案路徑]** 使用者存取的原始檔案路徑。  
  
    -   **[原始檔案路徑資料夾]** 使用者存取的原始檔案路徑的上層資料夾。  
  
    -   **[系統管理員電子郵件]** 系統管理員的電子郵件收件者清單。  
  
    -   **[資料擁有者電子郵件]** 資料擁有者的電子郵件收件者清單。  
  
6.  按一下 [設定電子郵件要求]  ，選取 [允許使用者請求協助]  核取方塊，然後按一下 [確定]  。  
  
7.  如果您想要查看使用者會看到的錯誤訊息，按一下 [預覽]  。  
  
8.  按一下 [確定]  。  
  
![解決方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
設定拒絕存取時的協助之後，您必須使用群組原則為所有檔案類型啟用。  
  
[執行此步驟中使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>使用群組原則為所有檔案類型設定拒絕存取時的協助  
  
1.  開啟 [群組原則管理]。 在 [伺服器管理員] 按一下 [工具]  ，然後按一下 [群組原則管理]  。  
  
2.  以滑鼠右鍵按一下適當的群組原則，然後按一下 [編輯]  。  
  
3.  依序按一下 [電腦設定]  、[原則]  、[系統管理範本]  、[系統]  ，然後按一下 [拒絕存取時的協助]  。  
  
4.  以滑鼠右鍵按一下 [為用戶端上所有檔案類型啟用拒絕存取時的協助]  ，然後按一下 [編輯]  。  
  
5.  按一下 [已啟用]  ，然後按一下 [確定]  。  
  
![解決方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
您也可以使用 [檔案伺服器資源管理員] 主控台，為檔案伺服器上的每個共用資料夾指定個別拒絕存取訊息。  
  
[執行此步驟中使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>使用檔案伺服器資源管理員為共用資料夾指定個別拒絕存取訊息  
  
1.  開啟檔案伺服器資源管理員。 在 [伺服器管理員] 中按一下 [工具]  ，然後按一下 [檔案伺服器資源管理員]  。  
  
2.  以滑鼠右鍵按一下 [檔案伺服器資源管理員 (本機)]  ，然後按一下 [分類管理]  。  
  
3.  以滑鼠右鍵按一下 [分類屬性]  ，然後按一下 [設定資料夾管理屬性]  。  
  
4.  在 [屬性]  方塊中，按一下 [拒絕存取時的協助訊息]  ，然後按一下 [新增]  。  
  
5.  按一下 [瀏覽]  ，然後選擇應該具有自訂拒絕存取訊息的資料夾。  
  
6.  在 [值]  方塊中，輸入使用者無法存取資料夾中資源時應該看到的訊息。  
  
    您可以將巨集新增到訊息，它會插入自訂文字。 巨集包括：  
  
    -   **[原始檔案路徑]** 使用者存取的原始檔案路徑。  
  
    -   **[原始檔案路徑資料夾]** 使用者存取的原始檔案路徑的上層資料夾。  
  
    -   **[系統管理員電子郵件]** 系統管理員的電子郵件收件者清單。  
  
    -   **[資料擁有者電子郵件]** 資料擁有者的電子郵件收件者清單。  
  
7.  按一下 [確定]  ，然後按一下 [關閉]  。  
  
![解決方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>步驟 2:設定電子郵件通知設定  
您必須在將會傳送拒絕存取時的協助訊息的每個檔案伺服器上設定電子郵件通知設定。  
  
[執行此步驟中使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  開啟檔案伺服器資源管理員。 在 [伺服器管理員] 中按一下 [工具]  ，然後按一下 [檔案伺服器資源管理員]  。  
  
2.  以滑鼠右鍵按一下 [檔案伺服器資源管理員 (本機)]  ，然後按一下 [設定選項]  。  
  
3.  按一下 [電子郵件通知]  索引標籤。  
  
4.  設定下列設定：  
  
    -   在 [SMTP 伺服器名稱或 IP 位址]  方塊中，輸入您組織中 SMTP 伺服器 IP 位址的名稱。  
  
    -   在 **預設系統管理員收件者**並**預設 'From' 電子郵件地址**方塊中，輸入檔案伺服器系統管理員電子郵件地址。  
  
5.  按一下 [傳送測試電子郵件]  ，確保已正確設定電子郵件通知。  
  
6.  按一下 [確定]  。  
  
![解決方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>步驟 3:確認已正確設定拒絕存取時的協助  
您可以確認正在執行 Windows 8 會嘗試存取的共用或中的檔案共用，它們不需要存取權的使用者拒絕存取時的協助正確設定。 拒絕存取訊息出現時，使用者應該會看到 [要求協助]  按鈕。 按一下 [要求協助] 按鈕後，使用者可以指定存取的原因，然後傳送電子郵件給資料夾擁有者或檔案伺服器系統管理員。 資料夾擁有者或檔案伺服器系統管理員可以確認您的電子郵件已到達，且包含適當的詳細資料。  
  
> [!IMPORTANT]  
> 如果您想要在執行 Windows Server 2012 的使用者驗證拒絕存取時的協助，您必須在連接到檔案共用之前先安裝桌面體驗。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [案例：拒絕存取時的協助](Scenario--Access-Denied-Assistance.md)  
  
-   [規劃拒絕存取時的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

