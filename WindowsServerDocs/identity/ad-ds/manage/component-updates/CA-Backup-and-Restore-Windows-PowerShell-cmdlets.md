---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: CA 備份與還原 Windows PowerShell Cmdlet
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1a36e72858e5ea2ae95cc0f1730e5906b3c85866
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070730"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>CA 備份與還原 Windows PowerShell Cmdlet

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
>
> **作者** ： Justin Turner，與 Windows 群組的資深支援擴大工程師
>
> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

## <a name="overview"></a>概觀
ADCSAdministration Windows PowerShell 模組是在 Windows Server 2012 中引進。  Windows Server 2012 R2 中的此課程模組新增了兩個新的 Cmdlet，以支援 CA 的備份和還原。

-   Backup-CARoleService

-   Restore-CARoleService

## <a name="backup-caroleservice"></a>Backup-CARoleService
**表 SEQ 資料表 \\ \* 阿拉伯文17：備份和還原 Windows PowerShell Cmdlet**

**ADCSAdministration Cmdlet：備份-Backup-caroleservice**

|引數-需要 **粗體** 引數|描述|
|------------------------------------------------|---------------|
|**-Path**|-字串-儲存備份的位置<br />-這是唯一未命名的參數<br />-位置參數<p>**範例︰**<p>備份-Backup-caroleservice。-Path c:\adcsbackup1<p>Backup-CARoleService c:\adcsbackup2|
|-KeyOnly|-在沒有資料庫的情況下備份 CA 憑證<p>**範例︰**<p>Backup-CARoleService c:\adcsbackup3-KeyOnly|
|-Password|-指定用來保護 CA 憑證和私密金鑰的密碼<br />-必須是安全字串<br />-DatabaseOnly 參數無效<p>範例：<p>Backup-CARoleService c:\adcsbackup4-Password (讀取主機提示 "Password："-AsSecureString) <p>Backup-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd！" -AsPlainText-Force) |
|-DatabaseOnly|-在沒有 CA 憑證的情況下備份資料庫<p>Backup-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|1. 允許您覆寫在-Path 參數中指定的位置 preexists 的備份。<p>Backup-CARoleService c:\adcsbackup1-Force|
|-增量|-執行增量備份<p>Backup-CARoleService c:\adcsbackup7-增量|
|-KeepLog|1. 指示命令保留記錄檔。 如果未指定參數，則預設會截斷記錄檔，但是累加案例中會截斷記錄檔。<p>Backup-CARoleService c:\adcsbackup7-KeepLog|

### <a name="-password-secure-string"></a>-Password <Secure String>
如果使用-Password 參數，則提供的密碼必須是安全字串。  使用「 **讀取主機** Cmdlet」來啟動安全密碼輸入的互動式提示，或使用 **ConvertTo-SecureString** Cmdlet 來指定內嵌密碼。

請參閱下列範例

**使用讀取主機為 Password 參數指定安全字串**

```powershell
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)
```

**使用 ConvertTo 指定 Password 參數的安全字串-SecureString**

```powershell
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)
```

## <a name="restore-caroleservice"></a>Restore-CARoleService
**ADCSAdministration Cmdlet： Restore-Backup-caroleservice**

|引數-需要 **粗體** 引數|描述|
|------------------------------------------------|---------------|
|**-Path**|-字串-還原備份的來源位置<br />-這是唯一未命名的參數<br />-位置參數<p>**範例︰**<p>Restore-Backup-caroleservice-Path c:\adcsbackup1-Force<p>Restore-CARoleService c:\adcsbackup2-Force|
|-KeyOnly|-在不含資料庫的情況下還原 CA 憑證<br />-如果使用-KeyOnly 選項來執行備份，就必須指定-<p>**範例︰**<p>Restore-CARoleService c:\adcsbackup3-KeyOnly-Force|
|-Password|-指定 CA 憑證和私密金鑰的密碼<br />-必須是安全字串<p>**範例︰**<p>Restore-CARoleService c:\adcsbackup4-Password (讀取主機提示 "Password："-AsSecureString) -Force<p>Restore-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd！" -AsPlainText-Force) -Force|
|-DatabaseOnly|-在沒有 CA 憑證的情況下還原資料庫<p>Restore-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|-可讓您覆寫預先存在的金鑰<br />-是選擇性參數，但就地還原時，可能需要<p>Restore-CARoleService c:\adcsbackup1-Force|

### <a name="issues"></a>問題
如果在使用具有-Password 參數的 Backup-CARoleService 時，ConvertTo-SecureString 函式失敗，則會採用非密碼保護的備份。

![CA 備份與還原](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)

**表 SEQ 資料表 \\ \* 阿拉伯文18：常見錯誤**

|動作|錯誤|註解|
|----------|---------|-----------|
|**Restore-Backup-caroleservice C:\ADCSBackup**|Restore-CARoleService：進程無法存取檔案，因為另一個進程正在使用該檔案。 (發生例外狀況於 HRESULT:<p>0x80070020) |執行 Restore-CARoleService Cmdlet 之前，請先停止 Active Directory 憑證服務服務|
|**Restore-Backup-caroleservice C:\ADCSBackup**|Restore-CARoleService：目錄不是空的。 HRESULT 的 (例外狀況： 0x80070091) |使用-Force 參數來覆寫預先存在的金鑰|
|**備份-Backup-caroleservice C:\ADCSBackup-Password (Read Host-Prompt "Password："-AsSecureString) -DatabaseOnly**|Backup-CARoleService：無法使用指定的具名引數來解析參數集。|-Password 參數只是用來保護私密金鑰的安全，因此當您未備份它們時，就會無效。|
|**Restore-Backup-caroleservice C:\ADCSBack15-Password (Read Host-Prompt "Password："-AsSecureString) -DatabaseOnly**|Restore-CARoleService：無法使用指定的具名引數來解析參數集。|-Password 參數只是用來保護私密金鑰的安全，因此當您未還原私密金鑰時，就會無效。|
|**Restore-Backup-caroleservice C:\ADCSBack14-Password (Read Host-Prompt "Password："-AsSecureString)**|Restore-CARoleService：系統找不到指定的檔案。 HRESULT 的 (例外狀況： 0x80070002) |指定的路徑不包含有效的資料庫備份。  可能是路徑無效，或使用-KeysOnly 選項來執行備份？|

## <a name="additional-resources"></a>其他資源
[Active Directory 憑證服務移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10))

[備份 CA 資料庫和私密金鑰](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_BackUpDB)

[在目的地伺服器上還原 CA 資料庫和設定](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_RestoreCA)

## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>試試看：使用 Windows PowerShell 在您的實驗室中備份 CA

1.  您可以使用本課程中的命令，備份以密碼保護的 CA 資料庫和私密金鑰。

2.  請在此時保留 CA 的還原。

