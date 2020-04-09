---
title: 如何偵測、啟用和停用 Windows 中的 SMBv1、SMBv2 和 SMBv3
description: 描述如何啟用和停用 Windows 用戶端和伺服器環境中的伺服器訊息區通訊協定（SMBv1、SMBv2 和 SMBv3）。
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: d6c47843dedaf45842f70d1bb408b59d63c03eb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815501"
---
# <a name="how-to-detect-enable-and-disable-smbv1-smbv2-and-smbv3-in-windows"></a>如何偵測、啟用和停用 Windows 中的 SMBv1、SMBv2 和 SMBv3

## <a name="summary"></a>摘要

本文說明如何在 SMB 用戶端和伺服器元件上啟用和停用伺服器訊息區（SMB）第1版（SMBv1）、SMB 版本2（SMBv2）和 SMB 第3版（SMBv3）。 

> [!IMPORTANT]
> 我們**建議您不要**停用 SMBv2 或 SMBv3。 僅停用 SMBv2 或 SMBv3 做為暫時的疑難排解措施。 請勿讓 SMBv2 或 SMBv3 停用。  

在 Windows 7 和 Windows Server 2008 R2 中，停用 SMBv2 會停用下列功能： 
 
- 要求複利-允許以單一網路要求傳送多個 SMB 2 要求    
- 較大的讀取和寫入-更有效率地使用更快速的網路    
- 資料夾和檔案屬性的快取-用戶端保留資料夾和檔案的本機複本    
- 持久的控制碼-允許連接在暫時中斷連線時，以透明的方式重新連線到伺服器    
- 改良的訊息簽署-HMAC SHA-256 將 MD5 取代為雜湊演算法    
- 增強檔案共用的擴充性-每台伺服器的使用者、共用和開啟檔案數目大幅增加    
- 符號連結的支援    
- 用戶端 oplock 租用模型-限制用戶端與伺服器之間傳輸的資料，改善高延遲網路的效能並增加 SMB 伺服器的擴充性    
- 大型 MTU 支援-適用于 10 gigabye （GB） Ethernet 的完整使用    
- 改良的能源效率-已開啟伺服器的用戶端可以進入睡眠狀態    

在 Windows 8、Windows 8.1、Windows 10、Windows Server 2012 和 Windows Server 2016 中，停用 SMBv3 會停用下列功能（以及前述清單中所述的 SMBv2 功能）： 
 
- 透明容錯移轉-在維護或容錯移轉期間，用戶端不會中斷叢集節點的重新連線    
- Scale Out –所有檔案叢集節點上共用資料的平行存取     
- 多重通道-當用戶端與伺服器之間有多個路徑可用時，網路頻寬和容錯的匯總  
- SMB 直接傳輸-新增 RDMA 網路支援以提供非常高效能，並具有低延遲和低 CPU 使用率    
- 加密–提供端對端加密，並保護不受信任網路上的竊聽    
- 目錄租用-透過快取改善分公司中的應用程式回應時間    
- 效能優化-針對小型隨機讀取/寫入 i/o 的優化

##  <a name="more-information"></a>詳細資訊

SMBv2 通訊協定是在 Windows Vista 和 Windows Server 2008 中引進。

SMBv3 通訊協定是在 Windows 8 和 Windows Server 2012 中引進。

如需 SMBv2 和 SMBv3 功能功能的詳細資訊，請參閱下列文章：

[伺服器訊息區概觀](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11))

[SMB 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff625695(v=ws.10))  

## <a name="how-to-gracefully-remove-smb-v1-in-windows-81-windows-10-windows-2012-r2-and-windows-server-2016"></a>如何在 Windows 8.1、Windows 10、Windows 2012 R2 和 Windows Server 2016 中正常移除 SMB v1

#### <a name="windows-server-2012-r2--2016-powershell-methods"></a>Windows Server 2012 R2 & 2016： PowerShell 方法

##### <a name="smb-v1"></a>SMB v1

