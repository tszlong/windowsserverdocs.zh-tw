---
title: 部署 DirectAccess 的必要條件
description: 本主題提供 Windows Server 2016 中 DirectAccess 部署的必要條件。
manager: brianlic
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 35dd601a1ca961d39352ca226142cc58c550273e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948264"
---
# <a name="prerequisites-for-deploying-directaccess"></a>部署 DirectAccess 的必要條件

>適用於：Windows Server (半年度管道)、Windows Server 2016

下表列出使用設定向導部署 DirectAccess 所需的必要條件。

|案例|先決條件|
|-|-|
|[使用消費者入門 Wizard 部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-必須針對所有設定檔啟用 Windows 防火牆<p>-僅支援執行 Windows 10 的用戶端 &reg; <br />              Windows &reg; 8 與 windows &reg; 8.1 Enterprise。<p>-不需要公開金鑰基礎結構。<p>-不支援部署雙因素驗證。 需要網域認證以進行驗證。<p>-自動將 DirectAccess 部署到目前網域中的所有行動電腦。<p>-網際網路的流量不會通過 DirectAccess。 不支援強制通道組態。<p>-DirectAccess 伺服器是網路位置伺服器。<p>-不支援 (NAP) 的網路存取保護。<p>-不支援使用 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 以外的功能來變更原則。<p>-如果是多網站設定，現在或未來，請先遵循 [使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)中的指導方針。|
|[使用進階設定部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-必須部署公開金鑰基礎結構。<br /> 如需詳細資訊，請參閱 [測試實驗室指南迷你模組：適用于 Windows Server 2012 的基本 PKI](/answers/topics/windows-server-2012.html)。<p>-必須針對所有設定檔啟用 Windows 防火牆。<p>下列伺服器作業系統支援 DirectAccess。<p>-您可以將所有版本的 Windows Server 2016 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以將所有版本的 Windows Server 2012 R2 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以將所有版本的 Windows Server 2012 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。<br />-您可以將所有版本的 Windows Server 2008 R2 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。<p>下列用戶端作業系統支援 DirectAccess。<p>-Windows 10 &reg; Enterprise<br />-Windows 10 &reg; Enterprise 2015 長期維護分支 (LTSB) <br />-Windows &reg; 8 和 8.1 Enterprise<br />-Windows &reg; 7 旗艦版<br />-Windows &reg; 7 企業版<p>-KerbProxy authentication 不支援強制通道設定。<p>-不支援使用 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 以外的功能來變更原則。<p>-不支援在另一部伺服器上分隔 NAT64/DNS64 和 IPHTTPS 伺服器角色。|