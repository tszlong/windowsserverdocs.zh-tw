---
title: QoS 常見問題集
description: 本主題提供品質服務 (QoS) 原則，在 Windows Server 2016 中相關問題的解答。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e63cd00a2016411e9f2c532c0e4c301dabdfd816
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-frequently-asked-questions"></a>QoS 原則常見問題集

>適用於：Windows Server（以每年次管道）、Windows Server 2016

以下被常見問題問題 – 與 QoS 原則 – 那些問題的解答。
  
1.  **我網域控制站會需要何種作業系統來執行使用 QoS 原則？**
  
     Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008

2.  **何種作業系統支援 QoS 原則的應用程式的使用者或電腦？**

     您可以將 QoS 原則套用到使用者或執行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的電腦。

3.  **QoS 原則套用到寄件者或收件者的流量嗎？**

     必須影響其輸出流量傳送的電腦上套用原則 QoS。 為了影響雙向流量的兩部電腦，QoS 原則需要套用至兩部電腦。

4.  **萬一發生衝突 QoS 原則部署至該相同電腦？**  
  
     如果多項原則套用，更特定 QoS 原則優先。 例如，原則主機狀態的地址 (192.168.4.12）取得套用而較特定網路位址 (192.168.0.0 月 16）。 如果電腦層級和使用者層級原則相同的特定，而不是電腦層級 QoS 原則套用使用者層級 QoS 原則。 

5.  **根據預設功能的 QoS 原則嗎？**

     否，預設不被支援 QoS 原則。 您必須建立以手動方式，可讓 QoS QoS 原則。  如需詳細資訊，請查看[管理 QoS 原則](qos-policy-manage.md)。

本指南中第一次主題，請查看[品質服務 (QoS) 原則](qos-policy-top.md)。
