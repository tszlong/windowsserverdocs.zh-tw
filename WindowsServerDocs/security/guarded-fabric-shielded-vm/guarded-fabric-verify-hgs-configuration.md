---
title: 驗證 HGS 設定
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d219b7aa9ca1e17df3281fd756106a6f07864116
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386355"
---
# <a name="verify-the-hgs-configuration"></a>驗證 HGS 設定

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016


接下來，我們需要驗證是否如預期般運作。 若要這麼做，請在提升許可權的 Windows PowerShell 主控台中執行下列命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

由於 HGS 設定尚未包含將在受防護網狀架構中的主機相關資訊，因此診斷會指出沒有主機能夠成功證明。 忽略此結果，並檢查診斷所提供的其他資訊。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

在 HGS 叢集中的每個節點上執行診斷。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [部署受防護主機](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

