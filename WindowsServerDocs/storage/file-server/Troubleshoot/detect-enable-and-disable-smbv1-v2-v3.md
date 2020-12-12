---
title: 如何在 Windows 中偵測、啟用和停用 SMBv1、Smbv2 弱點和 SMBv3
description: 描述如何在 Windows 用戶端和伺服器環境中啟用和停用伺服器訊息區通訊協定 (SMBv1、Smbv2 弱點和 SMBv3) 。
author: Deland-Han
manager: dcscontentpm
ms.topic: how-to
ms.author: delhan
ms.date: 10/29/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: 6ff7fe56cdb95bb35ccce39abdd800f9665d86dc
ms.sourcegitcommit: 36559331b20b7965a95fe26cb19a635f6e4a9723
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2020
ms.locfileid: "97364249"
---
# <a name="how-to-detect-enable-and-disable-smbv1-smbv2-and-smbv3-in-windows"></a>如何在 Windows 中偵測、啟用和停用 SMBv1、Smbv2 弱點和 SMBv3

>適用于： Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本文說明如何在 SMB 用戶端和伺服器元件上啟用和停用伺服器訊息區 (SMB) 第1版 (SMBv1) 、SMB 第2版 (Smbv2 弱點) 和 SMB 第3版 (SMBv3) 。

