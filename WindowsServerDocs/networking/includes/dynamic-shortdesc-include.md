---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms:topic: include
ms.openlocfilehash: f07840220dbbe955a47879b3090ddd340c8c514f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952822"
---
使用動態時，輸出負載是根據 TCP 通訊埠和 IP 位址的雜湊來散發。 動態模式也會即時重新平衡載入，讓指定的輸出流程可以在小組成員之間來回移動。 相反地，輸入負載會以與 Hyper-v 埠相同的方式進行散發。 簡言之，動態模式會利用位址雜湊和 Hyper-v 埠的最佳層面，而且是最高效能的負載平衡模式。

