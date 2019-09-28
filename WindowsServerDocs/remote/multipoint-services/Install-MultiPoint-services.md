---
title: 安裝 MultiPoint 服務
description: 瞭解如何在 Windows Server 2016 中安裝和設定 MultiPoint 服務
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 442699afe40ee67e4cd4f13572d1a482f675b84a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395378"
---
# <a name="install-multipoint-services"></a>安裝 MultiPoint 服務
如果您要從頭開始安裝伺服器，請遵循這些指示來安裝 MultiPoint 服務。  

在您安裝 Windows Server 2016 之後，會以系統管理員身分成功登入。 使用可啟用 MultiPoint 服務的伺服器管理員。 伺服器管理員會在啟動時自動開啟。 在儀表板上，選取 [**新增角色及功能**] 以啟用 MultiPoint 服務，並遵循 wizard 中的指示。

在安裝類型的區段中，您可能會使用 
- 以角色為基礎或以功能為基礎的安裝或
- 遠端桌面服務安裝

針對標準 MultiPoint 服務部署，建議您選取 [遠端桌面服務] 安裝，讓您可以輕鬆地選取 [部署類型] 底下的 [MultiPoint 服務] 角色。 針對以角色為基礎的安裝，您將需要在角色清單中選取 [ **MultiPoint 服務**]。 伺服器會在安裝成功後重新開機。  
  
## <a name="configure-your-primary-station"></a>設定您的主要工作站  
  
1.  在 [**建立 MultiPoint 伺服器站**] 頁面上，為該監視器輸入鍵盤上指定的字母。 正確的按鍵專案會使鍵盤與滑鼠與該工作站產生關聯。  
2.  以系統管理員身分登入。  