停用或移除 SMBv1 可能會導致某些舊版電腦或軟體的相容性問題，SMBv1 有嚴重的安全性弱點，因此 [強烈建議您不要使用它](https://techcommunity.microsoft.com/t5/storage-at-microsoft/stop-using-smb1/ba-p/425858)。

## <a name="disabling-smbv2-or-smbv3-for-troubleshooting"></a>停用 Smbv2 弱點或 SMBv3 以進行疑難排解

雖然我們建議您讓 Smbv2 弱點和 SMBv3 保持啟用狀態，但在 [SMB 伺服器上偵測狀態、啟用和停用 smb 通訊協定](detect-enable-and-disable-smbv1-v2-v3.md#how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-server)中所述，您可能會發現暫時停用其中一個來進行疑難排解會很有用。

在 Windows 10、Windows 8.1 和 Windows 8、Windows server 2019、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中，停用 SMBv3 會停用下列功能 (以及前述清單) 中所述的 Smbv2 弱點功能：

- 透明容錯移轉-在維護或容錯移轉期間，用戶端重新連線而不中斷叢集節點
- Scale Out –平行存取所有檔案叢集節點上的共用資料 
- 多重通道-如果用戶端與伺服器之間有多個路徑，則會匯總網路頻寬和容錯能力
- SMB 直接存取-新增具有低延遲和低 CPU 使用率之高效能的 RDMA 網路支援
- 加密-提供端對端加密，並防止無法信任的網路竊聽
- 目錄租用-透過快取改善分公司的應用程式回應時間
- 效能優化-小型隨機讀取/寫入 i/o 的優化

在 Windows 7 和 Windows Server 2008 R2 中，停用 Smbv2 弱點會停用下列功能：

- 要求複合-允許將多個 SMB 2 要求當作單一網路要求傳送
- 更大的讀取和寫入-更快速地使用網路
- 快取資料夾和檔內容-用戶端保留資料夾和檔案的本機副本
- 持久的控制碼-如果暫時中斷連線，則允許連線以透明的方式重新連接到伺服器
- 改進的訊息簽署-HMAC SHA-256 取代 MD5 作為雜湊演算法
- 改進檔案共用的擴充性：每個伺服器的使用者、共用和開啟檔案數目大幅增加
- 支援符號連結
- 用戶端 oplock 租用模型-限制用戶端與伺服器之間傳輸的資料，改善高延遲網路的效能，並增加 SMB 伺服器的擴充性
- 大型 MTU 支援-可充分利用 10 gigabye (GB) 乙太網路
- 提升能源效率-將檔案開啟至伺服器的用戶端可能會睡眠

Smbv2 弱點通訊協定是在 Windows Vista 和 Windows Server 2008 中引進，而 SMBv3 通訊協定是在 Windows 8 和 Windows Server 2012 引進。 如需 Smbv2 弱點和 SMBv3 功能功能的詳細資訊，請參閱下列文章：

[伺服器訊息區概觀](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v=ws.11))

[SMB 的新功能](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff625695(v=ws.10))

## <a name="how-to-remove-smb-v1"></a>如何移除 SMB v1

以下是如何在 Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016 和 Windows 2012 R2 中移除 SMBv1。

#### <a name="powershell-methods"></a>PowerShell 方法

##### <a name="smb-v1-client-and-server"></a>SMB v1 (用戶端和伺服器) 

- 檢測：

  ```PowerShell
  Get-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

- 禁用：

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

- 啟用：

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

#### <a name="windows-server-2012-r2-windows-server-2016-windows-server-2019-server-manager-method-for-disabling-smb"></a>Windows Server 2012 R2、Windows Server 2016、Windows Server 2019：停用 SMB 的伺服器管理員方法

##### <a name="smb-v1"></a>SMB v1

![伺服器管理員-儀表板方法](media/detect-enable-and-disable-smbv1-v2-v3-1.png)

#### <a name="windows-81-and-windows-10-powershell-method"></a>Windows 8.1 和 Windows 10： PowerShell 方法

##### <a name="smb-v1-protocol"></a>SMB v1 通訊協定

- 檢測：

  ```PowerShell
  Get-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

- 禁用：

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

- 啟用：

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

##### <a name="smb-v2v3-protocol-only-disables-smb-v2v3-server"></a>SMB v2/v3 通訊協定 (只會停用 SMB v2/v3 伺服器) 

- 檢測：

  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- 禁用：

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- 啟用：

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true
  ```

#### <a name="windows-81-and-windows-10-add-or-remove-programs-method"></a>Windows 8.1 和 Windows 10：新增或移除程式方法

![Add-Remove 程式用戶端方法](media/detect-enable-and-disable-smbv1-v2-v3-2.png)

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-server"></a>如何在 SMB 伺服器上偵測狀態、啟用和停用 SMB 通訊協定

### <a name="for-windows-8-and-windows-server-2012"></a>針對 Windows 8 和 Windows Server 2012

Windows 8 和 Windows Server 2012 引進了新的 **SMBServerConfiguration** Windows PowerShell Cmdlet。 此 Cmdlet 可讓您啟用或停用伺服器元件上的 SMBv1、Smbv2 弱點和 SMBv3 通訊協定。 

> [!NOTE]
> 當您在 Windows 8 或 Windows Server 2012 中啟用或停用 Smbv2 弱點時，也會啟用或停用 SMBv3。 發生此行為的原因是這些通訊協定共用相同的堆疊。

執行 **SMBServerConfiguration** 指令 Cmdlet 之後，您就不需要重新開機電腦。

##### <a name="smb-v1-on-smb-server"></a>Smb 伺服器上的 SMB v1

- 檢測：

  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB1Protocol
  ```

- 禁用：

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $false
  ```

- 啟用：
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $true
  ```

如需詳細資訊，請參閱 [Microsoft 的伺服器儲存體](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Stop-using-SMB1/ba-p/425858)。
##### <a name="smb-v2v3-on-smb-server"></a>Smb 伺服器上的 SMB v2/v3

- 檢測：

  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- 禁用：

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- 啟用：

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true
  ```

### <a name="for-windows-7-windows-server-2008-r2-windows-vista-and-windows-server-2008"></a>適用于 Windows 7、Windows Server 2008 R2、Windows Vista 和 Windows Server 2008

若要在執行 Windows 7、Windows Server 2008 R2、Windows Vista 或 Windows Server 2008 的 SMB 伺服器上啟用或停用 SMB 通訊協定，請使用 Windows PowerShell 或登錄編輯程式。

#### <a name="powershell-methods"></a>PowerShell 方法

> [!NOTE]
> 此方法需要 PowerShell 2.0 或更新版本的 PowerShell。

##### <a name="smb-v1-on-smb-server"></a>Smb 伺服器上的 SMB v1

檢測：

```PowerShell
Get-Item HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}
```

預設設定 = 已啟用 () 不會建立任何登錄機碼，因此不會傳回任何 SMB1 值

禁用：

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 -Force
```

啟用：

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 1 -Force
```

**注意** 進行這些變更之後，您必須重新開機電腦。
如需詳細資訊，請參閱 [Microsoft 的伺服器儲存體](https://techcommunity.microsoft.com/t5/storage-at-microsoft/stop-using-smb1/ba-p/425858)。
##### <a name="smb-v2v3-on-smb-server"></a>Smb 伺服器上的 SMB v2/v3

檢測：

```PowerShell
Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}
```

禁用：

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 0 -Force
```

