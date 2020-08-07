---
title: 從 Active Directory 網域取消登錄 NPS
description: 您可以使用本主題，在 NPS 預設網域或另一個網域中的 Windows Server 2016 中註冊執行網路原則伺服器的伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 71bb0328e7265ad6981cdb3089e80572315b0fdd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952104"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>從 Active Directory 網域取消登錄 NPS

>適用於：Windows Server (半年度管道)、Windows Server 2016

在管理 NPS 部署的過程中，您可能會發現將 NPS 移至另一個網域、取代 NPS 或淘汰 NPS 會很有説明。

當您移動或解除委任 NPS 時，您可以取消註冊 Active Directory 網域中的 nps，其中 NPS 有權讀取 Active Directory 中使用者帳戶的屬性。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

## <a name="to-unregister-an-nps"></a>取消註冊 NPS

1. 在網域控制站的伺服器管理員中，按一下 [**工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 [Active Directory 使用者和電腦] 主控台隨即開啟。

2. 按一下 [**使用者**]，然後按兩下 [ **RAS 和 資訊存取伺服器**]。

3. 按一下 [**成員**] 索引標籤，然後選取您要取消註冊的 NPS。

4. 按一下 [**移除**]，按一下 **[是]**，然後按一下 **[確定]**。

