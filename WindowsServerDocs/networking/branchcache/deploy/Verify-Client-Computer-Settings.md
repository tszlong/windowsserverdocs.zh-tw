---
title: 確認用戶端電腦設定
description: 瞭解如何確認已正確設定 BranchCache 的用戶端電腦。
manager: brianlic
ms.topic: how-to
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 944a36f2215ec9bfba4a7d90ef7708d3c38b8a77
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946734"
---
# <a name="verify-client-computer-settings"></a>確認用戶端電腦設定

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式來確認已正確設定 BranchCache 的用戶端電腦。

> [!NOTE]
> 此套裝程式含手動更新群組原則和重新開機 BranchCache 服務的步驟。 如果您重新開機電腦，就不需要執行這些動作，因為這種情況會自動發生。

您必須是系統 **管理員** 的成員，或等同于執行此程式。

### <a name="to-verify-branchcache-client-computer-settings"></a>確認 BranchCache 用戶端電腦設定

1.  若要在您要驗證其 BranchCache 設定的用戶端電腦上重新整理群組原則，請以系統管理員身分執行 Windows PowerShell，輸入下列命令，然後按 ENTER。

    `gpupdate /force`

2.  針對在託管快取模式中設定的用戶端電腦，並設定為自動依服務連接點探索託管快取伺服器，請執行下列命令來停止並重新啟動 BranchCache 服務。

    `net stop peerdistsvc`

    `net start peerdistsvc`

3.  執行下列命令來檢查目前的 BranchCache 操作模式。

    `Get-BCStatus`

4.  在 Windows PowerShell 中，檢查 **BCStatus** 命令的輸出。

    **BranchCacheIsEnabled** 的值應為 **True**。

    在 **ClientSettings** 中，[ **CurrentClientMode** ] 的值應該是 [ **DistributedClient** ] 或 [ **HostedCacheClient**]，視您使用本指南設定的模式而定。

    在 **ClientSettings** 中，如果您已設定託管快取模式，並在設定期間提供託管快取伺服器的名稱，或用戶端已使用服務連接點自動找到託管快取伺服器，則 **HostedCacheServerList** 的值應與託管快取伺服器的名稱或名稱相同。 例如，如果您的託管快取伺服器命名為 HCS1，而您的網域是 corp.contoso.com，則 **HostedCacheServerList** 的值為 **HCS1.corp.contoso.com**。

5.  如果上述任何一項 BranchCache 設定沒有正確的值，請使用本指南中的步驟來確認您所設定的群組原則或本機電腦原則設定，以及防火牆例外狀況，並確保這些設定正確無誤。 此外，請重新開機電腦，或依照此程式中的步驟重新整理群組原則並重新啟動 BranchCache 服務。



