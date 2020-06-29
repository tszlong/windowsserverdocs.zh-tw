---
title: Hyper-V 的第 1 代虛擬機器安全性設定
description: 說明 Hyper-V 管理員針對第 1 代虛擬機器提供的安全性設定
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f745ccd9e5a82aa79fb58798f233bf2662b00a70
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475635"
---
# <a name="generation-1-virtual-machine-security-settings"></a>第 1 代虛擬機器安全性設定

>適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

使用 Hyper-V 管理員中的第 1 代虛擬機器安全性設定，有助於保護虛擬機器的資料與狀態。

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Hyper-V 管理員中的加密支援設定

您可以選取下列加密支援選項，以利保護虛擬機器的資料和狀態。

- **將狀態和虛擬機器的移轉流量加密** - 將寫入到磁碟時的虛擬機器已儲存狀態以及即時的移轉流量加密。

若要啟用此選項，您必須為虛擬機器新增金鑰存放磁碟機。

## <a name="key-storage-drive-in-hyper-v-manager"></a>Hyper-V 管理員中的金鑰存放磁碟機

金鑰存放磁碟機會為虛擬機器提供小型磁碟機，以供儲存 BitLocker 金鑰。 這可讓虛擬機器將其作業系統磁碟加密，而不需要使用虛擬化的信賴平台模組 (TPM) 晶片。 金鑰存放磁碟機的內容會使用金鑰保護裝置進行加密。 金鑰保護裝置會授權 Hyper-V 主機執行虛擬機器。 金鑰存放磁碟機和金鑰保護裝置的內容都會儲存為虛擬機器執行階段狀態的一部分。

若要將金鑰存放磁碟機的內容解密並啟動虛擬機器，Hyper-V 主機必須是：

- 此虛擬機器已授權的受防護網狀架構一部分，或
- 從其中一個虛擬機器的守護者獲得私密金鑰。

若要深入了解受防護網狀架構，請參閱[安全性和保證](../../../security/Security-and-Assurance.md)中的＜受防護的 VM 簡介＞一節。

您可以在其中一個虛擬機器的 IDE 控制器上，將金鑰存放磁碟機新增到空的插槽。 若要這麼做，請按一下 [新增金鑰存放磁碟機] 以將金鑰存放磁碟機新增到此虛擬機器的第一個可用 IDE 控制器插槽。

## <a name="additional-references"></a>其他參考資料

- [Hyper-V 管理員中的第 2 代虛擬機器安全性設定](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [安全性和保證](../../../security/Security-and-Assurance.md)