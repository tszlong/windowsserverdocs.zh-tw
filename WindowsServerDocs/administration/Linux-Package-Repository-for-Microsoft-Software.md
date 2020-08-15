---
title: 適用于 Microsoft 產品的 Linux 軟體存放庫
description: 本檔說明如何使用和安裝適用于 Microsoft 產品的 Linux 軟體套件。
manager: szark
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 08/14/2020
ms.openlocfilehash: 421518ffceb2fb5cab0ed84e9299004084311924
ms.sourcegitcommit: b18effcba95c85d6e08e7d29808b576491a5352e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2020
ms.locfileid: "88243180"
---
# <a name="linux-software-repository-for-microsoft-products"></a>適用于 Microsoft 產品的 Linux 軟體存放庫

## <a name="overview"></a>概觀

Microsoft 會建立並支援適用于 Linux 系統的各種軟體產品，並透過標準 APT 和 YUM 套件存放庫來提供它們。 本檔說明如何在您的 Linux 系統上設定存放庫，讓您可以使用發佈的標準套件管理工具來安裝/升級 Microsoft 的 Linux 軟體。

Microsoft 的 Linux 軟體存放庫是由多個子存放庫所組成：

 - 生產-針對要在生產環境中使用的套件，指定生產子存放庫。 Microsoft 會根據您與 Microsoft 的適用支援合約或方案的條款，提供這些套件的商業支援。

 - mssql-伺服器-這些存放庫包含 Linux 上的 Microsoft SQL Server 的套件-另請參閱： [Linux 上的 SQL Server](https://www.microsoft.com/sql-server/sql-server-vnext-including-Linux)。

> [!NOTE]
> Linux 軟體存放庫中的套件受限於套件中的授權條款。 使用封裝前，請先閱讀授權條款。 安裝及使用套件即表示接受這些授權條款。 如果您不同意授權條款，請勿使用封裝。

## <a name="configuring-the-repositories"></a>設定存放庫

您可以安裝適用于 Linux 發行版本和版本的 Linux 套件，以自動設定存放庫。 此套件將會安裝存放庫設定以及工具（例如 apt/yum/zypper）用來驗證已簽署套件和/或存放庫中繼資料的 GPG 公開金鑰。

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL 和 variant) 

 - Enterprise Linux 6 (EL6) <p>`sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm`

 - Enterprise Linux 7 (EL7) <p>`sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm`

 - Enterprise Linux 8 (EL8) <p>`sudo rpm -Uvh https://packages.microsoft.com/config/rhel/8/packages-microsoft-prod.rpm`

### <a name="suse"></a>SUSE

 - SUSE Linux Enterprise Server 12<p>`sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm`

 - SUSE Linux Enterprise Server 15<p>`sudo rpm -Uvh https://packages.microsoft.com/config/sles/15/packages-microsoft-prod.rpm`

### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 16.04 (Xenial) <p>`curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -`<p>`sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod`<p>`sudo apt-get update`

 - Ubuntu 18.04 (Bionic) <p>`curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -`<p>`sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod`<p>`sudo apt-get update`

 - Ubuntu 20.04 (Disco) <p>`curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -`<p>`sudo apt-add-repository https://packages.microsoft.com/ubuntu/20.04/prod`<p>`sudo apt-get update`

## <a name="manual-configuration"></a>手動組態

存放庫設定檔可從 [packages.microsoft.com/config](https://packages.microsoft.com/config/)取得。您可以使用下列 URI 命名慣例來找出這些檔案的名稱和位置：

`https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)`

### <a name="package-and-repository-signing-key"></a>套件和存放庫簽署金鑰

- Microsoft 的 GPG 公開金鑰可能會在此下載： [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
- 公開金鑰識別碼： Microsoft (發行簽署) <gpgsecurity@microsoft.com>
- 公開金鑰指紋： `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>範例

 - RHEL/CentOS 7

```
# Install repository configuration
curl -sSL https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft-prod.repo

# Install Microsoft's GPG public key
curl -sSL https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
sudo rpm --import ./microsoft.asc
```

 - Ubuntu 20.04

```
# Install repository configuration
curl -sSL https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/microsoft-prod.list

# Install Microsoft GPG public key
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Update package index files
sudo apt-get update
```
