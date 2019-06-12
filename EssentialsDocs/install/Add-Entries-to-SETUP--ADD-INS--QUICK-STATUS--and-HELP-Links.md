---
title: 將項目新增至「設定」、「增益集」、「快速狀態」和「說明連結」
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f0a66d0d36a3012369a9bc26c513dad069235ad8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433790"
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>將項目新增至「設定」、「增益集」、「快速狀態」和「說明連結」

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以將工作新增至 **[設定]** 、 **[增益集]** 、 **[快速狀態]** 工作清單，並且可以將連結加新增至儀表板首頁中的 [社群連結] 區段。 將名為 OEMHomePageContent.home 檔案的 XML 檔案或名為 OEMHomePageContent.dll 的內嵌資源檔放在 %ProgramFiles%\Windows Server\Bin\Addins\Home 中，即可將工作和連結新增至這些清單和區段。 可使用內嵌資源檔來本地語系化新增的工作和連結中的文字。 .home 檔案包含工作和連結的 XML 定義。  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>將工作新增至 [設定]、[增益集]、[快速狀態] 工作清單，並且將連結新增至 [說明] 工作  
 您可以藉由使用 XML 定義工作和連結，選擇性地建立內嵌資源檔，並且將此檔案安裝在伺服器上，即可將工作新增至 **[設定]** 、 **[增益集]** **[快速狀態]** 工作清單，並且將連結新增至 **[說明]** 工作。 如果 XML 檔案安裝在沒有資源檔的伺服器上，其名稱必須為 OEMHomePageContent.home。 如果使用組件來安裝 XML 檔案和資源檔，其名稱必須為 OEMHomePageContent.dll，並且必須簽署 Authenticode。  
  
### <a name="define-the-tasks-and-links"></a>定義工作和連結  
 您可以使用文字編輯器 (如記事本) 來建立 .home 檔案，如果您還要建立內嵌資源檔，可使用 Visual Studio 2010 或更高版本來定義檔案。 下列程序顯示如何使用 Visual Studio 2010 或更高版本來建立檔案。  
  
##### <a name="to-define-the-tasks-and-links"></a>若要定義工作和連結  
  
1. 用滑鼠右鍵按一下 [開始] 功能表中的程式，然後選取 **[以系統管理員身分執行]** ，以系統管理員身分開啟 Visual Studio 2010 或更高版本。  
  
2. 依序按一下 **[檔案]** 、 **[新增]** ，然後按一下 **[專案]** 。  
  
3. 在 **[範本]** 窗格中，按一下 **[類別庫]** ，在 **[名稱]** 方塊中輸入 **OEMHomePageContent** ，然後按一下 **[確定]** 。  
  
4. 刪除 Class1.cs 檔案。  
  
5. 在新專案上按一下滑鼠右鍵，按一下 **[新增]** ，然後按一下 **[新增項目]** 。  
  
6. 在 [範本]  窗格中，按一下 [XML 檔案]  ，在 [名稱]  方塊中輸入 **OEMHomePageContent.home** ，然後按一下 [新增]  。  
  
   > [!NOTE]
   >  如果安裝沒有資源檔的 XML 檔案，其名稱必須為 OEMHomePageContent.home。 如果它包含在組件中，則可以是任何名稱，只要具有 .home 副檔名即可。  
  
