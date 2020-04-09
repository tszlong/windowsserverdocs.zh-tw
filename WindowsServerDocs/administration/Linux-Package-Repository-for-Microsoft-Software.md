---
title: 適用于 Microsoft 產品的 Linux 軟體存放庫
description: 本檔說明如何使用和安裝適用于 Microsoft 產品的 Linux 軟體套件。
ms.prod: windows-server
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: b57a1e7243f989a4529a666880572a9ceaa57644
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852061"
---
# <a name="linux-software-repository-for-microsoft-products"></a>適用于 Microsoft 產品的 Linux 軟體存放庫

## <a name="overview"></a>概觀
Microsoft 會建立並支援適用于 Linux 系統的各種軟體產品，並透過標準 APT 和 YUM 套件存放庫來提供它們。 本檔說明如何在您的 Linux 系統上設定存放庫，讓您可以使用發佈的標準套件管理工具來安裝/升級 Microsoft 的 Linux 軟體。

Microsoft 的 Linux 軟體存放庫是由多個子存放庫所組成：

 - 生產-針對要在生產環境中使用的套件，指定生產子存放庫。 Microsoft 會根據您與 Microsoft 的適用支援合約或方案的條款，提供這些套件的商業支援。

 - mssql-伺服器-這些存放庫包含 Linux 上的 Microsoft SQL Server 的套件-另請參閱： [Linux 上的 SQL Server](https://www.microsoft.com/sql-server/sql-server-vnext-including-Linux)。

> [!Note]
> Linux 軟體存放庫中的套件受限於套件中的授權條款。 使用套件之前，請先閱讀授權條款。 您的套件安裝和使用會構成您接受這些條款的規定。 如果您不同意授權條款，請勿使用套件。


## <a name="configuring-the-repositories"></a>設定存放庫
您可以安裝適用于 Linux 發行版本和版本的 Linux 套件，以自動設定存放庫。 此套件將會安裝存放庫設定以及工具（例如 apt/yum/zypper）用來驗證已簽署套件和/或存放庫中繼資料的 GPG 公開金鑰。

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux （RHEL 和 variant）

 - Enterprise Linux 6 （EL6）

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 （EL7）

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 （Trusty）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/14.04/prod
        sudo apt-get update

 - Ubuntu 16.04 （Xenial）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod
        sudo apt-get update

 - Ubuntu 18.04 （Bionic）

         curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
        sudo apt-get update

 - Ubuntu 18.10 （宇宙射線）

         curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.10/prod
        sudo apt-get update

 - Ubuntu 19.04 （Disco）

         curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/19.04/prod
        sudo apt-get update

### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>手動組態
存放庫設定檔可從[packages.microsoft.com/config](https://packages.microsoft.com/config/)取得。您可以使用下列 URI 命名慣例來找出這些檔案的名稱和位置：

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**套件和存放庫簽署金鑰**

 - Microsoft 的 GPG 公開金鑰可能會在此下載： [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - 公開金鑰識別碼： Microsoft （發行簽署） <gpgsecurity@microsoft.com>
 - 公開金鑰指紋： `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>範例：

 - RHEL/CentOS 7

        # Install repository configuration
        curl https://packages.microsoft.com/config/rhel/7/prod.repo > ./microsoft-prod.repo
        sudo cp ./microsoft-prod.repo /etc/yum.repos.d/

        # Install Microsoft's GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
        sudo rpm --import ./microsoft.asc

 - Ubuntu 16.04

        # Install repository configuration
        curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
        sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

        # Install Microsoft GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
        sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/



