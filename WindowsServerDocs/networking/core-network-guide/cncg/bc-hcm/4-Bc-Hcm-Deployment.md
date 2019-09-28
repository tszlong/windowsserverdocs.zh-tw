---
title: BranchCache 託管快取模式部署
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.prod: windows-server
ms.suite: na
ms.technology: networking-bc
ms.topic: article
ms.assetid: c635fa48-d064-4b8b-9dce-9f26abfbcfa4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8734942e6d048df4be0107c67af7a2f741845e8b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406409"
---
# <a name="branchcache-hosted-cache-mode-deployment"></a>BranchCache 託管快取模式部署

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本主題，以取得詳細的程式主題連結，引導您完成 BranchCache 託管快取模式部署流程。

請遵循下列步驟來部署 BranchCache 託管快取模式。

- [安裝 BranchCache 功能，並依服務連接點設定託管快取伺服器](5-Bc-Feature-Scp.md)

- [移動並調整託管快取&#40;的大小（選擇性）&#41;](6-Bc-Move-Resize-Cache.md)

- [在託管快取伺服器&#40;上預先雜湊並預先載入內容（選擇性）&#41;](7-Bc-Prehash-Preload.md)

- [透過服務連接點設定用戶端自動託管快取探索](10-Bc-Client-By-Scp.md)

>[!NOTE]
>本指南中的程式不包含開啟 [**使用者帳戶控制**] 對話方塊來要求您的許可權以繼續的情況的指示。 如果執行本指南的程序時此對話方塊開啟，或此對話方塊因回應您的動作而開啟，請按一下 [繼續]。

若要繼續進行本指南，請參閱[安裝 BranchCache 功能和依照服務連接點設定託管](5-Bc-Feature-Scp.md)快取伺服器。