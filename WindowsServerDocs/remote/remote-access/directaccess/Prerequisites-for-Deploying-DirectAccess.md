---
title: 部署 DirectAccess 的必要條件
description: 本主題提供 Windows Server 2016 中的 DirectAccess 部署的必要條件。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09c21facd1a0242b1edbd0436e90c8ebd79f0e2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824169"
---
# <a name="prerequisites-for-deploying-directaccess"></a>部署 DirectAccess 的必要條件

>適用於：Windows Server （半年通道），Windows Server 2016

下表列出使用 「 組態精靈來部署 DirectAccess 所需的必要條件。  
  
|||  
|-|-|  
|狀況|先決條件|  
|[部署單一 DirectAccess 伺服器使用開始使用的精靈](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Windows 防火牆必須能夠在所有設定檔<br /><br />-只支援執行 Windows 10 的用戶端&reg;， <br />              Windows&reg; 8 和 Windows&reg; 8.1 企業版。<br /><br />-不需要 a 公開金鑰基礎結構。<br /><br />-不支援部署雙因素驗證。 需要網域認證以進行驗證。<br /><br />自動將 DirectAccess 部署到目前網域中的所有攜帶型電腦中。<br /><br />-對網際網路的流量不會透過 DirectAccess。 不支援強制通道組態。<br /><br />-網路位置伺服器 DirectAccess 伺服器。<br /><br />不支援網路存取保護 (NAP)。<br /><br />-不支援使用的功能以外的 DirectAccess 管理主控台或 Windows PowerShell cmdlet 來變更原則。<br /><br />-對於多站台的設定，現在或未來，首先請依照下列中的指導方針[部署單一 DirectAccess 伺服器使用進階設定](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。|  
|[部署單一 DirectAccess 伺服器使用進階設定](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|為必須部署公開金鑰基礎結構。<br />    如需詳細資訊，請參閱[測試實驗室指南小單元：適用於 Windows Server 2012 的基本 PKI](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)。<br /><br />-Windows 防火牆必須能夠在所有設定檔。<br /><br />下列伺服器作業系統支援 DirectAccess。<br /><br />-您可以部署為 DirectAccess 用戶端或 DirectAccess 伺服器的所有版本的 Windows Server 2016。<br />-您可以部署為 DirectAccess 用戶端或 DirectAccess 伺服器的所有版本的 Windows Server 2012 R2。<br />-您可以部署為 DirectAccess 用戶端或 DirectAccess 伺服器的所有版本的 Windows Server 2012。<br />-您可以部署為 DirectAccess 用戶端或 DirectAccess 伺服器的所有版本的 Windows Server 2008 R2。<br /><br />下列用戶端作業系統支援 DirectAccess。<br /><br />-   Windows 10&reg; Enterprise<br />-Windows 10&reg;企業版 2015年長期詞彙維護分支 (LTSB)<br />-Windows&reg; 8 和 8.1 企業版<br />-   Windows&reg; 7 Ultimate<br />-Windows&reg; 7 企業版<br /><br />-強制通道組態不支援使用 KerbProxy 驗證。<br /><br />-不支援使用的功能以外的 DirectAccess 管理主控台或 Windows PowerShell cmdlet 來變更原則。<br /><br />不支援分隔 nat64/dns64 和另一部伺服器的 IP-HTTPS 伺服器角色。|  
  


