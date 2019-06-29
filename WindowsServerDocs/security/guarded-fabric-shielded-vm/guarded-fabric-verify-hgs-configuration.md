---
title: 驗證 HGS 設定
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8098edd1eea475cea1face5541459b262364a07b
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469549"
---
# <a name="verify-the-hgs-configuration"></a>驗證 HGS 設定

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016


接下來，我們需要驗證一切都運作正常。 若要這樣做，請在提升權限的 Windows PowerShell 主控台中執行下列命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

HGS 設定尚未包含會在受防護網狀架構主機的資訊，因為診斷會指出沒有主機能夠成功證明。 忽略此結果，並檢閱診斷所提供的其他資訊。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

在您的 HGS 叢集中的每個節點上執行的診斷。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [部署受防護主機](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

