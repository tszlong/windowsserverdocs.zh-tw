---
title: BranchCache 託管快取模式部署
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache 的指示。
manager: brianlic
ms.topic: article
ms.assetid: c635fa48-d064-4b8b-9dce-9f26abfbcfa4
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 212c1244bd2383b80fcc754cf6ebb877359c53c6
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947874"
---
# <a name="branchcache-hosted-cache-mode-deployment"></a>BranchCache 託管快取模式部署

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本主題來取得詳細程式主題的連結，這些主題會引導您完成 BranchCache 託管快取模式部署流程。

遵循下列步驟來部署 BranchCache 託管快取模式。

- [安裝 BranchCache 功能，並依服務連接點設定託管快取伺服器](5-Bc-Feature-Scp.md)

- [移動託管快取並調整其大小 &#40;選擇性&#41;](6-Bc-Move-Resize-Cache.md)

- [Prehash 和預先載入託管快取伺服器上的內容 &#40;選擇性&#41;](7-Bc-Prehash-Preload.md)

- [依服務連接點設定用戶端自動託管快取探索](10-Bc-Client-By-Scp.md)

>[!NOTE]
>本指南中的程式不包含 [ **使用者帳戶控制** ] 對話方塊開啟時，要求您的許可權以繼續執行的指示。 如果執行本指南的程序時此對話方塊開啟，或此對話方塊因回應您的動作而開啟，請按一下 [繼續]。

若要繼續進行本指南，請參閱 [安裝 BranchCache 功能，並依服務連接點設定託管](5-Bc-Feature-Scp.md)快取伺服器。