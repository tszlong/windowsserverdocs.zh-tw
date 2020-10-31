---
ms.assetid: c20231dd-2b83-4494-9385-1172272e00d6
title: 決定是否要升級現有網域或部署新網域
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2b7a1bcc6bb157c91b32edd9d4465ac54fb1ea42
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069210"
---
# <a name="determining-whether-to-upgrade-existing-domains-or-deploy-new-domains"></a>決定是否要升級現有網域或部署新網域

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您設計中的每個網域都是新網域或現有的已升級網域。 您未升級之現有網域中的使用者必須移至新網域。

在網域之間移動帳戶可能會影響終端使用者。 在決定是否要將使用者移至新網域或升級現有網域之前，請先評估新 AD DS 網域的長期管理優勢，以避免將使用者移入網域的成本。

如需將 Active Directory 網域升級至 Windows Server 2008 的詳細資訊，請參閱 [將 Active Directory 網域升級為 Windows server 2008 和 Windows server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10))。

如需有關在樹系內和之間重建 AD DS 網域的詳細資訊，請參閱 [ADMT 指南：遷移和重建 Active Directory 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10))。

如需協助您為新的和升級的網域記錄計畫的工作表，請從 [Windows Server 2003 部署套件的工作輔助](https://microsoft.com/download/details.aspx?id=9608) 程式下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟「網域規劃」 ( # A1) 。
