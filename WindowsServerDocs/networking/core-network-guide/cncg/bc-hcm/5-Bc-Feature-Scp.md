---
title: 安裝 BranchCache 功能並依據服務連接點設定託管快取伺服器
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6619b09df0d4c161148d22091337a5039c7ea3af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849649"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>安裝 BranchCache 功能並依據服務連接點設定託管快取伺服器

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在託管快取伺服器 HCS1，安裝 BranchCache 功能，並設定要登錄服務連接點的伺服器，您可以使用此程序\(SCP\) Active Directory 網域服務中\(AD DS\).

當您使用 SCP，AD DS 中註冊託管快取伺服器時，SCP 可讓用戶端自動探索託管快取伺服器，透過查詢 SCP 的 AD DS 已正確設定的電腦。 稍後在本指南提供如何設定用戶端電腦上執行此動作的指示。

>[!IMPORTANT]
>執行此程序之前，您必須將電腦加入網域，並使用靜態 IP 位址設定電腦。

若要執行此程序，您必須是 Administrators 群組成員。

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>安裝 BranchCache 功能及設定託管快取伺服器  

1. 在 系統管理員身分執行 Windows PowerShell 的伺服器電腦。 輸入下列命令，然後按 ENTER。

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  若要將電腦設定為託管快取伺服器之後安裝 BranchCache 功能，並在 AD DS 中登錄服務連接點，在 Windows PowerShell 中，輸入下列命令，然後按 ENTER 鍵。

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. 若要確認託管快取伺服器組態，請輸入下列命令，然後按 ENTER。

    ```  
    Get-BCStatus  
    ```  
  
    命令的結果會顯示您 BranchCache 安裝的各個層面的狀態。 以下是幾個 BranchCache 設定和正確的值，每個項目：  
  
    -   BranchCacheIsEnabled:True

    -   HostedCacheServerIsEnabled:True

    -   HostedCacheScpRegistrationEnabled:True

4. 若要準備內容伺服器中的資料套件複製到您的託管快取伺服器的步驟，可能是識別託管快取伺服器上現有的共用，或建立新的資料夾，並使其可從您的內容伺服器存取共用資料夾。 您內容的伺服器上建立資料套件之後，您會將資料封裝複製此託管快取伺服器上的共用資料夾。
  
5. 如果您要部署多部託管快取伺服器，重複此程序在每一部伺服器上。

若要繼續進行本指南，請參閱[移動和調整託管快取&#40;選擇性&#41;](6-Bc-Move-Resize-Cache.md)。
