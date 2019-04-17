---
title: "準備部署映像"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 16411ab073e9417c52592aa9a6b13707dd461537
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="preparing-the-image-for-deployment"></a>準備部署映像

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

備妥影像一般工具是 sysprep.exe。 執行這項工具可以產生映像和關機伺服器，初始設定伺服器包含映像重新開始時執行。 所有映像的修改必須先完成 sysprep.exe 執行。  
  
> [!NOTE]
>  您可以使用 sysprep.exe 重設 Windows Product 啟用最多個三次。  
  
#### <a name="to-prepare-the-image"></a>準備映像  
  
1.  Delete SkipIC.txt，您可以新增了嗎？  
  
2.  打開提升權限的命令提示字元視窗。 按一下**[開始]**，以滑鼠右鍵按一下 [**命令提示字元**，]，然後選取**以系統管理員身分執行**。  
  
3.  執行下列命令以重設登錄按鍵，讓使用者將會有完整的優惠期間之前，伺服器便會不相容。  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  執行下列命令新增登錄鍵來顯示鍵、語言頁面、地區設定頁面與使用者授權合約頁面。 這些網頁預設不會顯示在初始設定。 因此，如果您的使用者釋出預先安裝的方塊中，您需要新增登錄該鍵。 不過，如果您的使用者釋出 DVD，您不應該新增此機碼，在頁面將會顯示在 WinPE 與初始設定。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  如果您方塊預先金鑰來停用金鑰初始設定] 頁面。 [重要] 頁面將只會顯示時 ShowPreinstallPages true 和 KeyPreInstalled！true。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  執行下列命令，如果您想要停用的硬體需求檢查新增登錄金鑰。 這只是不符合的硬體需求您預先安裝的方塊。 如果您的使用者釋出 DVD，或您方塊符合硬體需求，建議新增該鍵。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  （選擇性）移除登在**%programdata%\Microsoft\Windows Server\Logs**。  
  
8.  準備 sysprep 下列範本中所示自動的 xml 檔案。  
  
    ```  
    <unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:ms="urn:schemas-microsoft-com:asm.v3">  
  
      <settings pass="oobeSystem">  
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
          <!-- Must have -->  
          <OOBE>  
             <HideEULAPage>true</HideEULAPage>  
          </OOBE>  
          <!-- Must have -->  
          <AutoLogon>   
            <Enabled>true</Enabled>   
            <Username>Administrator</Username>   
            <Domain>.</Domain>   
            <Password>   
              <!--You can set any password you like, but keep it consistent with password settings -->       
              <Value>Admin@123</Value>   
              <PlainText>true</PlainText>   
            </Password>   
          </AutoLogon>   
          <UserAccounts>   
           <AdministratorPassword>   
             <!--You can set any password you like, but keep it consistent with auto logon settings -->       
             <Value>Admin@123</Value>   
             <PlainText>true</PlainText>   
           </AdministratorPassword>   
          </UserAccounts>  
  
          <!-- Optional -->  
          <OEMInformation>  
             <HelpCustomized>true</HelpCustomized>  
             <Manufacturer>OEM name</Manufacturer>  
             <Model>model name</Model>  
             <SupportHours>hours</SupportHours>  
             <SupportPhone>123-456-7890</SupportPhone>  
             <SupportURL>http://www.contoso.com</SupportURL>  
          </OEMInformation>  
  
        </component>  
  
      </settings>  
  
      <settings pass="specialize">  
        <component name="Microsoft-Windows-Shell-Setup" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">  
          <!-- Must have -->  
          <ComputerName>Server</ComputerName>          
          <!-- Optional -->  
          <ProductKey>XXXXX-XXXXX-XXXXX-XXXXX-XXXXX</ProductKey>  
        </component>  
      </settings>  
    </unattend>  
    ```  
  
9. 執行下列命令，以 sysprep。  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  您也可以新增 %系統磁碟機 %，而不是在 unattend.xml sysprep 的參數。 如果檔案位於的 c:\ 它將會所涵蓋的使用者的設定，但如果作為 sysprep 的參數，它將不會涵蓋使用者的設定。 在 %系統磁碟機 unattend.xml 繼續嗎每次伺服器重新開機。 因此，請確定該建立 unattend.xml 在 %系統磁碟機之後，不重新啟動伺服器。  
  
10. 執行下列命令新增登錄金鑰略過 OOBE Windows 鍵頁面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. 執行下列命令新增登錄金鑰略過 Windows 語言選取] 頁面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  您必須執行 2 最後一個步驟，或是 Windows OOBE 頁面將會這是因為的初始設定頁面與中斷 server 的遠端管理的案例。  
  
12. 關機後 sysprep 的核取方塊，您可以擷取映像，或重新開機才能繼續 client 電腦的初始設定伺服器。  
  
> [!IMPORTANT]
>  建立伺服器復原媒體計畫的合作夥伴必須擷取映像，並建立復原媒體，才能繼續到下一個步驟。