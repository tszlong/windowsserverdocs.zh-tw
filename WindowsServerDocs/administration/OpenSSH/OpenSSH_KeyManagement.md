---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH、 SSH、 SSHD，安裝，安裝程式
contributor: maertendMSFT
author: maertendMSFT
title: Windows 的 OpenSSH 伺服器組態
ms.product: w10
ms.openlocfilehash: 3e2981cbbdfe34bf28a77d5121bff0b663e0d3bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837149"
---
# <a name="openssh-key-management"></a>OpenSSH 金鑰管理

在 Windows 環境中的大部分驗證是使用使用者名稱密碼組。
這適用於共用通用的網域的系統。 跨網域時，例如內部部署和雲端裝載系統之間會變得更困難。

相較之下，Linux 環境通常會使用公用金鑰/私用金鑰組，以驗證磁碟機。
OpenSSH 包含工具，可協助支援此功能，特別是：

* __ssh-keygen__產生安全的金鑰
* __ssh__並__ssh 新增__安全地儲存私密金鑰
* __scp__並__sftp__來安全地在伺服器的初次使用時複製公開金鑰檔案

本文件提供如何使用這些工具在 Windows 上開始使用 SSH 金鑰驗證的概觀。 如果您不熟悉 SSH 金鑰管理，我們強烈建議您檢閱[NIST 文件 IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf)標題為 「 安全性的互動式和自動化存取管理使用安全殼層 (SSH) 」。

## <a name="about-key-pairs"></a>關於金鑰組

金鑰組，請參閱公用和私密金鑰檔案所使用的特定驗證通訊協定。 

SSH 公開金鑰驗證會使用非對稱密碼編譯演算法來產生兩個金鑰的檔案 – 一個 「 私人 」 和其他 「 公用 」。 私密金鑰檔案，相當於使用密碼，並應該在所有情況下保護。 如果有人取得您的私密金鑰，他們可以登入您具有存取權的任何 SSH 伺服器。 什麼放在 SSH 伺服器，和可能共用而不會危害的私用金鑰的公開金鑰。

使用金鑰驗證時使用 SSH 伺服器，SSH 伺服器和用戶端會比較針對私用金鑰提供使用者名稱的公開金鑰。 如果無法驗證的公開金鑰，針對用戶端的私密金鑰，則驗證會失敗。 

可能的金鑰組實作多重要素驗證，要求產生的金鑰組時，會提供複雜密碼 （請參閱下方的金鑰產生）。 在驗證期間會提示使用者輸入複雜密碼，這搭配來驗證使用者的 SSH 用戶端上的私密金鑰存在。 

## <a name="host-key-generation"></a>主機金鑰產生

公開金鑰有特定 ACL 需求，在 Windows，等同於為只允許系統管理員和系統存取權。 為了簡化起見， 

* OpenSSHUtils PowerShell 模組已正確設定機碼的 Acl 建立，而且應該安裝在伺服器上
* 第一次使用 sshd，將會自動產生的主應用程式的金鑰組。 如果 ssh 代理程式正在執行時，金鑰會自動加入至本機存放區。 

為了方便金鑰驗證 SSH 伺服器，請從提升權限的 PowerShell 提示字元執行下列命令：

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

因為沒有與 sshd 服務相關聯的使用者，就會將主機金鑰儲存在 \ProgramData\ssh。


## <a name="user-key-generation"></a>使用者金鑰產生

若要使用索引鍵為基礎的驗證，必須先為您的用戶端產生公開/私密金鑰組。 從 PowerShell 或 cmd，使用的 ssh-keygen 來產生一些重要的檔案。

```powershell
cd ~\.ssh\
ssh-keygen
```

這應該會顯示如下 （其中由您的使用者名稱取代"username"）

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

您可以按下 enter 鍵接受預設位置，或指定您要用來產生金鑰的路徑。 此時，系統會提示您使用複雜密碼來加密您的私人金鑰檔案。
金鑰的檔案，以提供 2 雙因素驗證適用於複雜密碼。 針對此範例中，我們會將保留複雜密碼空白。 

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

現在您有公開/私密金鑰 ED25519 金鑰組 （.pub 檔案都是公用的索引鍵和其餘部分都是私用的索引鍵）：

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

請記住，私密金鑰檔案密碼的對等項目應受到保護，保護您的密碼相同的方式。
若要有幫助，使用 ssh 來安全地儲存在 Windows 安全性內容中，您的 Windows 登入相關聯的私密金鑰。 若要這樣做，請以系統管理員身分啟動 ssh 服務，使用 ssh-add 將私密金鑰儲存。 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

完成這些步驟之後，每當的私用金鑰驗證所需之此用戶端，從 ssh 代理程式會自動擷取本機的私用金鑰並將它傳遞給您的 SSH 用戶端。

> [!NOTE]
> 強烈建議您備份您的私密金鑰，以安全的位置，然後從本機系統中，刪除*之後*將它新增至 ssh 代理程式。
> 無法擷取私密金鑰，從代理程式。
> 如果您無法存取私密金鑰時，您必須建立新的金鑰組，並更新所有的系統上公開金鑰與您互動。

## <a name="deploying-the-public-key"></a>部署公用金鑰

若要使用上面所建立的使用者金鑰，公開金鑰必須放在伺服器上，文字檔案，稱為*authorized_keys* users\username\ssh 底下。 OpenSSH 的工具包括 scp，也就是安全的檔案傳輸公用程式，可協助完成這。

若要將您的公開金鑰的內容移動 (~\.ssh\id_ed25519.pub) 成為文字檔案，請呼叫的 authorized_keys ~\.ssh\ 伺服器/主機上的。

此範例會使用先前安裝上述指示中的主機上 OpenSSHUtils 模組中的修復 AuthorizedKeyPermissions 函式。

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

這些步驟完成時要使用 Windows 上的 SSH 金鑰型驗證所需的設定。
在此之後，使用者可以從任何具有私密金鑰的用戶端連線到 sshd 主機。