- 是否 

  ```PowerShell
  Get-WindowsFeature FS-SMB1
  ```

- 啟用 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

- 啟用 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

##### <a name="smb-v2v3"></a>SMB v2/v3

- 是否
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- 啟用

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- 啟用

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true 
  ```

#### <a name="windows-server-2012-r2-and-windows-server-2016-server-manager-method-for-disabling-smb"></a>Windows Server 2012 R2 和 Windows Server 2016：用來停用 SMB 的伺服器管理員方法

##### <a name="smb-v1"></a>SMB v1

![伺服器管理員-儀表板方法](media/detect-enable-and-disable-smbv1-v2-v3-1.png)

#### <a name="windows-81-and-windows-10-powershell-method"></a>Windows 8.1 和 Windows 10： PowerShell 方法

##### <a name="smb-v1-protocol"></a>SMB v1 通訊協定

- 是否 
  
  ```PowerShell
  Get-WindowsOptionalFeature –Online –FeatureName SMB1Protocol
  ```

- 啟用 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

- 啟用 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

##### <a name="smb-v2v3protocol"></a>SMB v2/v3 通訊協定

- 是否 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- 啟用 
  
  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $false
  ```

- 啟用

  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $true
  ```

#### <a name="windows-81-and-windows-10-add-or-remove-programs-method"></a>Windows 8.1 和 Windows 10：新增或移除程式方法

![[新增-移除程式] 用戶端方法](media/detect-enable-and-disable-smbv1-v2-v3-2.png)

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-server"></a>如何在 SMB 伺服器上偵測狀態、啟用和停用 SMB 通訊協定

### <a name="for-windows-8-and-windows-server-2012"></a>適用于 Windows 8 和 Windows Server 2012

Windows 8 和 Windows Server 2012 引進了新的**SMBServerConfiguration** Windows PowerShell Cmdlet。 此 Cmdlet 可讓您在伺服器元件上啟用或停用 SMBv1、SMBv2 和 SMBv3 通訊協定。  

> [!NOTE]   
> 當您啟用或停用 Windows 8 或 Windows Server 2012 中的 SMBv2 時，也會啟用或停用 SMBv3。 之所以會發生這種行為，是因為這些通訊協定共用相同的堆疊。     

您不需要在執行**SMBServerConfiguration** Cmdlet 之後重新開機電腦。 

##### <a name="smb-v1-on-smb-server"></a>SMB 伺服器上的 SMB v1

- 是否 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB1Protocol
  ```

- 啟用 

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $false
  ```

- 啟用 
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $true
  ```

如需詳細資訊，請參閱[Microsoft 的伺服器儲存體](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Stop-using-SMB1/ba-p/425858)。 
##### <a name="smb-v2v3-on-smb-server"></a>SMB 伺服器上的 SMB v2/v3

- 是否

  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- 啟用 
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- 啟用
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true
  ```

### <a name="for-windows-7-windows-server-2008-r2-windows-vista-and-windows-server-2008"></a>適用于 Windows 7、Windows Server 2008 R2、Windows Vista 和 Windows Server 2008

若要在 runningWindows 7、Windows Server 2008 R2、Windows Vista 或 Windows Server 2008 的 SMB 伺服器上啟用或停用 SMB 通訊協定，請使用 Windows PowerShell 或登錄編輯程式。 

#### <a name="powershell-methods"></a>PowerShell 方法

> [!NOTE]
> 此方法需要 powershell 2.0 或更新版本的 PowerShell。

##### <a name="smb-v1-on-smb-server"></a>SMB 伺服器上的 SMB v1

是否

```PowerShell
Get-Item HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}
```

預設設定 = [已啟用] （不會建立登錄機碼），因此不會傳回任何 SMB1 值

啟用

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force
```

啟用  

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 1 –Force
```  

**注意**進行這些變更之後，您必須重新開機電腦。 如需詳細資訊，請參閱[Microsoft 的伺服器儲存體](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/)。 
##### <a name="smb-v2v3-on-smb-server"></a>SMB 伺服器上的 SMB v2/v3

是否  

```PowerShell
Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath} 
```

啟用

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 0 –Force  
```

