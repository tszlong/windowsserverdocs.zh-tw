---
title: 啟用遠端桌面服務授權伺服器
description: 安裝並啟用 RD 授權伺服器
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: d28ceac9cde0ee2d4c92867bdd90d5c463a8cd4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888009"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>啟用遠端桌面服務授權伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

遠端桌面服務，將伺服器問題用戶端存取使用權 (Cal) 授權給使用者和裝置存取 RD 工作階段主機時。 您可以使用遠端桌面授權管理員來啟用授權伺服器。 

## <a name="install-the-rd-licensing-role"></a>安裝 RD 授權的角色

1. 登入您想要做為使用系統管理員帳戶的授權伺服器的伺服器。
2. 在 [伺服器管理員] 中，按一下**角色摘要**，然後按一下**新增角色**。
   按一下 **下一步**角色精靈 的第一頁上。
3. 選取**Remote Desktop Services**，然後按一下**下一步**，然後**下一步**在遠端桌面服務 頁面上。
4. 選取 [**遠端桌面授權**，然後按一下**下一步]**。
5. 設定網域-選取**設定此授權伺服器探索領域**，按一下**此網域**，然後按一下**下一步**。
6. 按一下 **\[安裝\]**。

## <a name="activate-the-license-server"></a>啟用授權伺服器

1. 開啟遠端桌面授權管理員: 按一下**開始 > 系統管理工具 > 遠端桌面服務 > 遠端桌面授權管理員**。
2. 授權伺服器，以滑鼠右鍵按一下，然後按一下**啟用伺服器**。
3. 按一下 [**下一步]** 歡迎頁面上。
4. 針對 [連線方法中，選取**自動連線 （建議）**，然後按一下**下一步]**。
5. 輸入您的公司資訊 （您的姓名、 公司名稱、 您的地理區域），然後按一下**下一步**。
6. （選擇性） 輸入任何其他的公司資訊 （例如，電子郵件和公司地址），然後再按一下**下一步**。 
7. 請確定**立即啟動安裝授權精靈**未選取 （我們將在稍後的步驟安裝授權），然後按一下**下一步**。

您的授權伺服器現在已準備好開始發出及管理授權的。 