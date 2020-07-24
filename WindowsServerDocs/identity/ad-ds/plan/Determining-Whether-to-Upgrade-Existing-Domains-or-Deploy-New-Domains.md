---
ms.assetid: c20231dd-2b83-4494-9385-1172272e00d6
title: 決定是否要升級現有網域或部署新網域
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e4f400e8320b6ff48fcea3289654656eb1ad1418
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966650"
---
# <a name="determining-whether-to-upgrade-existing-domains-or-deploy-new-domains"></a>決定是否要升級現有網域或部署新網域

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您設計中的每個網域都是新的網域或現有的升級網域。 現有網域中的使用者若未升級，則必須移至新網域。

在網域之間移動帳戶可能會影響終端使用者。 在決定是否要將使用者移至新網域或升級現有網域之前，請根據將使用者移至網域的成本，評估新 AD DS 網域的長期管理優勢。

如需將 Active Directory 網域升級至 Windows Server 2008 的詳細資訊，請參閱[將 Active Directory 網域升級至 Windows server 2008 和 Windows server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10))。

如需有關在樹系內和之間重組 AD DS 網域的詳細資訊，請參閱[ADMT 指南：遷移和重組 Active Directory 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10))。

如需協助您記錄新網域和升級網域之計畫的工作表，請從[Windows Server 2003 部署套件的工作輔助程式](https://microsoft.com/download/details.aspx?id=9608)下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開放「網域規劃」（DSSLOGI_5.doc）。
