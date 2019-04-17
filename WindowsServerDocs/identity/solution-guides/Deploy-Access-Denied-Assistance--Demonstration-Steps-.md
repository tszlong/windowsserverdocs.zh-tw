---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: "部署存取的協助（示範步驟）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5201441ba884fe4658b917919e60c7d20530341b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>部署存取的協助（示範步驟）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題如何設定存取的協助，並確認正常運作。  
  
**本文件**  
  
-   [步驟 1：設定存取的協助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [步驟 2：設定電子郵件通知設定](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [步驟 3：確認已正確設定存取的協助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> 本主題包含範例 Windows PowerShell cmdlet 可供您將部分所述的程序。 如需詳細資訊，請查看[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_1"></a>步驟 1：設定存取的協助  
您可以使用群組原則、設定存取的網域中的協助，或您可以設定協助排列每個檔案伺服器上使用「檔案伺服器資源管理員」主控台。 您也可以變更特定檔案伺服器上的共用資料夾的存取的訊息。  
  
您可以使用群組原則，如下所示設定網域存取的協助：  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>若要使用群組原則設定存取的協助  
  
1.  開放群組原則管理。 在伺服器管理員中，按一下**工具**，然後按**群組原則管理**。  
  
2.  適當的群組原則，以滑鼠右鍵按一下，然後按一下**編輯**。  
  
3.  按一下**電腦設定**，按一下 [**原則**，按一下 [**系統管理範本]**，按一下**系統**，，然後按一下 [ **Access-Denied 協助**。  
  
4.  以滑鼠右鍵按一下**自訂訊息存取錯誤的**，然後按一下 [**編輯**。  
  
5.  選取 [**啟用**選項。  
  
6.  設定下列選項：  
  
    1.  在**無法存取的使用者顯示以下訊息**方塊中，輸入訊息使用者將會看到他們時無法檔案或資料夾的存取。  
  
        您可以新增巨集的訊息，會將自訂的文字。 巨集包括：  
  
        -   **[原始檔案路徑]**的原始的檔案路徑存取的使用者。  
  
        -   **[原始的檔案路徑資料夾]**上層資料夾的存取的使用者的原始檔案路徑。  
  
        -   **[管理電子郵件]**系統管理員的電子郵件收件者的清單。  
  
        -   **[資料擁有者電子郵件]**資料擁有者的電子郵件收件者清單。  
  
    2.  選取 [**讓使用者要求協助**核取方塊。  
  
    3.  保留其他預設設定。  
  
![方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
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
  
或者，您可以設定存取的協助排列每個檔案伺服器上使用「檔案伺服器資源管理員」主控台。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>使用檔案伺服器資源管理員進行存取的協助  
  
1.  打開檔案伺服器資源管理員。 在伺服器管理員中，按一下**工具**，然後按**檔案伺服器資源管理員**。  
  
2.  以滑鼠右鍵按一下**（本機）檔案伺服器資源管理員**，然後按一下 [**設定選項**。  
  
3.  按一下**Access-Denied 協助**索引標籤。  
  
4.  選取 [**可以存取的協助**核取方塊。  
  
5.  在**無法檔案或資料夾的存取的使用者顯示以下訊息**方塊中，輸入訊息使用者將會看到他們時無法檔案或資料夾的存取。  
  
    您可以新增巨集的訊息，會將自訂的文字。 巨集包括：  
  
    -   **[原始檔案路徑]**的原始的檔案路徑存取的使用者。  
  
    -   **[原始的檔案路徑資料夾]**上層資料夾的存取的使用者的原始檔案路徑。  
  
    -   **[管理電子郵件]**系統管理員的電子郵件收件者的清單。  
  
    -   **[資料擁有者電子郵件]**資料擁有者的電子郵件收件者清單。  
  
6.  按一下**設定電子郵件要求**，請選取**可讓使用者要求協助**核取方塊，並再按**[確定]**。  
  
7.  按一下**預覽**如果您想要查看的錯誤訊息給使用者。  
  
8.  按一下**[確定]**。  
  
![方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
設定存取的協助之後，您必須所有的檔案類型的可以使用群組原則。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>若要使用群組原則的所有檔案類型的設定存取的協助  
  
1.  開放群組原則管理。 在伺服器管理員中，按一下**工具**，然後按**群組原則管理**。  
  
2.  適當的群組原則，以滑鼠右鍵按一下，然後按一下**編輯**。  
  
3.  按一下**電腦設定**，按一下 [**原則**，按一下 [**系統管理範本]**，按一下**系統**，，然後按一下 [ **Access-Denied 協助**。  
  
4.  以滑鼠右鍵按一下**的所有檔案類型，可以存取的協助 client**，然後按一下 [**編輯**。  
  
5.  按一下**啟用**，然後按**[確定]**。  
  
![方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
您也可以使用 [檔案伺服器資源管理員」主控台檔案伺服器上指定的每個共用資料夾不同存取的訊息。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>若要指定的共用資料夾不同存取的訊息使用檔案伺服器資源管理員  
  
1.  打開檔案伺服器資源管理員。 在伺服器管理員中，按一下**工具**，然後按**檔案伺服器資源管理員**。  
  
2.  展開**（本機）檔案伺服器資源管理員**，然後按一下 [**管理分類**。  
  
3.  以滑鼠右鍵按一下**分類屬性**，然後按一下 [**設定資料夾管理屬性**。  
  
4.  在**屬性**方塊中，按一下 [ **Access-Denied 協助訊息**，然後按一下 [**新增**。  
  
5.  按一下**瀏覽]**，然後選擇 [應該會有自訂存取的郵件資料夾。  
  
6.  在**值**方塊中輸入時，他們無法存取該資料夾中的資源必須向使用者的訊息。  
  
    您可以新增巨集的訊息，會將自訂的文字。 巨集包括：  
  
    -   **[原始檔案路徑]**的原始的檔案路徑存取的使用者。  
  
    -   **[原始的檔案路徑資料夾]**上層資料夾的存取的使用者的原始檔案路徑。  
  
    -   **[管理電子郵件]**系統管理員的電子郵件收件者的清單。  
  
    -   **[資料擁有者電子郵件]**資料擁有者的電子郵件收件者清單。  
  
7.  按一下**[確定]**，然後按**關閉**。  
  
![方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>步驟 2：設定電子郵件通知設定  
您必須設定電子郵件通知設定，將會傳送訊息存取的協助每個檔案伺服器上。  
  
[執行此步驟，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  打開檔案伺服器資源管理員。 在伺服器管理員中，按一下**工具**，然後按**檔案伺服器資源管理員**。  
  
2.  以滑鼠右鍵按一下**（本機）檔案伺服器資源管理員**，然後按一下 [**設定選項**。  
  
3.  按一下**的電子郵件通知**索引標籤。  
  
4.  下列設定：  
  
    -   在**SMTP 伺服器名稱或 IP 位址**方塊中，輸入您的組織 SMTP 伺服器的 IP 位址的名稱。  
  
    -   在**預設的系統管理員收件者**和**預設 '電子郵件地址從]**方塊中，輸入檔案伺服器管理員中的電子郵件地址。  
  
5.  按一下**傳送測試電子郵件]**以確保電子郵件通知設定正確。  
  
6.  按一下**[確定]**。  
  
![方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>步驟 3：確認已正確設定存取的協助  
您可以檢查存取的協助正確設定所需執行的 Windows 8 嘗試存取共用或中的檔案共用它們不擁有的存取權的使用者。 訊息存取的出現時，使用者應該會看到**要求協助**按鈕。 按一下要求協助按鈕後，使用者可以指定存取的原因，然後資料夾擁有者或檔案伺服器管理員傳送電子郵件。 資料夾擁有者或檔案伺服器管理員可確認您的電子郵件貨送到時包含適當的詳細資訊。  
  
> [!IMPORTANT]  
> 如果您想要存取的協助確認所遇到的使用者身分執行的 Windows Server 2012，您必須先連接檔案共用安裝桌面體驗。  
  
## <a name="BKMK_Links"></a>也了  
  
-   [案例：存取的協助](Scenario--Access-Denied-Assistance.md)  
  
-   [規劃存取的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

