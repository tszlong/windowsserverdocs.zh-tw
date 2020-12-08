---
title: 交涉、工作階段設定和樹狀連接失敗
description: 介紹如何針對協商、會話設定和樹狀連接失敗進行疑難排解。
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 6b310f08757f100dfc005a631e54d0a58d61025c
ms.sourcegitcommit: 2365a7b23e2eccd13be350306c622d2ad9d36bc8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96787731"
---
# <a name="negotiate-session-setup-and-tree-connect-failures"></a>交涉、工作階段設定和樹狀連接失敗

本文說明如何針對 SMB 協商、會話設定和樹狀連接要求期間發生的失敗進行疑難排解。

## <a name="negotiate-fails"></a>協商失敗

SMB 伺服器會從 SMB 用戶端接收 SMB 協商要求。 連接逾時並在60秒後重設。 大約200微秒之後可能會有 ACK 訊息。

此問題最常見的原因是防毒程式。

如果您使用的是 Windows Server 2008 R2，則會有此問題的修補程式。 請確定 SMB 用戶端和 SMB 伺服器是最新的。

## <a name="session-setup-fails"></a>會話設定失敗

SMB 伺服器收到 smb 用戶端的 smb 會話 \_ 設定要求，但無法回應。

如果 (FQDN) 的完整功能變數名稱或網路基本輸入/輸出系統 (NetBIOS) 伺服器名稱為通用命名慣例 (UNC) 路徑，則 Windows 會使用 Kerberos 進行驗證。

在 Negotiate 回應之後，會嘗試取得 Common Internet File System (CIFS) 服務主體名稱 (SPN) 伺服器的 Kerberos 票證。 查看 TCP 通訊埠88上的 Kerberos 流量，以確定 SMB 用戶端取得權杖時沒有 Kerberos 錯誤。

> [!NOTE]
> 在 Kerberos 預先驗證期間發生的錯誤是正常的。 在 Kerberos 預先驗證之後發生的錯誤 (不會) 驗證的實例，就是造成 SMB 問題的錯誤。

此外，請進行下列檢查：

- 請查看 SMB 會話設定要求中的安全性 blob \_ ，以確定傳送的認證正確無誤。

- 嘗試停用 SMB 伺服器名稱強化 (**SmbServerNameHardeningLevel = 0**) 。

- 當 SMB 伺服器透過 CNAME DNS 記錄來存取時，請確定該伺服器具有 SPN。

- 請確定 SMB 簽署可正常運作。  (對於較舊的協力廠商裝置而言特別重要。 ) 

## <a name="tree-connect-fails"></a>樹狀連接失敗

請確定使用者帳號憑證同時具有共用和 NT 檔案系統 (NTFS) 資料夾的許可權。

常見樹狀連接錯誤的原因可在 [3.3.5.7 接收 SMB2 樹 \_ 連接要求](/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87)中找到。 以下是兩個常見狀態碼的解決方案。

\[狀態 \_ 錯誤的 \_ 網路 \_ 名稱\]

請確定該共用存在於伺服器上，而且在 SMB 用戶端要求中的拼寫正確。

\[\_拒絕狀態存取 \_\]

確認共用所使用的磁片和資料夾存在且可供存取。

如果您使用 SMBv3 或更新版本，請檢查伺服器和共用是否需要加密，但用戶端不支援加密。 若要這麼做，請執行下列動作：

- 執行下列命令來檢查伺服器。

  ```PowerShell
  Get-SmbServerConfiguration | select Encrypt*
  ```

  如果 EncryptData 和 RejectUnencryptedAccess 為 true，則伺服器需要加密。

- 執行下列命令來檢查共用：

  ```PowerShell
  Get-SmbShare | select name, EncryptData  
  ```

  如果共用上的 EncryptData 為 true，且伺服器上的 RejectUnencryptedAccess 為 true，則共用需要加密

當您進行疑難排解時，請遵循下列指導方針：

- Windows 8、Windows Server 2012 和更新版本的 Windows 支援用戶端加密 (SMBv3 和更新版本) 。

- Windows 7、Windows Server 2008 R2 和較早版本的 Windows 不支援用戶端加密。

- Samba 和協力廠商裝置可能不支援加密。 您可能必須參閱產品檔以取得詳細資訊。

## <a name="references"></a>參考資料

如需詳細資訊，請參閱下列文章。

[3.3.5.4 接收 SMB2 NEGOTIATE 要求](/openspecs/windows_protocols/ms-smb2/b39f253e-4963-40df-8dff-2f9040ebbeb1)

[3.3.5.5 接收 SMB2 會話 \_ 設定要求](/openspecs/windows_protocols/ms-smb2/e545352b-9f2b-4c5e-9350-db46e4f6755e)

[3.3.5.7 接收 SMB2 樹 \_ 連接要求](/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87)