7. 將下列 XML 程式碼新增至 OEMHomePageContent.home 檔案中：  
  
   ```  
  
   <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
      <SetupMyServerTasks>  
         <Task name="MyTask"  
            description="MyTaskDescription"  
            id="GUID">  
                 <Action   
                 name=?MyAction1Name?   
                 image=?IconForAction1?  
                 type=?TaskType?  
                 exelocation=?ActionExeLocation? />  
                 <Action   
                 name=?MyAction2Name?   
                 image=?IconForAction2?  
                 type=?TaskType?  
                 exelocation=?ActionExeLocation? />  
                  ¦  
          </Task>  
                  ¦  
       </SetupMyServerTasks>  
   <MailServiceTasks>  
        <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œConnect to Email Service? category. -->  
   </MailServiceTasks>  
   <LineOfBusinessTasks>  
        <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œAdd-ins? category. -->  
  
   <GetQuickStatusTasks>  
         <Task name="MyQuickStatusTask1"  
            description="MyQuickStatusTask1Desc   "  
            id="GUID"  
            assembly="AssemblyName of quick status query implementation"  
            class="ClassName of quick status query implementation"           
            replaceid="GUID"/>  
              <!--  Same schema as Actions in œSetupMyServerTasks? -->   
            </Task>  
   </GetQuickStatusTasks>  
      <Links>  
         <Link  
            ID=?GUID?  
            Title="Displayed text of the link"  
            Description="A very short description"  
            ShellExecPath="Path to the application or URL"/>  
      </Links>  
   </Tasks>  
   ```  
  
    其中：  
  
   |屬性|描述|  
   |---------------|-----------------|  
   |Name (Task)|針對清單中的工作所顯示的名稱。 如果您建立內嵌資源檔，則此屬性的值便是字串資源。|  
   |description (Task)|工作的描述。 如果您建立內嵌資源檔，則此屬性的值便是字串資源。|  
   |id (Task)|工作的識別碼。 此識別碼必須是 GUID。 您可為 **exe** 工作建立新 GUID，但是對於 **global** 工作，您會使用您為子索引標籤的工作窗格定義工作時所建立的 GUID。如需有關建立 GUID 的詳細資訊，請參閱 [建立 GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。|  
   |image|此欄位將被忽略。|  
   |Name (Action)|顯示工作名稱。|  
   |Type (Action)|描述工作類型。 工作可以是下列工作之一：**global** 工作、**exe** 或 url 工作。 **global** 工作是您為子索引標籤的工作窗格定義工作時所建立的相同全域工作。如需子索引標籤的工作窗格及首頁的快速入門工作 」 或 「 一般工作清單中建立可以使用全域工作的詳細資訊，請參閱 œCreating 支援類別？在 < 如何：建立子索引標籤嗎？[Windows Server 解決方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。 **exe** 工作可用於從「快速入門工作」或「一般工作」清單執行應用程式。|  
   |exelocation|與工作關聯的應用程式路徑。 這個屬性只適用於 **exe** 工作。|  
   |replaceid|此工作所取代之工作的識別碼。|  
   |assembly|提供類別來實作快速狀態查詢的組件的 AssemblyName。 組件必須位於 Program files\ windows server\bin\\。|  
   |class|實作快速狀態查詢的類別的名稱。 類別需要實作 **ITaskStatusQuery** 介面。|  
   |Title (link)|針對連結顯示的文字。 如果您建立內嵌資源檔，則此屬性的值便是字串資源。|  
   |Description (link)|連結目的地的描述。 如果您建立內嵌資源檔，則此屬性的值便是字串資源。|  
   |ShellExecPath|應用程式或 URL 的路徑。<br /><br /> **注意：** ShellExecPath 屬性中支援的環境變數。|  
  
    下列程式碼範例顯示如何定義應用程式的連結：  
  
   ```  
   <Links>  
      <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
   </Links>  
   ```  
  
    下列程式碼範例顯示如何定義網頁的連結：  
  
   ```  
   <Links>  
      <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
   </Links>  
   ```  
  
8. 變更屬性值以表示您的工作或連結。  
  
9. 在 **[方案總管]** 中的 **[OEMHomePageContent.home]** 上按一下滑鼠右鍵，然後按一下 **[內容]** 。  在 **[內容]** 窗格中的 **[建立動作]** 下，選取 **[內嵌資源]** 。  
  
10. 儲存 OEMHomePageContent.home 檔案。  
  
    如需瞭解如何實作快速狀態查詢，請參閱 [Windows Server 解決方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)中的文件與範例。  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>變更設定/增益集工作的狀態  
 列在「設定」/「增益集」的工作，可在已完成 (已為增益集設定) 和未完成 (沒有為增益集設定) 狀態之間切換。  
  
 當您定義與新工作關聯的應用程式時，可以使用 Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper 命名空間的 SetTaskStatus 方法 (包括但未載明於「Windows Server 解決方案 SDK」中)，來變更工作的狀態。 例如，呼叫具有 TaskStatus.Complete 列舉值的 SetTaskStatus 方法 (SetTaskStatus(id, TaskStatus.Complete)，其中 **id** 是工作識別碼)，就可以將核取記號從灰色變更為綠色。 可用的列舉值是 TaskStatus.Complete、TaskStatus.Incomplete 或 TaskStatus.Hidden。  
  
##### <a name="replace-tasks"></a>取代工作  
 您可以將工作的 GUID 新增至工作定義的 replaceid 屬性，以取代在「快速入門工作」或「一般工作」清單中預先定義的工作。 下表列出可以在儀表板中取代的工作和對應識別碼：  
  
|工作名稱|識別碼|  
|---------------|----------------|  
|取得其他 Microsoft 產品的更新|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|設定伺服器備份|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|設定隨處存取|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|設定電子郵件警示通知|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|新增使用者帳戶|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|新增伺服器帳戶|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|隨處存取 - 快速狀態|6093B462-1F04-4212-8804-9BC823070FAD|  
|伺服器備份 - 快速狀態|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|伺服器資料夾 - 快速狀態|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|使用中的使用者帳戶 - 快速狀態|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|電子郵件警示 - 快速狀態|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|電腦 - 快速狀態|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(選用) 建立資源檔  
 如果想要將新增至「快速入門工作」、「一般工作」和「社群連結」的工作中的文字當地語系化，您必須建立含有 .home 檔和 .home.resx 檔 (用來定義文字字串) 的組件。  
  
###### <a name="to-create-the-resource-file"></a>若要建立資源檔  
  
1.  在您為工作建立的專案上按一下滑鼠右鍵，按一下 **[新增]** ，然後按一下 **[新增項目]** 。  
  
2.  在 **[範本]** 窗格中，按一下 **[資源檔案]** ，在 **[名稱]** 方塊中輸入 **OEMHomePageContent.home.resx**，然後按一下 **[新增]** 。  
  
    > [!NOTE]
    >  資源檔可以命名為任何名稱，只要具有 .home.resx 副檔名即可。  
  
3.  對於您新增的每一個工作或連結，您必須將字串和值新增至符合 OEMHomePageContent.home 檔案中所定義之 Task 和 Link 元素的 OEMHomePageContent.home.resx 檔案。 下列程式碼範例顯示針對資源檔結構化的 Tasks.xml 檔案範例：  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyDescription"  
             id="GUID">  
             <Action  
             name="MyActionname"  
             image="IconForAction"  
             type="TaskType"  
             exelocation="ActionExeLocation" />  
          </Task>  
       </SetupMyServerTasks>  
    </Tasks>  
  
    ```  
  
    > [!NOTE]
    >  用於當地語系化屬性中的識別碼不能包含空格。  
  
4.  將 MyTask、MyTaskDescription、MyActionName 和 IconForAction 資源名稱及適當的值新增至 .resx 檔案。  
  
5.  儲存 OEMHomePageContent.home.resx 檔案，然後建置方案。  
  
#####  <a name="BKMK_SignAssembly"></a> 登入具有 Authenticode 簽章的組件  
 您必須使用 Authenticode 來簽署組件，才能在作業系統中使用這些組件。 如需簽署組件的相關詳細資訊，請參閱 [使用 Authenticode 簽署和檢查程式碼](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
##### <a name="install-the-task-files"></a>安裝工作檔案  
 建立 .home 和 .home.resx 檔案之後，您必須在伺服器上安裝這些檔案。  
  
###### <a name="to-install-the-task-files"></a>若要安裝工作檔案  
  
1.  確定您的方案建置無誤。  
  
2.  如果您未建立內嵌資源檔，請將 OEMHomePageContent.home 檔案複製到伺服器上的 **%ProgramFiles%\Windows Server\Bin\Addins\Home** 中。 如果您已建立內嵌資源檔，請將 OEMHomePageContent.dll 檔案複製到伺服器上的 **%ProgramFiles%\Windows Server\Bin\Addins\Home** 中。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)