啟用

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 1 –Force 
```

> [!NOTE]
> 進行這些變更之後，您必須重新開機電腦。 

#### <a name="registry-editor"></a>登錄編輯程式

> [!IMPORTANT]
> 請仔細依循本節中的步驟。 若您不正確地修改登錄，可能會發生嚴重的問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。
 
若要啟用或停用 SMB 伺服器上的 SMBv1，請設定下列登錄機碼：

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB1
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created)
```

若要啟用或停用 SMB 伺服器上的 SMBv2，請設定下列登錄機碼： 

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB2
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created) 
```

> [!NOTE]
> 您必須在進行這些變更之後重新開機電腦。 

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-client"></a>如何在 SMB 用戶端上偵測狀態、啟用和停用 SMB 通訊協定

### <a name="for-windows-vista-windows-server-2008-windows-7-windows-server-2008-r2-windows-8-and-windows-server-2012"></a>適用于 Windows Vista、Windows Server 2008、Windows 7、Windows Server 2008 R2、Windows 8 和 Windows Server 2012

> [!NOTE]
> 當您啟用或停用 Windows 8 或 Windows Server 2012 中的 SMBv2 時，也會啟用或停用 SMBv3。 之所以會發生這種行為，是因為這些通訊協定共用相同的堆疊。 

##### <a name="smb-v1-on-smb-client"></a>SMB 用戶端上的 SMB v1

- Detect
  
  ```cmd
  sc.exe qc lanmanworkstation
  ```

- 啟用
  
  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= disabled
  ```

- 啟用

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= auto
  ```

如需詳細資訊，請參閱[Microsoft 的伺服器儲存體](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/) 
##### <a name="smb-v2v3-on-smb-client"></a>Smb 用戶端上的 SMB v2/v3

- 是否

  ```cmd
  sc.exe qc lanmanworkstation
  ```

- 啟用

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/nsi
  sc.exe config mrxsmb20 start= disabled 
  ```

