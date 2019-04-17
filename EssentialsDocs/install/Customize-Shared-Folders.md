---
title: "自訂共用的資料夾"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bcdd43183512bb225dd4afa916f2782c6eb79d7e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="customize-shared-folders"></a>自訂共用的資料夾

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

根據預設，伺服器資料夾會建立在 0 上最大資料磁碟分割。 合作夥伴可以自訂的位置，指定其他伺服器資料夾中，使用下列步驟：  
  
1.  使用自訂的磁碟分割設定，請建立原廠映像，並使用 sysprep 建立新的儲存空間登錄金鑰。 在初始設定 (IC)，此登錄鍵檢查儲存空間 IC 工作。 如果有的話，預設伺服器資料夾會建立在 C:\ServerFolders directory 中。  
  
    #### <a name="to-create-a-new-storage-registry-key"></a>若要建立新的儲存空間登錄鍵  
  
    1.  在伺服器上，將滑鼠移到畫面的右上角，然後按一下**搜尋**。  
  
    2.  在搜尋方塊中，輸入**regedit**，然後按**Regedit**應用程式。  
  
    3.  在瀏覽窗格中，展開**跳**，展開 [**軟體**，然後展開 [ **Microsoft**。  
  
    4.  以滑鼠右鍵按一下**Windows Server**，按一下 [**新**，然後按一下 [**鍵**。  
  
    5.  命名按鍵**存放裝置**。  
  
    6.  在瀏覽窗格中，以滑鼠右鍵按一下新的儲存空間登錄鍵、按一下**新**，然後按一下 [ **DWORD（32 位元）值**。  
  
    7.  名稱字串**CreateFoldersOnSystem**。  
  
    8.  以滑鼠右鍵按一下**CreateFoldersOnSystem**，然後按**修改]**。 **編輯字串**對話方塊中出現。  
  
    9. 設定的這個新的金鑰值**1**，然後按**[確定]**。  
  
2.  使用 PostIC.cmd 指令碼，移動至不同位置的資料夾，或建立其他資料夾。 查看下列範例：[第 1 範例：建立自訂資料夾，並使用 Windows PowerShell 來從 PostIC.cmd 移動預設資料夾到新的位置](Customize-Shared-Folders.md#BKMK_Example1)。  
  
3.  使用 Windows Server 方案 SDK 移動至不同位置的資料夾，或建立其他資料夾。 查看下列範例：[第 2 範例：建立自訂資料夾，並使用 Windows Server 方案 SDK 將現有的資料夾](Customize-Shared-Folders.md#BKMK_Example2)。  
  
 合作夥伴（選擇性）c 磁碟機上保留資料的資料夾這可讓使用者或經銷商判斷資料磁碟機上的資料資料夾的版面配置。  
  
###  <a name="BKMK_Example1"></a>範例 1：建立自訂資料夾，並使用 Windows PowerShell 來從 PostIC.cmd 移動預設資料夾到新的位置  
  
1.  建立後執行以在詳細初始設定工作 PostIC.cmd 檔案[適用於執行文章初始設定工作建立 PostIC.cmd 檔案](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md)一節。  
  
2.  使用「記事本」中建立檔名為**customizefolders.ps1**中 C:\Windows\Setup\Scripts 資料夾貼上到檔案的命令下列 Windows PowerShell®（標示適當行，視想要的行為）。  
  
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
  
3.  將下列命令新增至 PostIC.cmd 檔案執行的指令碼。  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to deafult  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="BKMK_Example2"></a>建立自訂資料夾，並使用 Windows Server 方案 SDK 將現有的資料夾範例 2:  
 可以可執行檔，以符合您所建立的程式碼，然後稱為 PostIC.cmd 檔案從或稱為直接從安裝增益集。  
  
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
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)