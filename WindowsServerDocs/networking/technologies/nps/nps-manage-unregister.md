---
title: 從 Active Directory 網域取消登錄 NPS
description: 您可以使用本主題，在 NPS 預設網域或其他網域中的 Windows Server 2016 中，註冊執行網路原則伺服器的伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: aff5090a5f9fcd2791835f21e2caf1c0dccaa510
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949334"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>從 Active Directory 網域取消登錄 NPS

>適用於：Windows Server (半年度管道)、Windows Server 2016

在管理 NPS 部署的過程中，您可能會發現將 NPS 移至另一個網域、取代 NPS 或淘汰 NPS 很有用。

當您移動或解除委任 NPS 時，可以取消註冊 Active Directory 網域中的 NPS，其中 NPS 有權讀取 Active Directory 中的使用者帳戶屬性。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

## <a name="to-unregister-an-nps"></a>取消註冊 NPS

1. 在網域控制站上的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 Active Directory 消費者和電腦主控台隨即開啟。

2. 按一下 [ **使用者**]，然後按兩下 [ **RAS 和 資訊存取伺服器**]。

3. 按一下 [ **成員** ] 索引標籤，然後選取您要取消註冊的 NPS。

4. 按一下 [ **移除**]，按一下 [ **是**]，然後按一下 **[確定]**。