啟用：

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 1 -Force
```

> [!NOTE]
> 進行這些變更之後，您必須重新開機電腦。

#### <a name="registry-editor"></a>登錄編輯器

> [!IMPORTANT]
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。

若要在 SMB 伺服器上啟用或停用 SMBv1，請設定下列登錄機碼：

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB1
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created)
```

若要在 SMB 伺服器上啟用或停用 Smbv2 弱點，請設定下列登錄機碼：

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB2
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created)
```

> [!NOTE]
> 進行這些變更之後，您必須重新開機電腦。

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-client"></a>如何在 SMB 用戶端上偵測狀態、啟用和停用 SMB 通訊協定

### <a name="for-windows-vista-windows-server-2008-windows-7-windows-server-2008-r2-windows-8-and-windows-server-2012"></a>適用于 Windows Vista、Windows Server 2008、Windows 7、Windows Server 2008 R2、Windows 8 和 Windows Server 2012

> [!NOTE]
> 當您在 Windows 8 或 Windows Server 2012 中啟用或停用 Smbv2 弱點時，也會啟用或停用 SMBv3。 發生此行為的原因是這些通訊協定共用相同的堆疊。

##### <a name="smb-v1-on-smb-client"></a>Smb 用戶端上的 SMB v1

- Detect

  ```cmd
  sc.exe qc lanmanworkstation
  ```

- 禁用：

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= disabled
  ```

- 啟用：

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= auto
  ```

如需詳細資訊，請參閱 [Microsoft 的伺服器儲存體](https://techcommunity.microsoft.com/t5/storage-at-microsoft/stop-using-smb1/ba-p/425858)
##### <a name="smb-v2v3-on-smb-client"></a>Smb 用戶端上的 SMB v2/v3

- 檢測：

  ```cmd
  sc.exe qc lanmanworkstation
  ```

- 禁用：

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/nsi
  sc.exe config mrxsmb20 start= disabled
  ```

