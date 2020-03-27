---
title: 安裝 BranchCache 功能並依據服務連接點設定託管快取伺服器
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 987b46b1748d5a889aa69823d3492a707948a100
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318478"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>安裝 BranchCache 功能並依據服務連接點設定託管快取伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程式在託管快取伺服器 HCS1 上安裝 BranchCache 功能，並將伺服器設定為在 Active Directory Domain Services \(AD DS\)中註冊服務連接點 \(SCP\)。

當您在 AD DS 中註冊具有 SCP 的託管快取伺服器時，SCP 會允許已正確設定的用戶端電腦，藉由查詢 SCP 的 AD DS 來自動探索託管快取伺服器。 本指南稍後會提供如何設定用戶端電腦來執行此動作的指示。

>[!IMPORTANT]
>執行此程式之前，您必須先將電腦加入網域，並使用靜態 IP 位址來設定電腦。

若要執行此程序，您必須是 Administrators 群組成員。

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>安裝 BranchCache 功能並設定託管快取伺服器  

1. 在伺服器電腦上，以系統管理員身分執行 Windows PowerShell。 輸入下列命令，然後按 ENTER。

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  若要在安裝 BranchCache 功能之後將電腦設定為託管快取伺服器，並在 AD DS 中註冊服務連接點，請在 Windows PowerShell 中輸入下列命令，然後按 ENTER。

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. 若要確認託管快取伺服器設定，請輸入下列命令，然後按 ENTER。

    ```  
    Get-BCStatus  
    ```  
  
    命令的結果會顯示 BranchCache 安裝所有層面的狀態。 以下是幾個 BranchCache 設定和每個專案的正確值：  
  
    -   BranchCacheIsEnabled： True

    -   HostedCacheServerIsEnabled： True

    -   HostedCacheScpRegistrationEnabled： True

4. 若要準備將您的資料封裝從內容伺服器複製到託管快取伺服器的步驟，請在託管快取伺服器上識別現有的共用，或建立新的資料夾並共用該資料夾，以便從您的內容伺服器進行存取。 在內容伺服器上建立資料套件之後，您會將資料套件複製到託管快取伺服器上的這個共用資料夾。
  
5. 如果您要部署多部託管快取伺服器，請在每部伺服器上重複此程式。

若要繼續進行本指南，請參閱[移動並調整託管&#40;快&#41;取的大小選擇](6-Bc-Move-Resize-Cache.md)。
