---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: CA 備份與還原 Windows PowerShell cmdlet
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6055d9b694f72a6a874acdcb5135fde61bcf0d76
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442757"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>CA 備份與還原 Windows PowerShell cmdlet

> 適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012
> 
> **作者**:Justin Turner 資深支援高階工程師，與 Windows 群組  
> 
> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
## <a name="overview"></a>總覽  
Window Server 2012 中引進了 ADCSAdministration Windows PowerShell 模組。  此模組支援的備份和還原 CA 的 Window Server 2012 R2 中已加入兩個新的 cmdlet。  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**資料表 SEQ 表格\\\*阿拉伯文 17:備份及還原 Windows PowerShell Cmdlet**  
  
**ADCSAdministration Cmdlet:Backup-CARoleService**  
  
|引數-**粗體**引數是必要|描述|  
|------------------------------------------------|---------------|  
|**-Path**|字串-儲存備份的位置<br />-這是唯一未命名參數<br />為位置參數<br /><br />**範例:**<br /><br />Backup-CARoleService.-Path c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|沒有資料庫的備份 CA 憑證<br /><br />**範例:**<br /><br />Backup-CARoleService c:\adcsbackup3 -KeyOnly|  
|-Password|-指定要保護的 CA 憑證和私密金鑰的密碼<br />-必須是安全字串<br />-使用-DatabaseOnly 參數不正確<br /><br />範例：<br /><br />Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)<br /><br />Backup-caroleservice c:\adcsbackup5-密碼 (Convertto-securestring"Pa55w0rd ！ 」 -AsPlainText -Force)|  
|-DatabaseOnly|備份資料庫，而不將 CA 憑證<br /><br />Backup-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|1.可讓您覆寫備份，在指定的位置 preexists-Path 參數中<br /><br />Backup-CARoleService c:\adcsbackup1 -Force|  
|-Incremental|-執行增量備份<br /><br />Backup-CARoleService c:\adcsbackup7 -Incremental|  
|-KeepLog|1.指示要保留的記錄檔的命令。 如果未指定此參數，除了在累加式案例中的預設值會截斷記錄檔<br /><br />Backup-CARoleService c:\adcsbackup7 -KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
如果使用-密碼參數，所提供的密碼必須是安全字串。  使用  **Read-host** cmdlet 來啟動互動式提示針對安全的密碼項目，或使用**Convertto-securestring**指令程式來指定密碼中內嵌。  
  
檢閱下列的範例  
  
**指定使用 Read-host Password 參數的安全字串**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**指定使用 Convertto-securestring Password 參數的安全字串**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet:Restore-CARoleService**  
  
|引數-**粗體**引數是必要|描述|  
|------------------------------------------------|---------------|  
|**-Path**|字串-從備份還原的位置<br />-這是唯一未命名參數<br />為位置參數<br /><br />**範例:**<br /><br />Restore-CARoleService.-Path c:\adcsbackup1 -Force<br /><br />Restore-CARoleService c:\adcsbackup2 -Force|  
|-KeyOnly|-還原而不需要資料庫的 CA 憑證<br />-如果備份使用-KeyOnly 選項必須指定<br /><br />**範例:**<br /><br />Restore-CARoleService c:\adcsbackup3 -KeyOnly -Force|  
|-Password|-指定的 CA 憑證和私密金鑰的密碼<br />-必須是安全字串<br /><br />**範例:**<br /><br />Restore-CARoleService c:\adcsbackup4 -Password (read-host -prompt "Password:" -AsSecureString) -Force<br /><br />Restore-caroleservice c:\adcsbackup5-密碼 (Convertto-securestring"Pa55w0rd ！ 」 -AsPlainText -Force) -Force|  
|-DatabaseOnly|-還原資料庫，而不將 CA 憑證<br /><br />Restore-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|-可讓您覆寫的預先存在的索引鍵<br />-這是選擇性參數，但在還原時就地，並可能需要<br /><br />Restore-CARoleService c:\adcsbackup1 -Force|  
  
### <a name="issues"></a>問題  
在非密碼受保護備份時使用 Backup-caroleservice Convertto-securestring 函式時使用-密碼參數。  
  
![CA 備份與還原](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**資料表 SEQ 表格\\\*阿拉伯文 18:常見的錯誤**  
  
|動作|錯誤|註解|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-caroleservice:此程序無法存取檔案，因為它正由另一個處理序。 (發生例外狀況於 HRESULT:<br /><br />0x80070020)|停止 Active Directory 憑證服務服務之前執行 Restore-caroleservice cmdlet|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-caroleservice:目錄不是空的。 (發生例外狀況於 HRESULT:0x80070091)|使用-Force 參數覆寫既有的金鑰|  
|**Backup-caroleservice C:\ADCSBackup-密碼 (Read-host-Prompt"的密碼:"-AsSecureString)-DatabaseOnly**|Backup-caroleservice:解析參數集不能使用指定的具名參數。|參數是-密碼僅用密碼來保護私密金鑰，因此無效時您不會備份並|  
|**Restore-CARoleService C:\ADCSBack15 -Password (Read-Host -Prompt "Password:" -AsSecureString) -DatabaseOnly**|Restore-caroleservice:解析參數集不能使用指定的具名參數。|參數是-密碼僅用密碼來保護私密金鑰，因此無效時您不還原它們|  
|**Restore-CARoleService C:\ADCSBack14 -Password (Read-Host -Prompt "Password:" -AsSecureString)**|Restore-caroleservice:系統找不到指定的檔案。 (發生例外狀況於 HRESULT:0x80070002)|指定的路徑不包含有效的資料庫備份。  可能是路徑無效，或備份-KeysOnly 選項嗎？|  
  
## <a name="additional-resources"></a>其他資源  
[Active Directory 憑證服務移轉指南](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[備份 CA 資料庫和私密金鑰](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[在目的地伺服器上還原 CA 資料庫和設定](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>試試看吧：在您的實驗室使用 Windows PowerShell 將 CA 備份  
  
1.  在這一課使用命令，以備份 CA 資料庫和私密金鑰使用密碼保護。  
  
2.  延後還原 CA 的這一次。  
  