- 啟用：

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb20 start= auto
  ```

> [!NOTE]
> - 您必須在提高許可權的命令提示字元中執行這些命令。
> - 進行這些變更之後，您必須重新開機電腦。


## <a name="disable-smbv1-server-with-group-policy"></a>使用群組原則停用 SMBv1 伺服器

此程式會在登錄中設定下列新專案：

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

- 登錄專案： **SMB1**
- REG_DWORD： **0** = 已停用

若要使用群組原則來進行設定，請遵循下列步驟：

1. 開啟 [群組原則管理主控台]。 在應包含新的喜好設定項目之群組原則物件 (GPO) 上按一下滑鼠右鍵，然後按一下 [編輯]。

2. 在 [ **電腦** 設定] 底下的主控台樹中，展開 [ **喜好** 設定] 資料夾，然後展開 [ **Windows 設定** ] 資料夾。

3. 以滑鼠右鍵按一下 **[登錄** ] 節點，指向 [ **新增**]，然後選取 [登錄 **專案**]。

   ![登錄-新增-登錄專案](media/detect-enable-and-disable-smbv1-v2-v3-3.png)

在 [ **新** 登錄內容] 對話方塊中，選取下列各項：

- **動作**：建立
- **Hive**： HKEY_LOCAL_MACHINE
- 機 **碼路徑**： SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters
- **值名稱**： SMB1
- **數值型別**： REG_DWORD
- **值資料**：0

![新登錄屬性-一般](media/detect-enable-and-disable-smbv1-v2-v3-4.png)

這會停用 SMBv1 伺服器元件。 此群組原則必須套用到網域中所有必要的工作站、伺服器和網域控制站。

> [!NOTE]
> 您也可以將[WMI 篩選器](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc947846(v=ws.10))設定為排除不支援的作業系統或選取的排除專案，例如 Windows XP。

> [!IMPORTANT]
> 當您在舊版 Windows XP 或舊版 Linux 和協力廠商系統 (不支援 Smbv2 弱點或 SMBv3 的網域控制站上進行這些變更時，請務必小心) 需要存取 SYSVOL 或停用 SMB v1 的其他檔案共用。

## <a name="disable-smbv1-client-with-group-policy"></a>使用群組原則停用 SMBv1 用戶端

若要停用 SMBv1 用戶端，必須更新服務登錄機碼，以停用 **MRxSMB10** 的開頭，然後必須從 **LanmanWorkstation** 的專案中移除對 **MRxSMB10** 的相依性，如此一來，就能在不需要 **MRxSMB10** 的情況下啟動。

這會更新並取代登錄中下列兩個專案的預設值：

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\mrxsmb10**

登錄專案： **啟動** REG_DWORD： **4**= 已停用

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation**

登錄專案： **DependOnService** REG_MULTI_SZ： **"Bowser"、"MRxSmb20"、"NSI"**

> [!NOTE]
> 預設包含的 MRxSMB10，現在已移除為相依性。

若要使用群組原則來進行設定，請遵循下列步驟：

1. 開啟 [群組原則管理主控台]。 在應包含新的喜好設定項目之群組原則物件 (GPO) 上按一下滑鼠右鍵，然後按一下 [編輯]。

2. 在 [ **電腦** 設定] 底下的主控台樹中，展開 [ **喜好** 設定] 資料夾，然後展開 [ **Windows 設定** ] 資料夾。

3. 以滑鼠右鍵按一下 **[登錄** ] 節點，指向 [ **新增**]，然後選取 [登錄 **專案**]。

4. 在 [ **新** 登錄內容] 對話方塊中，選取下列各項：

   - **動作**：更新
   - **Hive**： HKEY_LOCAL_MACHINE
   - 機 **碼路徑**： SYSTEM\CurrentControlSet\services\mrxsmb10
   - **值名稱**：啟動
   - **數值型別**： REG_DWORD
   - **值資料**：4

   ![啟動屬性-一般](media/detect-enable-and-disable-smbv1-v2-v3-5.png)

5. 然後移除剛停用之 **MRxSMB10** 的相依性。

   在 [ **新** 登錄內容] 對話方塊中，選取下列各項：

   - **動作**：取代
   - **Hive**： HKEY_LOCAL_MACHINE
   - 機 **碼路徑**： SYSTEM\CurrentControlSet\Services\LanmanWorkstation
   - **值名稱**： DependOnService
   - **數值型別**： REG_MULTI_SZ
   - **值資料**：
      - Bowser
      - MRxSmb20
      - NSI

   > [!NOTE]
   > 這三個字串不會有專案符號 (請參閱下列螢幕擷取畫面) 。

   ![DependOnService 屬性](media/detect-enable-and-disable-smbv1-v2-v3-6.png)

   預設值會在許多版本的 Windows 中包含 **MRxSMB10** ，因此藉由使用這個多重值字串來取代它們，它實際上會將 **MRxSMB10** 做為 **LanmanServer** 的相依性，並從四個預設值移至上述三個值。

   > [!NOTE]
   > 當您使用群組原則管理主控台時，不需要使用引號或逗號。 只需在個別行上輸入每個專案。

6. 重新開機目標系統，以完成停用 SMB v1。

### <a name="auditing-smbv1-usage"></a>審核 SMBv1 使用方式

若要判斷哪些用戶端嘗試使用 SMBv1 連接到 SMB 伺服器，您可以在 Windows Server 2016、Windows 10 和 Windows Server 2019 上啟用審核。 您也可以在 Windows 7 和 Windows Server 2008 R2 上進行審核，如果他們安裝了5月2018日更新，以及在 Windows 8、Windows 8.1、Windows Server 2012 和 Windows Server 2012 R2 上，如果他們安裝了7月的2017每月更新。

- 啟用：

  ```PowerShell
  Set-SmbServerConfiguration -AuditSmb1Access $true
  ```

- 禁用：

  ```PowerShell
  Set-SmbServerConfiguration -AuditSmb1Access $false
  ```

- 檢測：

  ```PowerShell
  Get-SmbServerConfiguration | Select AuditSmb1Access
  ```

啟用 SMBv1 審核時，事件3000會出現在 "Microsoft-Windows-SMBServer\Audit" 事件記錄檔中，識別嘗試與 SMBv1 連接的每個用戶端。

### <a name="summary"></a>摘要

如果所有設定都位於相同的群組原則物件 (GPO) 中，群組原則管理] 會顯示下列設定。

![群組原則管理編輯器-Registry](media/detect-enable-and-disable-smbv1-v2-v3-7.png)

### <a name="testing-and-validation"></a>測試和驗證

設定好之後，請允許原則進行複寫和更新。 如有需要，請在命令提示字元中執行 **gpupdate/force** ，然後檢查目的電腦以確定已正確套用登錄設定。 請確定 SMB v2 和 SMB v3 適用于環境中的所有其他系統。

> [!NOTE]
> 別忘了重新開機目標系統。
