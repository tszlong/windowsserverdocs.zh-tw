---
title: 自訂共用資料夾
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 861107035408fc39d0dc5e4d94a4d82d8dfba74e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818071"
---
# <a name="customize-shared-folders"></a>自訂共用資料夾

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

伺服器資料夾依預設會建立在磁碟 0 最大的資料磁碟分割上。 合作夥伴可使用下列步驟來自訂位置，以及指定其他伺服器資料夾：  
  
1. 使用自訂磁碟分割設定建立原廠映像，然後在使用 sysprep 前建立新的 Storage 登錄機碼。 在初始設定 (IC) 期間，存放裝置 IC 工作會檢查是否有此登錄機碼。 如果存在，則會在 C:\ServerFolders 目錄中建立預設的伺服器資料夾。  
  
   #### <a name="to-create-a-new-storage-registry-key"></a>建立新的 Storage 登錄機碼  
  
   1.  在伺服器上，將滑鼠移至畫面的右上角，然後按一下 **[搜尋]** 。  
  
   2.  在 [搜尋] 方塊中，輸入 **regedit**，然後按一下 [Regedit] 應用程式。  
  
   3.  在瀏覽窗格中，依序展開 **[HKEY_LOCAL_MACHINE]** 、 **[SOFTWARE]** 與 **[Microsoft]** 。  
  
   4.  以滑鼠右鍵按一下 **[Windows Server]** ，按一下 **[新增]** ，然後按一下 **[機碼]** 。  
  
   5.  將機碼命名為 **Storage**。  
  
   6.  在瀏覽窗格中，以滑鼠右鍵按一下新的 Storage 登錄機碼，按一下 **[新增]** ，然後按 **[DWORD (32-位元) 值]** 。  
  
   7.  將字串命名為 **CreateFoldersOnSystem**。  
  
   8.  在 **[CreateFoldersOnSystem]** 上按一下滑鼠右鍵，然後按一下 **[修改]** 。 **[編輯字串]** 對話方塊隨即出現。  
  
   9. 將此新機碼的值設定為 **1**，然後按一下 **[確定]** 。  
  
2. 使用 PostIC.cmd 指令碼將資料夾移至不同的位置，或建立其他資料夾。 請參閱下列範例：[範例 1：使用 Windows PowerShell 建立自訂資料夾，然後透過 PostIC.cmd 將預設資料夾移至新位置](Customize-Shared-Folders.md#BKMK_Example1)。  
  
3. 使用 Windows Server 解決方案 SDK 將資料夾移至不同的位置，或建立其他資料夾。 請參閱下列範例：[範例 2：使用 Windows Server 解決方案 SDK 建立自訂資料夾並移動現有資料夾](Customize-Shared-Folders.md#BKMK_Example2)。  
  
   或者，合作夥伴也可以將資料夾保留在磁碟機 C 上。如此可讓使用者或轉銷商判斷資料磁碟機上的資料夾配置。  
  
###  <a name="example-1-create-a-custom-folder-and-move-the-default-folders-to-a-new-location-from-posticcmd-by-using-windows-powershell"></a><a name="BKMK_Example1"></a>範例1：使用 Windows PowerShell 建立自訂資料夾，並將預設資料夾移至 Postic.cmd 中的新位置  
  
1.  依照[建立 PostIC.cmd 檔案以便執行初始設定後續的工作](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md)一節中的詳細說明，建立用來執行初始設定後續工作的 PostIC.cmd 檔案。  
  
2.  使用 [記事本]，在 C:\Windows\Setup\Scripts 資料夾中建立名為**customizefolders.ps1**的檔案，然後將下列 Windows PowerShell&reg; 命令貼入檔案中（根據所要的行為取消標示適當的行）。  
  
    ```  
    # Move the Documents folder to D:\ServerFolders  
    #Get-WssFolder -name Documents| Move-WssFolder - NewDrive D:\ -Force  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Move all folders to D:\ServerFolders  
    #foreach( $folder in Get-WssFolder )  
    #{  
    #    Move-WssFolder $folder -NewDrive D:\ -Force  
    #}  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Create a custom folder named Custom Folder  
    #Add-WssFolder -Name CustomFolder -Path D:\ServerFolders\CustomFolder -Description "Custom Folder"  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    exit 0  
    ```  
  
3.  將下列行新增至 PostIC.cmd 檔案並執行此指令碼。  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to default  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="example-2-create-a-custom-folder-and-move-an-existing-folder-by-using-the-windows-server-solutions-sdk"></a><a name="BKMK_Example2"></a>範例2：使用 Windows Server 解決方案 SDK 建立自訂資料夾並移動現有資料夾  
 您所建立的程式碼可編譯為可執行檔，然後從 PostIC.cmd 檔案加以呼叫，或直接從已安裝的增益集呼叫。  
  
```  
static void Main(string[] args)  
{  
 StorageManager storageManager = new StorageManager();  
 //Connect to the StorageManager  
 storageManager.Connect();  
  
 //Move the Documents folder to D:\ServerFolders  
 Folder targetFolder = storageManager.Folders.First(folder => folder.Name == "Documents");  
  
 MoveFolderRequest moveRequest = targetFolder.GetMoveRequest(@"D:\");  
 moveRequest.MoveFolder();  
  
 //Verify operation was successful, if so print result  
 if (moveRequest.Status == OperationStatus.Succeeded)  
 {  
  Console.WriteLine("Folder {0} now located at {1}", targetFolder.Name, targetFolder.Path);  
 }  
  
 string newFolderName = "New Custom Folder";  
 string newFolderLocation = @"C:\ServerFolders\New Custom Folder";  
  
 //Create add request based with specific name and location  
 CreateFolderRequest request = storageManager.GetCreateFolderRequest(newFolderName, newFolderLocation);  
  
 //Give Guest user read only permission to folder  
 request.PermissionsByName.Add(new NamePermission("Guest", Permission.ReadOnly));  
  
 //Create the new folders  
 request.CreateFolder();  
  
 //Verify operation was successful, if so print result  
 if( request.Status == OperationStatus.Succeeded)  
 {  
  Folder newFolder = storageManager.Folders.First(folder => folder.Path == newFolderLocation);  
  
  Console.WriteLine("Folder {0} created at {1}", newFolder.Name, newFolder.Path);  
 }  
}  
```  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)
