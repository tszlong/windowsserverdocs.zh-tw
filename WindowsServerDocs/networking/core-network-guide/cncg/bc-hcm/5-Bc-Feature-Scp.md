---
title: 安裝 BranchCache 功能並依據服務連接點設定託管快取伺服器
description: 瞭解如何在託管快取伺服器上安裝 BranchCache 功能、HCS1，以及將伺服器設定為在 AD DS 中註冊 SCP。
manager: brianlic
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: bfee053492e9c28869ec39cc75bdd8d0c376c256
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965560"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>安裝 BranchCache 功能並依據服務連接點設定託管快取伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程式在託管快取伺服器上安裝 BranchCache 功能 HCS1，並設定伺服器 \( \) 在 Active Directory Domain Services AD DS 中註冊服務連接點 SCP \( \) 。

當您在 AD DS 中向 SCP 註冊託管快取伺服器時，SCP 會讓設定正確的用戶端電腦透過查詢 SCP 的 AD DS 來自動探索託管快取伺服器。 本指南稍後會提供如何設定用戶端電腦來執行此動作的指示。

>[!IMPORTANT]
>執行此程式之前，您必須先將電腦加入網域，並使用靜態 IP 位址來設定電腦。

若要執行此程序，您必須是 Administrators 群組成員。

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>安裝 BranchCache 功能並設定託管快取伺服器

1. 在伺服器電腦上，以系統管理員身分執行 Windows PowerShell。 輸入下列命令，然後按 ENTER 鍵。

    ```
    Install-WindowsFeature BranchCache
    ```

2.  若要在安裝 BranchCache 功能之後將電腦設定為託管快取伺服器，並在 AD DS 中註冊服務連接點，請在 Windows PowerShell 中輸入下列命令，然後按 ENTER 鍵。

    ```
    Enable-BCHostedServer -RegisterSCP
    ```

3. 若要確認託管快取伺服器設定，請輸入下列命令，然後按 ENTER。

    ```
    Get-BCStatus
    ```

    命令的結果會顯示 BranchCache 安裝之所有層面的狀態。 以下是一些 BranchCache 設定和每個專案的正確值：

    -   BranchCacheIsEnabled： True

    -   HostedCacheServerIsEnabled： True

    -   HostedCacheScpRegistrationEnabled： True

4. 若要準備將您的資料套件從內容伺服器複製到託管快取伺服器的步驟，請在託管快取伺服器上識別現有的共用，或建立新的資料夾並共用該資料夾，以便從內容伺服器存取該資料夾。 在內容伺服器上建立資料封裝之後，您會將資料套件複製到託管快取伺服器上的這個共用資料夾。

5. 如果您要部署一部以上的託管快取伺服器，請在每部伺服器上重複此程式。

若要繼續進行本指南，請參閱 [將託管快取移動和調整大小 &#40;選擇性&#41;](6-Bc-Move-Resize-Cache.md)。
