---
title: 適用於 Microsoft 產品的 Linux 軟體存放庫
description: 本文件說明如何使用和安裝 Linux 的 Microsoft 產品的軟體套件。
ms.custom: na
ms.prod: windows-server-threshold
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: dbdbd0f436645f7e19c07e4f3278c5073636a547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831859"
---
# <a name="linux-software-repository-for-microsoft-products"></a>適用於 Microsoft 產品的 Linux 軟體存放庫

## <a name="overview"></a>總覽
Microsoft 建置並支援各種不同的軟體產品，適用於 Linux 系統，並使其可供透過標準的 APT 和 YUM 套件存放庫。 本文件說明如何設定您的 Linux 系統上的儲存機制，以便您可以接著安裝/升級 Microsoft Linux 軟體使用散發套件的標準套件管理工具。

Microsoft 的 Linux 軟體存放庫是由多個子存放庫所組成：

 - prod-子儲存機制適合在生產環境中使用的封裝指定生產環境。 這些套件盡一切商業上受到 Microsoft 依據適用的支援合約或您有 Microsoft 的計劃的條款。

 - mssql server-這些儲存機制包含封裝 Microsoft SQL Server on Linux-請參閱也：[Linux 上的 SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux)。

>[!Note]
Linux 軟體存放庫中的封裝是位在封裝中的授權條款。 請閱讀授權條款，才能使用此封裝。 您的安裝和使用封裝即表示您接受這些條款。 如果您不同意授權條款，請勿使用封裝。


## <a name="configuring-the-repositories"></a>設定存放庫
安裝適用於您的 Linux 散發套件和版本的 Linux 套件，並可自動設定存放庫。 此套件會安裝存放庫組態，以及用來驗證簽署的封裝和/或儲存機制中繼資料工具，例如 apt/yum/zypper GPG 公用金鑰。

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux （RHEL 和變體）

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 （駒）

        wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.04 (Xenial)

        wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.10 (Yakkety)

        wget https://packages.microsoft.com/config/ubuntu/16.10/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update


### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>手動設定
存放庫設定檔都是從[packages.microsoft.com/config](https://packages.microsoft.com/config/)。這些檔案的位置與名稱可以是位於使用下列 URI 的命名慣例：

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**封裝和儲存機制的簽署金鑰**

 - Microsoft 的 GPG 公用金鑰可能會在這裡下載： [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - 公用金鑰的識別碼：Microsoft （簽署版本） <gpgsecurity@microsoft.com>
 - 公用金鑰和指紋： `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

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



