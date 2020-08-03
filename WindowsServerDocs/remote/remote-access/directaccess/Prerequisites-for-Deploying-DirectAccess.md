---
title: 部署 DirectAccess 的必要條件
description: 本主題提供在 Windows Server 2016 中部署 DirectAccess 的必要條件。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f1fdae2625245675c2df7b5c5480fe5b08732b4a
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518244"
---
# <a name="prerequisites-for-deploying-directaccess"></a>部署 DirectAccess 的必要條件

>適用於：Windows Server (半年度管道)、Windows Server 2016

下表列出使用設定向導部署 DirectAccess 所需的必要條件。

|狀況|先決條件|
|-|-|
|[使用消費者入門 Wizard 部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-必須在所有設定檔上啟用 Windows 防火牆<p>-僅支援執行 Windows 10 的用戶端 &reg; ， <br />              Windows &reg; 8 和 windows &reg; 8.1 企業版。<p>-不需要公開金鑰基礎結構。<p>-不支援部署雙因素驗證。 需要網域認證以進行驗證。<p>-自動將 DirectAccess 部署到目前網域中的所有行動電腦。<p>-網際網路的流量不會經過 DirectAccess。 不支援強制通道組態。<p>-DirectAccess 伺服器是網路位置伺服器。<p>-不支援網路存取保護（NAP）。<p>-不支援使用 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 以外的功能來變更原則。<p>-若為多網站設定，現在或未來，請先遵循[使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)中的指引。|
|[使用進階設定部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-必須部署公用金鑰基礎結構。<br /> 如需詳細資訊，請參閱[測試實驗室指南迷你模組： Windows Server 2012 的基本 PKI](https://docs.microsoft.com/answers/topics/windows-server-2012.html)。<p>-必須在所有設定檔上啟用 Windows 防火牆。<p>下列伺服器作業系統支援 DirectAccess。<p>-您可以部署所有版本的 Windows Server 2016 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以部署所有版本的 Windows Server 2012 R2 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以部署所有版本的 Windows Server 2012 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以部署所有版本的 Windows Server 2008 R2 做為 DirectAccess 用戶端或 DirectAccess 伺服器。<p>下列用戶端作業系統支援 DirectAccess。<p>-Windows 10 &reg; 企業版<br />-Windows 10 &reg; 企業版2015長期維護分支（LTSB）<br />-Windows &reg; 8 和8.1 企業版<br />-Windows &reg; 7 旗艦版<br />-Windows &reg; 7 企業版<p>-KerbProxy authentication 不支援強制通道設定。<p>-不支援使用 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 以外的功能來變更原則。<p>-不支援在另一部伺服器上分隔 NAT64/DNS64 和 IPHTTPS 伺服器角色。|



