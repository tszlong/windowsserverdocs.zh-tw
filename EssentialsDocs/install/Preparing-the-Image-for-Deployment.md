---
title: 準備用於部署的映像
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: a08b8f284701cf486f23a22604ea0c90740146d4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623408"
---
# <a name="preparing-the-image-for-deployment"></a>準備用於部署的映像

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

用來準備映像的一般工具為 sysprep.exe。 執行此工具來一般化映像並關閉伺服器，這樣在重新啟動包含該映像的伺服器時，將會執行初始設定。 對於映像的所有修改都必須在執行 sysprep.exe 之前完成。

> [!NOTE]
>  您最多可以使用 sysprep.exe 重設三次 Windows 產品啓用。

#### <a name="to-prepare-the-image"></a>若要準備映像

1.  刪除新增的 SkipIC.txt。

2.  開啟提升權限的命令提示字元視窗。 按一下 **[開始]**，以滑鼠右鍵按一下 **[命令提示字元]**，然後選取 **[以系統管理員身分執行]**。

3.  執行下列命令以重設登錄機碼，讓使用者在伺服器變成不相容之前有完整的寬限期。

    ```
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f
    ```

4.  執行下列命令來新增登錄機碼，以顯示機碼、語言頁面、地區設定頁面和 EULA 頁面。 根據預設，在初始設定期間將不會顯示這些頁面。 因此，如果要釋出預先安裝的機器，您需要新增此登錄機碼。 不過，如果您要釋出 DVD，則不應新增此機碼，因為這些頁面將會在 WinPE 和初始設定期間顯示。

    ```
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f
    ```

5.  如果您的機器已預先設定機碼，請停用初始設定機碼頁面。 只有在 ShowPreinstallPages = true 且 KeyPreInstalled != true 時，才會顯示機碼頁面。

    ```
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f
    ```

6.  如果要停用硬體需求檢查，請執行下列命令來新增登錄機碼。 這僅適用於預先安裝且不符合硬體需求的機器。 如果要釋出 DVD，或機器符合硬體需求，建議不要新增此機碼。

    ```
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f
    ```

7.  (選用) 移除 **%programdata%\Microsoft\Windows Server\Logs** 下的記錄。

8.  準備 sysprep 的自動安裝 xml 檔，如以下範本所示：

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

9. 執行 sysprep 的下列命令。

    ```
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit
    ```

    > [!IMPORTANT]
    >  您也可以將 unattend.xml 新增至 %systemdrive% 之下而不新增為 sysprep 參數。 如果檔案位於 c：\ 底下它會由使用者的設定所涵蓋，但如果作為 sysprep 的參數，則不會包含在使用者的設定中。 %systemdrive% 之下的 unattend.xml 將會在每次重新啟動伺服器時刪除。 因此，當您在 %systemdrive% 之下建立 unattend.xml 後，請確認伺服器並未重新啟動。

10. 執行下列命令新增登錄機碼以略過 Windows OOBE 機碼頁面。

    ```
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f
    ```

11. 執行下列命令新增登錄機碼以略過 Windows 語言選取頁面。

    ```
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f
    ```

    > [!IMPORTANT]
    >  您必須執行最後 2 個步驟，否則將在初始設定期間出現 Windows OOBE 頁面，並中斷遠端管理的伺服器案例。

12. 在 sysprep 之後關閉機器，您可以擷取映像或重新啟動伺服器，從用戶端電腦繼續執行初始設定。

> [!IMPORTANT]
>  想要建立伺服器復原媒體的合作夥伴必須先擷取映像並建立復原媒體，再進行下一個步驟。