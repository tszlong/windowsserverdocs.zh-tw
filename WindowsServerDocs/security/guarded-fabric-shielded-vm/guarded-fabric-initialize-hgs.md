---
description: 深入瞭解： (HGS) 初始化主機守護者服務
title: 初始化 HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: f2dc8999a851aa3830a0ffb8e17c3eb9909fe2d1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047256"
---
# <a name="initialize-the-host-guardian-service-hgs"></a> (HGS) 初始化主機守護者服務

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

當您初始化 HGS 時，會指定 HGS 將用來測量受防護主機健全狀況的模式。 有兩個互斥的選項。 如需有關要選擇哪一種模式的背景資訊，請參閱 [主控者的受防護網狀架構與受防護的 VM 規劃指南](guarded-fabric-planning-for-hosters.md)。

下列主題涵蓋每種模式的部署步驟：

- [TPM-信任的證明 (TPM 模式) ](guarded-fabric-initialize-hgs-tpm-mode.md)
- [主機金鑰證明 (金鑰模式) ](guarded-fabric-initialize-hgs-key-mode.md)
- [系統管理員-信任的證明 (AD 模式) ](guarded-fabric-initialize-hgs-ad-mode.md)

您應該在實體伺服器上執行這些步驟。
