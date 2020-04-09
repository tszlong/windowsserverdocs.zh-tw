---
title: 使用 TPM 信任的證明來初始化 HGS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b8ceebe63a586ec95b502dfea12f99d174549448
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856611"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>使用 TPM 信任的證明來初始化 HGS

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

這些步驟會根據您是在新樹系或現有防禦樹系中初始化 HGS 而有所不同：

1. [在新樹系中初始化 HGS 叢集（預設值）](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   \- 或者 -

   [初始化現有防禦樹系中的 HGS 叢集](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [安裝受信任的 TPM 根憑證](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns.md)

