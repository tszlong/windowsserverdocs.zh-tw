---
title: 在新樹系中安裝 HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: c18c14435c93d08c98b1a765fd820294a273a575
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939542"
---
# <a name="install-hgs-in-a-new-forest"></a>在新樹系中安裝 HGS

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

## <a name="add-the-hgs-server-role"></a>新增 HGS 伺服器角色

在提高許可權的 PowerShell 會話中執行下列命令，以新增 HGS 伺服器角色並安裝 HGS。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)]

## <a name="install-hgs"></a>安裝 HGS

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)]

## <a name="next-steps"></a>後續步驟

- 如需設定 TPM 型證明的後續步驟，請參閱[使用新的專用樹系中的 tpm 模式初始化 HGS 叢集 (預設) ](guarded-fabric-initialize-hgs-tpm-mode-default.md)。
- 如需設定主機金鑰證明的下一個步驟，請參閱[使用新的專用樹系中的金鑰模式初始化 HGS 叢集 (預設) ](guarded-fabric-initialize-hgs-key-mode-default.md)。
- 如需在 Windows Server 2019) 中設定以系統管理員為基礎之證明 (已被取代的後續步驟，請參閱[在新的專用樹系中使用 AD 模式初始化 HGS 叢集 (預設) ](guarded-fabric-initialize-hgs-ad-mode-default.md)。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [初始化 HGS](guarded-fabric-initialize-hgs.md)


