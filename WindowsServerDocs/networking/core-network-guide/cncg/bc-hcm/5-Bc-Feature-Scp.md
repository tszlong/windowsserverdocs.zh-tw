---
title: 安裝 BranchCache 功能，並設定裝載快取伺服器服務連接點，
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 854ff9f80a2221a857fab4e6ea7f7c8e6d51f1ef
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>安裝 BranchCache 功能，並設定裝載快取伺服器服務連接點，

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程序 BranchCache」功能在伺服器上安裝裝載快取，HCS1，並設定為隊伍登記服務連接點伺服器 \(SCP\) Active Directory Domain Services \(AD DS\) 中的。

當您使用 AD DS SCP 登記裝載快取的伺服器時，SCP 可 client 自動查詢 SCP AD DS 探索裝載快取的伺服器設定正確的電腦。 在本文稍後提供如何設定 client 電腦上的指示執行此動作。

>[!IMPORTANT]
>執行此程序之前，您必須加入網域的電腦和電腦靜態 IP 位址設定。

若要執行此程序，您必須是系統管理員群組成員。

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>若要安裝 BranchCache 功能和設定裝載快取伺服器  

1. 以系統管理員身分執行 Windows PowerShell 伺服器電腦。 輸入下列命令，，然後按 ENTER 鍵。

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  若要將電腦設定為裝載快取伺服器之後安裝的 BranchCache 功能，並在 AD DS 登記服務連接點，Windows PowerShell 中，輸入下列命令，然後按 ENTER 鍵。

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. 若要確認裝載快取伺服器設定，輸入下列命令，然後按 ENTER。

    ```  
    Get-BCStatus  
    ```  
  
    命令的結果顯示所有方面 BranchCache 安裝的狀態。 以下是一些 BranchCache 設定和正確的值為每個項目：  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. 若要準備您的資料套件複製到裝載快取的伺服器內容伺服器的步驟，不論是找出裝載快取的伺服器上現有共用或建立新的資料夾和，使其可以存取您內容伺服器的共用資料夾。 建立內容伺服器上的資料套件之後，您將此裝載快取伺服器上的共用資料夾複製資料的套件。
  
5. 如果您的部署一部以上的裝載快取伺服器，請重複此程序各個伺服器上。

若要繼續使用此快速入門，請查看[移動及調整大小裝載快取與 #40; 選用和 #41;](6-Bc-Move-Resize-Cache.md).
