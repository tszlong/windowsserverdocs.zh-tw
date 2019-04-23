---
title: 初始化 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c689a1d5f69731db0cb85a884f5af2ee0230e7be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865449"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>初始化 「 主機守護者服務 (HGS)

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

當您初始化 HGS 時，您會指定 HGS 將用來測量的受防護主機的健全狀況的模式。 有兩個互斥的選項。 如需選擇哪一種模式的背景資訊，請參閱[受防護網狀架構與受防護的 VM 規劃指南的主機服務提供者](guarded-fabric-planning-for-hosters.md)。

下列主題涵蓋每一種模式的部署步驟：

- [TPM 信任證明 （TPM 模式）](guarded-fabric-initialize-hgs-tpm-mode.md)
- [主機金鑰證明 （索引鍵模式）](guarded-fabric-initialize-hgs-key-mode.md)
- [系統管理信任證明 （AD 模式）](guarded-fabric-initialize-hgs-ad-mode.md)

您必須在實體伺服器上執行這些步驟。