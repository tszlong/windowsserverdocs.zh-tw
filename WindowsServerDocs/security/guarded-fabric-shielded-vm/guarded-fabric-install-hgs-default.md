---
description: 深入瞭解：在新樹系中安裝 HGS
title: 在新樹系中安裝 HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 934efb699dd1c9f37219606ebc7f1b748f61eb18
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049726"
---
# <a name="install-hgs-in-a-new-forest"></a>在新樹系中安裝 HGS

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

## <a name="add-the-hgs-server-role"></a>新增 HGS 伺服器角色

在提高許可權的 PowerShell 會話中執行下列命令，以新增 HGS 伺服器角色並安裝 HGS。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)]

## <a name="install-hgs"></a>安裝 HGS

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)]

## <a name="next-steps"></a>後續步驟

- 如需設定以 TPM 為基礎的證明的後續步驟，請參閱 [在新的專用樹系中使用 TPM 模式初始化 HGS 叢集 (預設) ](guarded-fabric-initialize-hgs-tpm-mode-default.md)。
- 如需設定主機金鑰證明的後續步驟，請參閱 [在新的專用樹系中使用金鑰模式初始化 HGS 叢集 (預設) ](guarded-fabric-initialize-hgs-key-mode-default.md)。
- 如需設定以系統管理員為基礎的證明 (在 Windows Server 2019) 中淘汰的後續步驟，請參閱 [在新的專用樹系中使用 AD 模式初始化 HGS 叢集 (預設) ](guarded-fabric-initialize-hgs-ad-mode-default.md)。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [初始化 HGS](guarded-fabric-initialize-hgs.md)


