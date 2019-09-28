---
title: 初始化 HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 07c40b1da829239dda5210dde0dabe485f659ae0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403586"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>初始化主機守護者服務（HGS）

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

當您初始化 HGS 時，您可以指定 HGS 將用來測量受防護主機健全狀況的模式。 有兩個相互排斥的選項。 如需有關要選擇哪一種模式的背景資訊，請參閱[主控者的受防護網狀架構與受防護的 VM 規劃指南](guarded-fabric-planning-for-hosters.md)。

下列主題涵蓋每種模式的部署步驟：

- [TPM 信任證明（TPM 模式）](guarded-fabric-initialize-hgs-tpm-mode.md)
- [主機金鑰證明（金鑰模式）](guarded-fabric-initialize-hgs-key-mode.md)
- [系統管理員信任的證明（AD 模式）](guarded-fabric-initialize-hgs-ad-mode.md)

您應該在實體伺服器上執行這些步驟。