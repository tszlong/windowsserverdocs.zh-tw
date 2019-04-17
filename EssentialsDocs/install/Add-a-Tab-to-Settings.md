---
title: "索引標籤新增至 [設定"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9eaa1aa5a9c5e8d4c2e36f2000e0adecc83245d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-tab-to-settings"></a>索引標籤新增至 [設定

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以新增儀表板上的設定] 索引標籤來建立和驗證碼組件所使用作業系統設定管理員安裝。  
  
## <a name="add-a-tab-to-settings"></a>索引標籤新增至 [設定  
 您可以設定新增索引標籤，執行下列工作：  
  
-   [新增 ISettingsData 介面實作組件](Add-a-Tab-to-Settings.md#BKMK_ISettingsData)。  
  
-   [登入的驗證碼簽章組件](Add-a-Tab-to-Settings.md#BKMK_SignAssembly)。  
  
-   [組件參照電腦上安裝](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly)。  
  
###  <a name="BKMK_ISettingsData"></a>新增 ISettingsData 介面實作組件  
 包含 ISettingsData 介面 Microsoft.WindowsServerSolutions.Settings 命名空間 AdminCommon.dll 組件位於 \Program Files\Windows Server\Bin 中。  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>若要新增 ISettingsData 程式碼組件  
  
1.  Visual Studio 2010 開放系統管理員的身分，以滑鼠右鍵按一下計畫的**[開始]**功能表，然後選取**以系統管理員身分執行**。  
  
2.  按一下**檔案**，按一下 [**新**，然後按一下 [**專案**。  
  
3.  在**新專案**對話方塊中，按**Visual C#**，按一下 [**程式庫**，輸入**DashboardSettingsPage**的名稱為方案，然後按一下**[確定]**。  
  
    > [!IMPORTANT]
    >  已安裝在伺服器的組件必須 DashboardSettingsPage.dll 命名，然後再將 dll 複製到 %ProgramFiles%\Windows Server\Bin\OEM。  
  
4.  建立控制項您想要使用的索引標籤中。在此範例中設定可以控制稱為 MySettingsControl。  
  
5.  重新命名 Class1.cs 檔案。 例如，MySettingTab.cs。  
  
6.  新增的參考 AdminCommon.dll 檔案。  
  
7.  新增下列使用隱私權聲明：  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  變更命名空間和課程標頭，以符合下列範例：  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. 初始化控制項您所建立的索引標籤上的執行個體。例如：  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. 新增課程對於。 下列範例顯示建構函式：  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. 新增的認可方法，提交設定變更。 下列範例顯示認可的方法：  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. 新增 TabControl 方法，辨識，控制項索引標籤。下列範例顯示 TabControl 方法：  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. 新增 TabId 方法，提供的唯一索引標籤。下列範例顯示 TabId 方法：  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. 新增的 tab 鍵順序方法，傳回] 索引標籤的訂單。下列範例顯示 tab 鍵順序方法：  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  使用數字 0 開始定義] 索引標籤的訂單。 第一次顯示 Microsoft 建設索引標籤，然後根據您所定義的索引標籤順序顯示您的索引標籤。 例如，如果您有三種設定] 索引標籤，您指定] 索引標籤順序 0、1，以及根據您想要顯示的索引標籤的順序 2。  
  
15. 新增的 TabTitle 方法，提供] 索引標籤的標題。下列範例顯示 TabTitle 方法：  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  標題文字可以也會需要當地語系化的資源檔案從。  
  
16. 儲存或建置方案。  
  
###  <a name="BKMK_SignAssembly"></a>登入驗證碼簽章組的件  
 您必須在作業系統中使用的驗證碼登入一組件。 如需有關登入一組件，請查看[簽章和檢查的驗證碼的程式碼](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
###  <a name="BKMK_InstallAssembly"></a>參考電腦上安裝組件  
 成功建置方案之後，地點 DashboardSettingsPage.dll 檔案的複本參照電腦的下列資料夾：  
  
 **%Programfiles%\Windows Server\Bin\OEM**  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)