- 啟用

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb20 start= auto
  ```

> [!NOTE]
> - 您必須在提高許可權的命令提示字元中執行這些命令。    
> - 進行這些變更之後，您必須重新開機電腦。    
 

## <a name="disable-smbv1-server-with-group-policy"></a>使用群組原則停用 SMBv1 伺服器

此程式會在登錄中設定下列新專案：

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters** 

- 登錄專案： **SMB1** 
- REG_DWORD： **0** = 已停用   

若要使用群組原則來進行此設定，請遵循下列步驟：
 
1. 開啟 [群組原則管理主控台]。 在應包含新的喜好設定項目之群組原則物件 (GPO) 上按一下滑鼠右鍵，然後按一下 [編輯]。

2. 在 [**電腦**設定] 底下的主控台樹中，展開 [**喜好**設定] 資料夾，然後展開 [ **Windows 設定**] 資料夾。

3. 以滑鼠右鍵按一下 **[登錄**] 節點，指向 [**新增**]，然後選取 [登錄**專案**]。

   ![登錄-新增-登錄專案](media/detect-enable-and-disable-smbv1-v2-v3-3.png)    
 
在 [**新增**登錄內容] 對話方塊中，選取下列各項： 
 
- **動作**：建立    
- **Hive**： HKEY_LOCAL_MACHINE    
- **金鑰路徑**： SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters    
- **值名稱**： SMB1    
- **數值型別**： REG_DWORD    
- **數值資料**：0    
 
![新的登錄內容-一般](media/detect-enable-and-disable-smbv1-v2-v3-4.png)

這會停用 SMBv1 伺服器元件。 此群組原則必須套用至網域中所有必要的工作站、伺服器和網域控制站。

> [!NOTE]
> [WMI 篩選器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc947846(v=ws.10))也可以設定為排除不支援的作業系統或選取的排除專案，例如 Windows XP。

> [!IMPORTANT]
> 當您在舊版 Windows XP 或舊版 Linux 和協力廠商系統（不支援 SMBv2 或 SMBv3）的網域控制站上進行這些變更時，請務必小心，需要存取 SYSVOL 或停用 SMB v1 的其他檔案共用。     

## <a name="disable-smbv1-client-with-group-policy"></a>使用群組原則停用 SMBv1 用戶端

若要停用 SMBv1 用戶端，必須更新服務登錄機碼，以停用**MRxSMB10**的開頭，然後必須從**LanmanWorkstation**的專案中移除對**MRxSMB10**的相依性，讓它可以正常啟動，而不需要先啟動**MRxSMB10** 。

這會更新並取代登錄中下列2個專案的預設值： 

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\services\mrxsmb10** 

登錄專案：**啟動**REG_DWORD： **4**= 已停用

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanWorkstation** 

登錄專案： **DependOnService** REG_MULTI_SZ： **"Bowser"、"MRxSmb20"、"NSI"**   

> [!NOTE]
> 預設包含的 MRxSMB10，現在已移除為相依性。

若要使用群組原則來進行此設定，請遵循下列步驟：
 
1. 開啟 [群組原則管理主控台]。 在應包含新的喜好設定項目之群組原則物件 (GPO) 上按一下滑鼠右鍵，然後按一下 [編輯]。

2. 在 [**電腦**設定] 底下的主控台樹中，展開 [**喜好**設定] 資料夾，然後展開 [ **Windows 設定**] 資料夾。

3. 以滑鼠右鍵按一下 **[登錄**] 節點，指向 [**新增**]，然後選取 [登錄**專案**]。    

4. 在 [**新增**登錄內容] 對話方塊中，選取下列各項： 
 
   - **動作**：更新
   - **Hive**： HKEY_LOCAL_MACHINE
   - **金鑰路徑**： SYSTEM\CurrentControlSet\services\mrxsmb10
   - **值名稱**： Start
   - **數值型別**： REG_DWORD
   - **數值資料**：4
 
   ![啟動屬性-一般](media/detect-enable-and-disable-smbv1-v2-v3-5.png)

5. 然後移除剛停用之**MRxSMB10**的相依性。

   在 [**新增**登錄內容] 對話方塊中，選取下列各項： 
 
   - **動作**：取代
   - **Hive**： HKEY_LOCAL_MACHINE
   - **金鑰路徑**： SYSTEM\CurrentControlSet\Services\LanmanWorkstation
   - **值名稱**： DependOnService
   - **數值型別**： REG_MULTI_SZ 
   - **數值資料**：
      - Bowser
      - MRxSmb20
      - NSI
 
   > [!NOTE]
   > 這三個字串不會有專案符號（請參閱下列螢幕擷取畫面）。

   ![DependOnService 屬性](media/detect-enable-and-disable-smbv1-v2-v3-6.png) 

   預設值包含許多 Windows 版本中的**MRxSMB10** ，因此藉由使用這個多重值字串來取代它們，它實際上是將**MRxSMB10**移除為**LanmanServer**的相依性，並從四個預設值減少為上述三個值。

   > [!NOTE]
   > 當您使用群組原則管理主控台時，不需要使用引號或逗號。 只要在個別行上輸入每個專案。

6. 重新開機目標系統，以完成 SMB v1 的停用。

### <a name="summary"></a>摘要

如果所有設定都位於相同的群組原則物件（GPO）中，群組原則管理會顯示下列設定。

![群組原則管理編輯器-登錄](media/detect-enable-and-disable-smbv1-v2-v3-7.png) 

### <a name="testing-and-validation"></a>測試和驗證

這些設定完成後，允許原則進行複寫和更新。 如需測試，請在命令提示字元中執行**gpupdate/force** ，然後檢查目的電腦，以確保正確套用登錄設定。 請確定 SMB v2 和 SMB v3 適用于環境中的所有其他系統。

> [!NOTE]
> 別忘了重新開機目標系統。
