---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, 安裝, 設定
contributor: maertendMSFT
author: maertendMSFT
title: 適用於 Windows 的 OpenSSH 伺服器設定
ms.product: w10
ms.openlocfilehash: fa3d40617a04c092403d9d2e018bd2eb82d20cd9
ms.sourcegitcommit: effbc183bf4b370905d95c975626c1ccde057401
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781315"
---
# <a name="openssh-key-management"></a>OpenSSH 金鑰管理

Windows 環境中的大部分驗證都是使用使用者名稱-密碼配對來完成。
這適用於共用通用網域的系統。 跨網域 (例如在內部部署和雲端託管系統之間) 工作時，會變得更棘手。

相較之下，Linux 環境通常會使用公開金鑰/私密金鑰組來驅動驗證。
OpenSSH 包含可協助支援此操作的工具，特別是：

* __ssh-keygen__，用來產生安全金鑰
* __ssh-agent__ 和 __ssh-add__，用來安全地儲存私密金鑰
* __scp__ 和 __sftp__，在初始使用伺服器期間安全地複製公開金鑰檔案

本文件概述如何在 Windows 上使用這些工具，開始搭配 SSH 使用金鑰驗證。 如果您不熟悉 SSH 金鑰管理，我們強烈建議您檢閱 [NIST 文件 IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf)，標題為「使用安全殼層 (SSH) 的互動式和自動化存取管理的安全性」。

## <a name="about-key-pairs"></a>關於金鑰組

金鑰組指的是特定驗證通訊協定所使用的公開和私密金鑰檔案。 

SSH 公開金鑰驗證會使用非對稱式密碼編譯演算法來產生兩個金鑰檔案 - 一個「私密」、另一個「公用」。 私密金鑰檔案相當於密碼，而且在所有情況下都應該受到保護。 如果有人取得您的私密金鑰，他們就可以登入您有權存取的任何 SSH 伺服器。 公開金鑰是放在 SSH 伺服器上的金鑰，而且可以在不危及私密金鑰的情況下共用。

搭配 SSH 伺服器使用金鑰驗證時，SSH 伺服器和用戶端會根據私密金鑰提供的使用者名稱來比較公開金鑰。 如果無法根據用戶端私密金鑰驗證公開金鑰，驗證就會失敗。 

多重要素驗證可以使用金鑰組來實作，方法是要求在產生金鑰組時提供複雜密碼 (請參閱以下的金鑰產生)。 在驗證期間，系統會提示使用者輸入複雜密碼，該密碼會與 SSH 用戶端上的私密金鑰一起用來驗證使用者。 

## <a name="host-key-generation"></a>主機金鑰產生

公開金鑰具有特定的 ACL 需求，在 Windows 上等同於只允許系統管理員和系統的存取權。 為了簡化此作業， 

* 已建立 OpenSSHUtils PowerShell 模組來適當地設定金鑰 ACL，而且應該安裝在伺服器上
* 第一次使用 sshd 時，將會自動產生主機的金鑰組。 如果 ssh-agent 正在執行，金鑰會自動新增至本機存放區。 

若要使用 SSH 伺服器輕鬆進行金鑰驗證，請從提升權限的 PowerShell 提示字元執行下列命令：

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

因為沒有與 sshd 服務相關聯的使用者，所以主機金鑰會儲存在 \ProgramData\ssh 底下。


## <a name="user-key-generation"></a>使用者金鑰產生

若要使用金鑰型驗證，您必須先為您的用戶端產生一些公開/私密金鑰組。 從 PowerShell 或 cmd，使用 ssh-keygen 來產生一些金鑰檔案。

```powershell
cd ~\.ssh\
ssh-keygen
```

應該會顯示如下的內容 (其中 "username" 會取代為您的使用者名稱)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

您可以按 Enter 鍵以接受預設值，或指定您想要產生金鑰所在的路徑。 此時，系統會提示您使用複雜密碼來加密您的私密金鑰檔案。
複雜密碼可與金鑰檔案搭配使用，以提供雙因素驗證。 在此範例中，我們會將複雜密碼保留空白。 

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in C:\Users\username\.ssh\id_ed25519.
Your public key has been saved in C:\Users\username\.ssh\id_ed25519.pub.
The key fingerprint is: 
SHA256:OIzc1yE7joL2Bzy8!gS0j8eGK7bYaH1FmF3sDuMeSj8 username@server@LOCAL-HOSTNAME

The key's randomart image is:
+--[ED25519 256]--+
|        .        |
|         o       |
|    . + + .      |
|   o B * = .     |
|   o= B S .      |
|   .=B O o       |
|  + =+% o        |
| *oo.O.E         |
|+.o+=o. .        |
+----[SHA256]-----+
```

現在您有公開/私密 ED25519 金鑰組 (.pub 檔案是公開金鑰，其餘則是私密金鑰)：

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

請記住，私密金鑰檔案相當於密碼，應該與保護密碼的方式一樣加以保護。
為了協助您進行這項操作，請使用 ssh-agent 安全地將私密金鑰儲存在 Windows 安全性內容中，並與您的 Windows 登入產生關聯。 若要這麼做，請以系統管理員身分啟動 ssh-agent 服務，並使用 ssh-add 來儲存私密金鑰。 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

完成這些步驟之後，每當此用戶端需要私密金鑰來進行驗證時，ssh-agent 就會自動擷取本機私密金鑰，並將它傳遞給您的 SSH 用戶端。

> [!NOTE]
> 強烈建議您將私密金鑰備份至安全的位置，然後在將它新增至 ssh-agent 之後  ，從本機系統將它刪除。
> 無法從代理程式擷取私密金鑰。
> 如果您失去私密金鑰的存取權，就必須建立新的金鑰組，並且在您互動的所有系統上更新公開金鑰。

## <a name="deploying-the-public-key"></a>部署公開金鑰

若要使用以上建立的使用者金鑰，必須將公開金鑰放在伺服器中位於 users\username\.ssh\. 下，稱為 authorized_keys  的文字檔 OpenSSH 工具包含 scp，這是安全的檔案傳輸公用程式，可協助進行此工作。

將您的公開金鑰內容 (~\.ssh\id_ed25519.pub) 移至 server/host 上，在 ~\.ssh\ 中稱為 authorized_keys 的文字檔。

這個範例會在 OpenSSHUtils 模組中使用 Repair-AuthorizedKeyPermissions 函式，該模組先前已依照上述指示安裝在主機上。

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

這些步驟會完成在 Windows 上搭配 SSH 使用金鑰型驗證所需的設定。
然後，使用者就可以從任何具有私密金鑰的用戶端連線到 sshd 主機。

