---
title: 將索引標籤加入 [設定]
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854979"
---
# <a name="add-a-tab-to-settings"></a>將索引標籤加入 [設定]

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以建立及安裝設定管理員在作業系統中使用的程式碼組件，將索引標籤新增至儀表板上的 [設定]。  
  
## <a name="add-a-tab-to-settings"></a>將索引標籤新增至 [設定]  
 您可以執行下列工作將索引標籤新增至 [設定]：  
  
-   [將 ISettingsData 介面的實作新增至組件](Add-a-Tab-to-Settings.md#BKMK_ISettingsData)。  
  
-   [Sign the assembly with an Authenticode signature](Add-a-Tab-to-Settings.md#BKMK_SignAssembly)。  
  
-   [將組件安裝到參照電腦上](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly)。  
  
###  <a name="BKMK_ISettingsData"></a> 將 ISettingsData 介面的實作新增至組件  
 ISettingsData 介面包含在 AdminCommon.dll 組件 (位於 \Program Files\Windows Server\Bin 中) 的 Microsoft.WindowsServerSolutions.Settings 命名空間中。  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>若要將 ISettingsData 程式碼新增至組件  
  
1.  用滑鼠右鍵按一下 **[開始]** 功能表中的程式，然後選取 **[以系統管理員身分執行]**，以系統管理員身分開啟 Visual Studio 2010。  
  
2.  依序按一下 **[檔案]**、 **[新增]**，然後按一下 **[專案]**。  
  
3.  在 **[新增專案]** 對話方塊中，依序按一下 **[Visual C#]** 和 **[類別庫]**，輸入 **DashboardSettingsPage** 作為方案的名稱，然後按一下 **[確定]**。  
  
    > [!IMPORTANT]
    >  安裝在伺服器上的組件，名稱必須為 DashboardSettingsPage.dll，然後將 dll 複製到 %ProgramFiles%\Windows Server\Bin\OEM。  
  
4.  建立要在索引標籤中使用的控制項。在此範例中，這個設定控制項名為 MySettingsControl。  
  
5.  重新命名 Class1.cs 檔案。 例如，MySettingTab.cs。  
  
6.  新增 AdminCommon.dll 檔案的參照。  
  
7.  新增下列 using 陳述式：  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  變更命名空間和類別標頭，以符合下列範例：  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. 起始您為索引標籤建立之控制項的執行個體。例如:   
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. 新增類別的建構函式。 下列程式碼範例顯示建構函式：  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. 新增 Commit 方法，這個方法會提交設定變更。 下列程式碼範例顯示 Commit 方法：  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. 新增 TabControl 方法，這個方法會定義索引標籤的控制項。下列程式碼範例顯示 TabControl 方法：  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. 新增 TabId 方法，這個方法會提供索引標籤的唯一識別項。下列程式碼範例顯示 TabId 方法：  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. 新增 TabOrder 方法，這個方法會傳回索引標籤的順序。下列程式碼範例顯示 TabOrder 方法：  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  使用從 0 開始的數字來定義索引標籤順序。 Microsoft 內建的設定索引標籤會先顯示，然後才根據您定義的索引標籤順序來顯示您的索引標籤。 例如，如果您有三個設定索引標籤，就要依據所要的索引標籤顯示順序，指定索引標籤順序 0、1 和 2。  
  
15. 新增 TabTitle 方法，這個方法會提供索引標籤的標題。下列程式碼範例顯示 TabTitle 方法：  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  標題文字也可以取自資源檔，以滿足當地語系化的需求。  
  
16. 儲存並建置方案。  
  
###  <a name="BKMK_SignAssembly"></a> 登入具有 Authenticode 簽章的組件  
 您必須使用 Authenticode 來簽署組件，才能在作業系統中使用這些組件。 如需簽署組件的相關詳細資訊，請參閱 [使用 Authenticode 簽署和檢查程式碼](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
###  <a name="BKMK_InstallAssembly"></a> 在參照電腦上安裝組件  
 在您成功建置方案之後，請將 DashboardSettingsPage.dll 檔案複製一份到參照電腦上的下列資料夾中：  
  
 **%Programfiles%\Windows Server\Bin\OEM**  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)