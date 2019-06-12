---
title: 安裝新樹系中的 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 84f88d96f1e16767dec3b21b34aa226e544afaac
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447401"
---
# <a name="install-hgs-in-a-new-forest"></a>安裝新樹系中的 HGS 

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

## <a name="add-the-hgs-server-role"></a>新增 HGS 伺服器角色

在新增 HGS 伺服器角色，並安裝 HGS 已提升權限的 PowerShell 工作階段中執行下列命令。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>安裝 HGS 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>後續步驟

- 若要設定 TPM 型證明的下一個步驟，請參閱[初始化新的專用樹系 （預設值） 中使用 TPM 模式 HGS 叢集](guarded-fabric-initialize-hgs-tpm-mode-default.md)。
- 設定主機金鑰證明的下一個步驟，請參閱 <<c0> [ 初始化新的專用樹系 （預設值） 中使用索引鍵模式 HGS 叢集](guarded-fabric-initialize-hgs-key-mode-default.md)。
- 下一個步驟來設定 以系統管理員為基礎的證明，（在 Windows Server 2019 中已被取代），請參閱[初始化新的專用樹系 （預設值） 中使用 AD 模式 HGS 叢集](guarded-fabric-initialize-hgs-ad-mode-default.md)。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [初始化 HGS](guarded-fabric-initialize-hgs.md)


