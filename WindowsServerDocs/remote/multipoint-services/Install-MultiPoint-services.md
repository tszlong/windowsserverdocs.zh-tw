---
title: 安裝 MultiPoint 服務
description: 了解如何安裝和設定 Windows Server 2016 中的 MultiPoint 服務
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 52a824bbca3e9f2e1c7823601f6208ae19ae50ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866219"
---
# <a name="install-multipoint-services"></a>安裝 MultiPoint 服務
如果您要安裝的伺服器，從頭依照這些指示安裝 MultiPoint 服務。  

您已成功安裝 Windows Server 2016 之後，系統管理員身分登入。 使用伺服器管理員，您可以在其中啟用 MultiPoint 服務。 伺服器管理員會在啟動時自動開啟。 在儀表板選取**新增角色及功能**來讓 MultiPoint 服務，並遵循精靈中的指示。

在 [安裝類型] 區段中就可能會移使用 
- 角色型或功能型安裝或
- 遠端桌面服務安裝

適用於標準的 MultiPoint 服務部署中，我們建議選取可讓您輕鬆地選取部署類型在 MultiPoint 服務角色的遠端桌面服務安裝。 以角色為基礎的安裝，您將需要選取**MultiPoint 服務**的角色清單中。 安裝成功後，伺服器會重新啟動。  
  
## <a name="configure-your-primary-station"></a>設定您的主要站台  
  
1.  在 **建立 MultiPoint Server 站台**頁面上，輸入該監視器的鍵盤從指定的字母。 正確的機碼項目建立關聯的鍵盤和滑鼠該站台。  
2.  以系統管理員身分登入。  