---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: "CA 備份及還原 Windows PowerShell cmdlet"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aba86ed080cc0b4043805531f0a2138b1b1d3cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>CA 備份及還原 Windows PowerShell cmdlet

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
>
**作者**: Justin Turner 資深支援工程師視窗群組  
  
> [!NOTE]  
> 本文由 Microsoft 客戶支援工程師撰寫，以及適用於系統管理員經驗和系統設計師超過參考 TechNet 上的主題通常會提供深入的技術解釋的功能與 Windows Server 2012 R2 方案正在尋找。 不過，尚未經歷相同編輯行程，以便某些語言的似乎比哪些通常位於 TechNet 較少的外觀。  
  
## <a name="overview"></a>概觀  
在視窗 Server 2012 中引進 ADCSAdministration Windows PowerShell 模組。  有兩個新 cmdlet 已加入此模組在視窗 Server 2012 R2 備份與還原 CA 的支援。  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**表格 7 表格 \\\ * 阿拉伯文 17：備份及還原 Windows PowerShell Cmdlet**  
  
**ADCSAdministration Cmdlet: Backup-CARoleService**  
  
|引數-**粗體**所需的引數|描述|  
|------------------------------------------------|---------------|  
|**路徑**|-字串-位置要儲存的備份<br />-這是唯一參數的未命名<br />-位置參數<br /><br />**範例：**<br /><br />Backup-CARoleService。-Path c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|-備份不資料庫 CA 憑證<br /><br />**範例：**<br /><br />Backup-CARoleService c:\adcsbackup3-KeyOnly|  
|密碼|-指定保護 CA 憑證和私密金鑰密碼<br />-必須安全字串<br />-與-DatabaseOnly 參數不正確<br /><br />範例：<br /><br />Backup-CARoleService c:\adcsbackup4-密碼 (Read-Host-命令提示字元中」的密碼:「-AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5-密碼 (ConvertTo-SecureString」Pa55w0rd」！ -AsPlainText-推動）|  
|-DatabaseOnly|備份不 CA 憑證資料庫<br /><br />Backup-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-推動|1.可讓您在指定的位置 preexists 備份覆寫-Path 參數中<br /><br />Backup-CARoleService c:\adcsbackup1-推動|  
|-增量|執行增量備份<br /><br />Backup-CARoleService c:\adcsbackup7-增量|  
|-KeepLog|1.指示登入檔案，將指令。 如果未指定切換，登入的檔案會被截斷預設以外增量案例中<br /><br />Backup-CARoleService c:\adcsbackup7-KeepLog|  
  
### <a name="-password-secure-string"></a>密碼 <Secure String>  
如果-密碼參數，則所提供的密碼必須安全字串。  使用**朗讀主機**cmdlet 上市互動式提示字元中的輸入安全密碼，或使用**ConvertTo-SecureString** cmdlet 指定密碼中列。  
  
檢視的下列範例  
  
**指定安全密碼參數使用 Read-Host 字串**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**指定安全密碼參數使用 ConvertTo-SecureString 字串**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet: Restore-CARoleService**  
  
|引數-**粗體**所需的引數|描述|  
|------------------------------------------------|---------------|  
|**路徑**|-字串-位置從備份還原<br />-這是唯一參數的未命名<br />-位置參數<br /><br />**範例：**<br /><br />Restore-CARoleService。-Path c:\adcsbackup1-推動<br /><br />Restore-CARoleService c:\adcsbackup2-推動|  
|-KeyOnly|-還原不資料庫 CA 憑證<br />-如果備份-KeyOnly 選項必須指定<br /><br />**範例：**<br /><br />Restore-CARoleService c:\adcsbackup3-KeyOnly-推動|  
|密碼|-指定 CA 憑證和私密金鑰的密碼<br />-必須安全字串<br /><br />**範例：**<br /><br />Restore-CARoleService c:\adcsbackup4-密碼 (讀取主機-命令提示字元中」的密碼:「-AsSecureString)-推動<br /><br />Restore-CARoleService c:\adcsbackup5-密碼 (ConvertTo-SecureString」Pa55w0rd」！ -AsPlainText-推動）-推動|  
|-DatabaseOnly|-還原不 CA 憑證資料庫<br /><br />Restore-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-推動|-可讓您要覆寫現有的按鍵<br />-是選擇性的參數，但還原就地、時，可能需要<br /><br />Restore-CARoleService c:\adcsbackup1-推動|  
  
### <a name="issues"></a>問題  
在非密碼保護備份如果 ConvertTo-SecureString 功能將會失敗時使用 Backup-CARoleService-密碼參數使用。  
  
![CA 備份與還原](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**表格 7 表格 \\\ * 阿拉伯文 18：常見的錯誤**  
  
|控制項目|錯誤|意見|  
|----------|---------|-----------|  
|**Restore-CARoleServiceC:\ADCSBackup**|Restore-CARoleService：程序無法存取檔案，因為它使用其他處理程序。 (從 HRESULT 例外：<br /><br />0x80070020)|停止之前執行 Restore-CARoleService cmdlet Active Directory 憑證服務服務|  
|**Restore-CARoleServiceC:\ADCSBackup**|Restore-CARoleService：不是空。 (從 HRESULT 例外：0x80070091)|使用-推動參數覆寫現有的按鍵|  
|**Backup-CARoleServiceC:\ADCSBackup-密碼 (Read-Host-提示」的密碼:「-AsSecureString)-DatabaseOnly**|Backup-CARoleService：使用名參數指定無法解析參數設定。|參數-密碼僅使用密碼保護私密金鑰，因此不正確時無法備份他們|  
|**Restore-CARoleServiceC:\ADCSBack15-密碼 (Read-Host-提示」的密碼:「-AsSecureString)-DatabaseOnly**|Restore-CARoleService：使用名參數指定無法解析參數設定。|參數-密碼僅使用密碼保護私密金鑰，因此不正確當您無法還原|  
|**Restore-CARoleServiceC:\ADCSBack14-密碼 (Read-Host-提示」的密碼:」為 AsSecureString)**|Restore-CARoleService：系統找不到指定的檔案。 (從 HRESULT 例外：0x80070002)|指定的路徑不包含有效資料庫備份。  可能是無效的路徑或備份-KeysOnly 選項嗎？|  
  
## <a name="additional-resources"></a>其他資源  
[Active Directory 憑證服務移轉指南](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[備份 CA 資料庫及私密金鑰](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[還原 CA 資料庫和目的地伺服器上的設定](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>試試看：備份 CA 中使用 Windows PowerShell 測試環境  
  
1.  使用這個課程中的命令 CA 資料庫及使用密碼保護的私密金鑰備份。  
  
2.  在這次的 CA 還原暫停。  
  


