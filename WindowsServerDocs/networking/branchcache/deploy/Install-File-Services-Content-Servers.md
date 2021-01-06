---
title: 安裝檔案服務內容伺服器
description: 瞭解如何安裝檔案服務伺服器角色的 [網路檔案的 BranchCache] 角色服務，並根據您的需求在檔案共用上啟用 BranchCache。
manager: brianlic
ms.topic: how-to
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 57fa39120b31ea2622c3a7d997a62fa671d2455a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949554"
---
# <a name="install-file-services-content-servers"></a>安裝檔案服務內容伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

若要部署執行檔案服務伺服器角色的內容伺服器，您必須安裝檔案服務伺服器角色的 [網路檔案的 BranchCache] 角色服務。 此外，您必須根據您的需求，在檔案共用上啟用 BranchCache。

在內容伺服器的設定期間，您可以允許 BranchCache 發佈所有檔案共用的內容，也可以選取要發行的檔案共用子集。

> [!NOTE]
> 當您將啟用 BranchCache 的檔案伺服器或 Web 服務器部署為內容伺服器時，現在會在 BranchCache 用戶端要求檔案之前離線計算內容資訊。 由於這項改進，您不需要為內容伺服器設定雜湊發行，如同您在舊版 BranchCache 中所做的一樣。
>
> 這項自動雜湊產生可提供更快速的效能和更多的頻寬節省，因為內容資訊已可供第一個要求內容的用戶端使用，且已執行計算。

請參閱下列主題以部署內容伺服器。

-   [設定檔案服務伺服器角色](../../branchcache/deploy/Configure-the-File-Services-server-role.md)

-   [啟用檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)

-   [在檔案共用上啟用 BranchCache &#40;選擇性&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)



