---
title: "將項目新增至 [設定、增益集，快速狀態，並協助連結"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 6d3303f2c6d84932ad9d5dee8a547cd478447732
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>將項目新增至 [設定、增益集，快速狀態，並協助連結

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以加入工作，**安裝**，**增益集**、**快速狀態**工作清單，以及您可以新增儀表板中的 [首頁] 頁面的社群連結區段連結。 工作及以下連結會將 XML 檔案名稱為 OEMHomePageContent.home 檔案或 %ProgramFiles%\Windows Server\Bin\Addins\Home 命名 OEMHomePageContent.dll embedded 的資源檔案新增至這些清單和章節。 Embedded 的資源檔案，可以用於將工作和您所新增的連結文字。 .Home 檔案包含 XML 定義的任務和的連結。  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>設定、增益集，快速狀態工作清單加入工作，並加入協助工作連結  
 您可以加入工作，**安裝**，**增益集**、**快速狀態**工作清單及以下連結**協助**定義的任務和連結使用 XML，工作選擇性建立 embedded 的資源檔案，並安裝在伺服器上的檔案。 如果正在不資源檔案伺服器上安裝 XML 檔案，它必須命名 OEMHomePageContent.home。 如果組件用於安裝 XML 檔案和資源檔案，它必須名 OEMHomePageContent.dll 和必須簽署的驗證碼。  
  
### <a name="define-the-tasks-and-links"></a>定義任務和的連結  
 您可以建立.home 檔案，使用文字編輯器例如記事本或如果您正在也建立了 embedded 的資源檔案使用 Visual Studio 2010 或更高版本定義檔案。 下列程序示範如何建立檔案使用 Visual Studio 2010 或更高版本。  
  
##### <a name="to-define-the-tasks-and-links"></a>定義任務和的連結  
  
1.  打開 Visual Studio 2010 或更高版本系統管理員的身分，以滑鼠右鍵按一下 [開始] 功能表中的程式，並選取 [**系統管理員身分執行**。  
  
2.  按一下**檔案**，按一下 [**新**，然後按一下 [**專案**。  
  
3.  在**範本**窗格中，按**程式庫**，輸入**OEMHomePageContent**在**名稱**方塊，然後再按一下**[確定]**。  
  
4.  Delete Class1.cs 檔案。  
  
5.  以滑鼠右鍵按一下新增專案中，按一下**新增**，然後按**新項目**。  
  
6.  在**範本**窗格中，按**XML 檔案**，輸入**OEMHomePageContent.home**在**名稱**方塊，然後再按一下**新增**。  
  
    > [!NOTE]
    >  如果您正在安裝 XML 檔案不資源檔案，它必須命名 OEMHomePageContent.home。 如果在組件包含它，它可提供任何名稱，只要有.home 擴充功能。  
  
7.  新增下列程式碼 XML OEMHomePageContent.home 檔案：  
  
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
  
     位置：  
  
    |屬性|描述|  
    |---------------|-----------------|  
    |名稱（工作）|清單中顯示的名稱。 如果您建立 embedded 的資源檔案，此屬性的值為字串資源。|  
    |描述（工作）|工作的描述。 如果您建立 embedded 的資源檔案，此屬性的值為字串資源。|  
    |來電顯示（工作）|工作的識別碼。 此必須是 GUID。 建立新的 GUID 適用於**exe**工作，但的**通用**工作，您使用的 GUID 時所定義子] 索引標籤的 [工作] 窗格中的工作。如需有關建立 GUID 的詳細資訊，請查看[建立 Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。|  
    |影像|此欄位會忽略。|  
    |名稱（動作）|顯示名稱的工作。|  
    |輸入（動作）|描述類型的工作。 工作可以下列其中一個動作:-**全球**工作，**exe**，或 url 工作。 A**全球**工作是定義窗格工作子索引標籤中時建立的相同全球工作。如需有關建立通用工作，可用來取得開始工作或一般工作清單的 [首頁] 頁面這兩個子] 索引標籤的 [工作] 窗格中，查看 œCreating 支援類別嗎？在 œHow 以：建立子索引標籤嗎？[Windows Server 方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。 **Exe**工作可以用來執行應用程式取得開始工作，或列出一般工作。|  
    |exelocation|工作相關聯的應用程式的路徑。 此屬性僅適用於**exe**工作。|  
    |replaceid|這項工作會取代工作識別碼。|  
    |組件|組件會提供實作快速狀態查詢課程 AssemblyName。 組件必須位於程式 files\ windows server\bin\\。|  
    |課|名稱課程實作快速狀態查詢。 必須實作課程**ITaskStatusQuery**介面。|  
    |職稱（連結）|顯示連結的文字。 如果您建立 embedded 的資源檔案，此屬性的值為字串資源。|  
    |描述（連結）|連結目的地的描述。 如果您建立 embedded 的資源檔案，此屬性的值為字串資源。|  
    |ShellExecPath|應用程式或 URL 的路徑。<br /><br /> **注意：**中 ShellExecPath 屬性支援環境變數。|  
  
     下列範例顯示如何應用程式定義的連結：  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     下列範例顯示如何定義網頁連結：  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  變更代表您的工作或連結屬性的值。  
  
9. 在**總管**，以滑鼠右鍵按一下**OEMHomePageContent.home**，然後按一下 [**屬性**。  在**屬性**窗格中，在**建置動作**、選取**Embedded 資源**。  
  
10. 儲存 OEMHomePageContent.home 檔案。  
  
 以了解如何實作查詢快速狀態，請參考文件和範例在[Windows Server 方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>變更的設定日增益集工作的狀態  
 列出的安裝程式與增益集的工作可切換的狀態完成（適用於 Add-ins 設定）和尚未完成（不的 Add-ins 設定）。  
  
 當您定義將您新的工作相關聯的應用程式時，您可以使用 SetTaskStatus 方法 Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper 命名空間（包含，但無法在 Windows Server 方案 SDK 記載）的變更工作的狀態。 例如您可能會核取記號從變成灰色 green SetTaskStatus 使用呼叫方法 TaskStatus.Complete 列舉值 (SetTaskStatus (id TaskStatus.Complete)，其中**id**是識別碼工作)。 列舉值，可以使用的 TaskStatus.Complete、TaskStatus.Incomplete 或 TaskStatus.Hidden。  
  
##### <a name="replace-tasks"></a>取代工作  
 您可以取代工作 GUID 加入 replaceid 屬性工作定義的預定取得開始工作或的一般工作清單中的工作。 下表列出的工作，可以要求更換儀表板中的對應識別碼：  
  
|工作的名稱|識別碼|  
|---------------|----------------|  
|取得適用於其他 Microsoft 你更新|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|設定伺服器的備份|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|隨處存取設定|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|設定電子郵件通知|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|新增帳號|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|新增資料夾伺服器|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|隨處存取-快速狀態|6093B462-1F04-4212-8804-9BC823070FAD|  
|伺服器備份-快速狀態|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|伺服器資料夾-快速狀態|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|使用中的使用者帳號-快速狀態|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|電子郵件通知-快速狀態|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|快速狀態的電腦-|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>（選擇性）建立資源檔案  
 如果您想要將您加入了開始工作、一般工作和社群連結的工作中的文字，您必須建立組件包含.home 檔案和。定義字串 home.resx 檔案。  
  
###### <a name="to-create-the-resource-file"></a>若要建立的資源檔案  
  
1.  以滑鼠右鍵按一下您建立您的工作專案中，按一下**新增**，然後按一下 [**新項目**。  
  
2.  在**範本**窗格中，按**資源檔案**，輸入**OEMHomePageContent.home.resx**在**名稱**方塊，然後再按一下**新增**。  
  
    > [!NOTE]
    >  資源檔案可以提供任何名稱，只要有。home.resx 擴充功能。  
  
3.  每個任務或您新增的連結，您必須符合的任務和連結的項目所定義 OEMHomePageContent.home 檔案中的 OEMHomePageContent.home.resx 檔案新增字串及值。 下列範例顯示資源檔案均的結構 Tasks.xml 檔案的範例：  
  
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
    >  在所使用的當地語系化屬性識別碼能空間。  
  
4.  新增 MyTask、MyTaskDescription、MyActionName，以及 IconForAction 資源名稱.resx 檔案以適當的值。  
  
5.  儲存 OEMHomePageContent.home.resx 檔案，以及然後方案。  
  
#####  <a name="BKMK_SignAssembly"></a>登入驗證碼簽章組的件  
 您必須在作業系統中使用的驗證碼登入一組件。 如需有關登入一組件，請查看[簽章和檢查的驗證碼的程式碼](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
##### <a name="install-the-task-files"></a>安裝工作檔案  
 建立.home 之後和。home.resx 檔案，您必須安裝在伺服器上。  
  
###### <a name="to-install-the-task-files"></a>若要安裝的工作檔案  
  
1.  請確定您的方案，不會出現錯誤的組建。  
  
2.  如果您未建立 embedded 的資源檔案，OEMHomePageContent.home 檔案複製到**%ProgramFiles%\Windows Server\Bin\Addins\Home**在伺服器上。 如果您建立 embedded 的資源檔案，請複製 OEMHomePageContent.dll 檔案**%ProgramFiles%\Windows Server\Bin\Addins\Home**在伺服器上。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)