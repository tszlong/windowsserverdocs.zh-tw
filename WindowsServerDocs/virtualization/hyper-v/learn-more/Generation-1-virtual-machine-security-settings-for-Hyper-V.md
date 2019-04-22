---
title: 適用於 HYPER-V 的第 1 代虛擬機器安全性設定
description: 描述第 1 代虛擬機器的 HYPER-V Manager 中可用的安全性設定
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 923216142a45071bc3623e3f37b37cc6a2361f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812139"
---
# <a name="generation-1-virtual-machine-security-settings"></a>第 1 代虛擬機器的安全性設定

>適用於：Windows Server 2016、 Microsoft HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

使用 HYPER-V 管理員中的第 1 代虛擬機器的安全性設定，來協助保護資料和虛擬機器的狀態。

## <a name="encryption-support-settings-in-hyper-v-manager"></a>在 HYPER-V 管理員中的加密支援設定

您可以協助保護資料和虛擬機器的狀態，藉由選取下列的 [加密] 支援選項。

- **加密狀態與虛擬機器移轉流量**-寫入到磁碟和即時移轉流量時，會加密儲存的虛擬機器的狀態。

若要啟用此選項，您必須新增虛擬機器的主要儲存體磁碟機。

## <a name="key-storage-drive-in-hyper-v-manager"></a>在 HYPER-V 管理員中的金鑰儲存磁碟機

金鑰的儲存體磁碟機提供小的磁碟機的 BitLocker 金鑰會儲存虛擬機器。 這可讓虛擬機器，以加密其作業系統磁碟，而不需要虛擬化的信賴平台模組 (TPM) 晶片。 使用金鑰保護裝置會加密金鑰的儲存體磁碟機的內容。 金鑰保護裝置 authories HYPER-V 主機執行虛擬機器。 這兩個金鑰的儲存體磁碟機和金鑰保護裝置的內容會儲存為虛擬機器的執行階段狀態的一部分。

若要解密的金鑰儲存體磁碟機的內容，並啟動虛擬機器，HYPER-V 主機必須是：

- 此虛擬機器中，已獲授權的受防護網狀架構的一部分或
- 其中一個虛擬機器的守護者具有私密金鑰。

若要深入了解受防護網狀架構，請參閱簡介受防護的 Vm 一節[安全性和保證](../../../security/Security-and-Assurance.md)。

您可以將金鑰儲存體磁碟機加入其中一個虛擬機器的 IDE 控制器上的空插槽。 若要這樣做，請按一下**將金鑰儲存體磁碟機新增**將金鑰儲存體磁碟機新增至這部虛擬機器的第一個免費的 IDE 控制器位置。

##<a name="see-also"></a>另請參閱

- [在 HYPER-V 管理員中的第 2 代虛擬機器安全性設定](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [安全性和保證](../../../security/Security-and-Assurance.md)