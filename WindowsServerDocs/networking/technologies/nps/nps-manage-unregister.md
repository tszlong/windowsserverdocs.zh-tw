---
title: 從 Active Directory 網域取消登錄 NPS
description: 您可以使用本主題中註冊伺服器，NPS 預設網域中，或另一個網域中的 Windows Server 2016 中執行網路原則伺服器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe4773efd89aeb413b3793f874ad6a1b030294a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864349"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>從 Active Directory 網域取消登錄 NPS

>適用於：Windows Server （半年通道），Windows Server 2016

在過程中管理您的 NPS 部署，您可能會發現它可將 NPS 移到另一個網域，來取代 NPS 中，或要淘汰的 NPS。 

當您移動或解除委任 NPS 時，您可以取消註冊 NPS 位置具有讀取 Active Directory 中的使用者帳戶的屬性權限的 Active Directory 網域中的 NPS。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

## <a name="to-unregister-an-nps"></a>若要取消註冊 NPS

1. 在網域控制站，在 伺服器管理員 中，按一下 **工具**，然後按一下**Active Directory 使用者和電腦**。 [Active Directory 使用者和電腦] 主控台隨即開啟。

2. 按一下 **使用者**，然後按兩下**RAS 及 IAS 伺服器**。

3. 按一下 **成員**索引標籤，然後再選取您想要取消註冊的 NPS。

4. 按一下 **移除**，按一下**是**，然後按一下**確定**。

