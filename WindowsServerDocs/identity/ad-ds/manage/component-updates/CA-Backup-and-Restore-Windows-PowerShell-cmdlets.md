---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: CA 備份與還原 Windows PowerShell Cmdlet
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 817a4c117bfd39799a5147d657262eb208c9a79b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943443"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>CA 備份與還原 Windows PowerShell Cmdlet

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
>
> **作者**： Justin Turner，具備 Windows 群組的資深支援提升工程師
>
> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

## <a name="overview"></a>概觀
ADCSAdministration Windows PowerShell 模組是在 Window Server 2012 引進。  Windows Server 2012 R2 中的這個模組已加入兩個新的 Cmdlet，以支援 CA 的備份與還原。

-   Backup-CARoleService

-   Restore-CARoleService

## <a name="backup-caroleservice"></a>Backup-CARoleService
**表 SEQ 資料表 \\ \* 阿拉伯文17：備份和還原 Windows PowerShell Cmdlet**

**ADCSAdministration Cmdlet：備份-Backup-caroleservice**

|Arguments-需要**粗體**引數|描述|
|------------------------------------------------|---------------|
|**-Path**|-String-用來儲存備份的位置<br />-這是唯一未命名的參數<br />-位置參數<p>**範例︰**<p>備份-Backup-caroleservice.-Path c:\adcsbackup1<p>備份-Backup-caroleservice c:\adcsbackup2|
|-KeyOnly|-備份沒有資料庫的 CA 憑證<p>**範例︰**<p>備份-Backup-caroleservice c:\adcsbackup3-KeyOnly|
|-Password|-指定用來保護 CA 憑證和私密金鑰的密碼<br />-必須是安全字串<br />-使用-DatabaseOnly 參數時無效<p>範例：<p>備份-Backup-caroleservice c:\adcsbackup4-密碼 (的讀取-主機提示字元 "Password："-AsSecureString) <p>備份-Backup-caroleservice c:\adcsbackup5-Password (Convertto-html-SecureString "Pa55w0rd！" -AsPlainText-Force) |
|-DatabaseOnly|-備份不含 CA 憑證的資料庫<p>備份-Backup-caroleservice c:\adcsbackup6-DatabaseOnly|
|-Force|1. 允許您覆寫在-Path 參數所指定的位置中 preexists 的備份<p>備份-Backup-caroleservice c:\adcsbackup1-Force|
|-增量|-執行增量備份<p>備份-Backup-caroleservice c:\adcsbackup7-增量|
|-KeepLog|1. 指示命令保留記錄檔。 如果未指定參數，預設會截斷記錄檔，但不包括增量案例<p>備份-Backup-caroleservice c:\adcsbackup7-KeepLog|

### <a name="-password-secure-string"></a>-Password <Secure String>
如果使用-Password 參數，則提供的密碼必須是安全字串。  使用「**讀取主機**」 Cmdlet 來啟動安全密碼輸入的互動式提示，或使用**convertto-html-SecureString**指令程式來指定內嵌的密碼。

請參閱下列範例

**使用讀取主機指定密碼參數的安全字串**

```powershell
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)
```

**使用 Convertto-html 為 Password 參數指定安全字串-SecureString**

```powershell
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)
```

## <a name="restore-caroleservice"></a>Restore-CARoleService
**ADCSAdministration Cmdlet： Restore-Backup-caroleservice**

|Arguments-需要**粗體**引數|描述|
|------------------------------------------------|---------------|
|**-Path**|-字串-要從中還原備份的位置<br />-這是唯一未命名的參數<br />-位置參數<p>**範例︰**<p>Restore-Backup-caroleservice。-Path c:\adcsbackup1-Force<p>還原-Backup-caroleservice c:\adcsbackup2-Force|
|-KeyOnly|-還原沒有資料庫的 CA 憑證<br />-如果備份是使用-KeyOnly 選項所建立，則必須指定<p>**範例︰**<p>還原-Backup-caroleservice c:\adcsbackup3-KeyOnly-Force|
|-Password|-指定 CA 憑證和私密金鑰的密碼<br />-必須是安全字串<p>**範例︰**<p>還原-Backup-caroleservice c:\adcsbackup4-密碼 (的讀取-主機提示字元 "Password："-AsSecureString) -Force<p>Restore-Backup-caroleservice c:\adcsbackup5-Password (Convertto-html-SecureString "Pa55w0rd！" -AsPlainText-Force) -Force|
|-DatabaseOnly|-在沒有 CA 憑證的情況下還原資料庫<p>Restore-Backup-caroleservice c:\adcsbackup6-DatabaseOnly|
|-Force|-可讓您覆寫預先存在的金鑰<br />-是選擇性參數，但在就地還原時，可能需要<p>還原-Backup-caroleservice c:\adcsbackup1-Force|

### <a name="issues"></a>問題
如果 Convertto-html-SecureString 函式在使用 Backup-caroleservice 搭配-Password 參數時失敗，則會採用非密碼保護的備份。

![CA 備份與還原](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)

**資料表 SEQ 資料表 \\ \* 阿拉伯文18：常見錯誤**

|動作|錯誤|註解|
|----------|---------|-----------|
|**還原-Backup-caroleservice C:\ADCSBackup**|Backup-caroleservice：進程無法存取檔案，因為另一個進程正在使用該檔案。 (發生例外狀況於 HRESULT:<p>0x80070020) |執行 Backup-caroleservice Cmdlet 之前，請先停止 Active Directory 憑證服務服務|
|**還原-Backup-caroleservice C:\ADCSBackup**|Restore-Backup-caroleservice：目錄不是空的。 來自 HRESULT 的 (例外狀況： 0x80070091) |使用-Force 參數來覆寫預先存在的金鑰|
|**備份-Backup-caroleservice C:\ADCSBackup-密碼 (的讀取-主機提示字元 "Password："-AsSecureString) -DatabaseOnly**|Backup-caroleservice：無法使用指定的具名引數解析參數集。|-Password 參數只能用來保護私密金鑰，因此當您未進行備份時，就會無效|
|**還原-Backup-caroleservice C:\ADCSBack15-密碼 (的讀取-主機提示字元 "Password："-AsSecureString) -DatabaseOnly**|Restore-Backup-caroleservice：無法使用指定的具名引數解析參數集。|-Password 參數只能用來保護私密金鑰，因此當您未進行還原時，它會是不正確|
|**還原-Backup-caroleservice C:\ADCSBack14-密碼 (的讀取-主機提示字元 "Password："-AsSecureString) **|Restore-Backup-caroleservice：系統找不到指定的檔案。 來自 HRESULT 的 (例外狀況： 0x80070002) |指定的路徑不包含有效的資料庫備份。  可能是路徑無效，或備份是使用-KeysOnly 選項所建立的嗎？|

## <a name="additional-resources"></a>其他資源
[Active Directory 憑證服務移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10))

[備份 CA 資料庫和私密金鑰](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_BackUpDB)

[在目的地伺服器上還原 CA 資料庫和設定](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_RestoreCA)

## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>試試看：在您的實驗室中使用 Windows PowerShell 備份 CA

1.  使用本課程中的命令，備份以密碼保護的 CA 資料庫和私密金鑰。

2.  請隨時關閉 CA 的還原。

