---
title: QoS 常見問題集
description: 本主題提供 Windows Server 2016 中的服務品質 (QoS) 原則的相關問題的解答。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9fb54cc3bda9a259188ae02fb2ef2836dd8718d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833749"
---
# <a name="qos-policy-frequently-asked-questions"></a>QoS 原則常見問題集

>適用於：Windows Server （半年通道），Windows Server 2016

下列為常見問題集-與解答這些問題 – QoS 原則。
  
1.  **若要執行，才能使用 QoS 原則將我的網域控制站需要在哪些作業系統？**
  
     Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008

2.  **使用者或電腦 QoS 原則的應用程式支援哪些作業系統？**

     您可以套用 QoS 原則到使用者或執行 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012，Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的電腦。

3.  **QoS 原則適用於寄件者或接收者的流量嗎？**

     會影響其輸出流量傳送的電腦上，就必須套用 QoS 原則。 若要影響的兩部電腦的雙向流量，必須套用至這兩部電腦 QoS 原則。

4.  **如果衝突的 QoS 原則會部署到同一部電腦發生什麼事？**  
  
     如果多個原則套用時，更具體的 QoS 原則的優先順序較高。 比方說，取得套用原則，指出主機位址 (192.168.4.12)，而不是較不特定的網路位址 (192.168.0.0/16)。 如果電腦層級和使用者層級的原則有相同的精確性，而不是電腦層級 QoS 原則套用使用者層級 QoS 原則。 

5.  **預設啟用 QoS 原則嗎？**

     否，預設不會啟用 QoS 原則。 您必須建立 QoS 原則，以手動方式啟用 QoS。  如需詳細資訊，請參閱 <<c0> [ 管理 QoS 原則](qos-policy-manage.md)。

如本指南中的第一個主題，請參閱 <<c0> [ 服務品質 (QoS) 原則](qos-policy-top.md)。
