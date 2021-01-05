---
title: 將索引標籤加入 [設定]
description: 瞭解如何藉由建立和安裝程式碼元件，將索引標籤新增至儀表板上的設定。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 45ae9fd3561430006d99cf99849f6e1696de0859
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711583"
---
# <a name="add-a-tab-to-settings"></a>將索引標籤加入 [設定]

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以建立及安裝設定管理員在作業系統中使用的程式碼組件，將索引標籤新增至儀表板上的 [設定]。

## <a name="add-a-tab-to-settings"></a>將索引標籤新增至 [設定]
 您可以執行下列工作將索引標籤新增至 [設定]：

-   [將 ISettingsData 介面的實作新增至組件](Add-a-Tab-to-Settings.md#BKMK_ISettingsData)。

-   [使用 Authenticode 簽署來簽署組件](Add-a-Tab-to-Settings.md#BKMK_SignAssembly)。

-   [將組件安裝到參照電腦上](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly)。

###  <a name="add-an-implementation-of-the-isettingsdata-interface-to-the-assembly"></a><a name="BKMK_ISettingsData"></a> 將 ISettingsData 介面的執行新增至元件
 ISettingsData 介面包含在 AdminCommon.dll 組件 (位於 \Program Files\Windows Server\Bin 中) 的 Microsoft.WindowsServerSolutions.Settings 命名空間中。

##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>若要將 ISettingsData 程式碼新增至組件

1.  用滑鼠右鍵按一下 **[開始]** 功能表中的程式，然後選取 **[以系統管理員身分執行]**，以系統管理員身分開啟 Visual Studio 2010。

2.  依序按一下 **[檔案]**、**[新增]**，然後按一下 **[專案]**。

3.  在 [新增專案] 對話方塊中，按一下 [Visual C#]，按一下 [類別庫]，輸入 **DashboardSettingsPage** 作為解決方案的名稱，然後按一下 [確定]。

    > [!IMPORTANT]
    >  安裝在伺服器上的組件，名稱必須為 DashboardSettingsPage.dll，然後將 dll 複製到 %ProgramFiles%\Windows Server\Bin\OEM。

4.  建立您要在索引標籤中使用的控制項。在此範例中，設定控制項的名稱是 MySettingsControl。

5.  重新命名 Class1.cs 檔案。 例如，MySettingTab.cs。

6.  新增 AdminCommon.dll 檔案的參照。

7.  加入下列 using 陳述式：

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

9. 將您為索引標籤建立的控制項實例具現化。例如：

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

12. 加入 TabControl 方法，以識別索引標籤的控制項。下列程式碼範例顯示 TabControl 方法：

    ```

    System.Windows.Forms.Control ISettingsData.TabControl
    {
       get { return tab; }
    }
    ```

13. 加入 TabId 方法，它會提供索引標籤的唯一識別碼。下列程式碼範例顯示 TabId 方法：

    ```

    private Guid id = Guid.NewGuid();

    Guid ISettingsData.TabId
    {
       get { return id; }
    }
    ```

14. 加入 TabOrder 方法，此方法會傳回索引標籤的順序。下列程式碼範例顯示 TabOrder 方法：

    ```

    int ISettingsData.TabOrder
    {
       get { return 0; }
    }
    ```

    > [!NOTE]
    >  使用從 0 開始的數字來定義索引標籤順序。 Microsoft 內建的設定索引標籤會先顯示，然後才根據您定義的索引標籤順序來顯示您的索引標籤。 例如，如果您有三個設定索引標籤，就要依據所要的索引標籤顯示順序，指定索引標籤順序 0、1 和 2。

15. 加入 TabTitle 方法，它會提供索引標籤的標題。下列程式碼範例顯示 TabTitle 方法：

    ```

    string ISettingsData.TabTitle
    {
      get { return "My Settings Tab"; }
    }
    ```

    > [!NOTE]
    >  標題文字也可以取自資源檔，以滿足當地語系化的需求。

16. 儲存並建置方案。

###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a> 使用 Authenticode 簽章簽署元件
 您必須使用 Authenticode 來簽署組件，才能在作業系統中使用這些組件。 如需簽署組件的相關詳細資訊，請參閱 [使用 Authenticode 簽署和檢查程式碼](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。

###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a> 在參照電腦上安裝元件
 在您成功建置方案之後，請將 DashboardSettingsPage.dll 檔案複製一份到參照電腦上的下列資料夾中：

 **%Programfiles%\Windows Server\Bin\OEM**

## <a name="see-also"></a>另請參閱
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)