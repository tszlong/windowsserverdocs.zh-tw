---
title: 部署 DirectAccess 的必要條件
description: 本主題提供在 Windows Server 2016 中部署 DirectAccess 的必要條件。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 14da51a2617764f2ee11eedcbc7a4e10b3b0da15
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309318"
---
# <a name="prerequisites-for-deploying-directaccess"></a>部署 DirectAccess 的必要條件

>適用於：Windows Server (半年通道)、Windows Server 2016

下表列出使用設定向導部署 DirectAccess 所需的必要條件。  
  
|||  
|-|-|  
|案例|必要條件|  
|[使用消費者入門 Wizard 部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-必須在所有設定檔上啟用 Windows 防火牆<br /><br />-僅支援執行 Windows 10&reg;的用戶端、 <br />              Windows&reg; 8 和 Windows&reg; 8.1 企業版。<br /><br />-不需要公開金鑰基礎結構。<br /><br />-不支援部署雙因素驗證。 需要網域認證以進行驗證。<br /><br />-自動將 DirectAccess 部署到目前網域中的所有行動電腦。<br /><br />-網際網路的流量不會經過 DirectAccess。 不支援強制通道組態。<br /><br />-DirectAccess 伺服器是網路位置伺服器。<br /><br />-不支援網路存取保護（NAP）。<br /><br />-不支援使用 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 以外的功能來變更原則。<br /><br />-若為多網站設定，現在或未來，請先遵循[使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)中的指引。|  
|[使用進階設定部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-必須部署公用金鑰基礎結構。<br />    如需詳細資訊，請參閱[測試實驗室指南迷你模組： Windows Server 2012 的基本 PKI](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)。<br /><br />-必須在所有設定檔上啟用 Windows 防火牆。<br /><br />下列伺服器作業系統支援 DirectAccess。<br /><br />-您可以部署所有版本的 Windows Server 2016 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以部署所有版本的 Windows Server 2012 R2 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以部署所有版本的 Windows Server 2012 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以部署所有版本的 Windows Server 2008 R2 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<br /><br />下列用戶端作業系統支援 DirectAccess。<br /><br />-Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 長期維護分支（LTSB）<br />-Windows&reg; 8 和8.1 企業版<br />-Windows&reg; 7 旗艦版<br />-Windows&reg; 7 企業版<br /><br />-KerbProxy authentication 不支援強制通道設定。<br /><br />-不支援使用 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 以外的功能來變更原則。<br /><br />-不支援在另一部伺服器上分隔 NAT64/DNS64 和 IPHTTPS 伺服器角色。|  
  


