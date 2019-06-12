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
ms.openlocfilehash: 954393126333bf04d2aa46a01089d88bc91151cb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447319"
---
# <a name="verify-the-hgs-configuration"></a>驗證 HGS 設定

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016


接下來，我們需要驗證一切都運作正常。 若要這樣做，請在提升權限的 Windows PowerShell 主控台中執行下列命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

HGS 設定尚未包含會在受防護網狀架構主機的資訊，因為診斷會指出沒有主機能夠成功證明。 忽略此結果，並檢閱診斷所提供的其他資訊。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

<!-- When a link is available for an updated troubleshooting guide, add a sentence like the following and create a link to the troubleshooting guide:
If failures did occur, please review the remediation steps provided or see the Troubleshooting Guide.
-->

在您的 HGS 叢集中的每個節點上執行的診斷。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [部署受防護主機](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

