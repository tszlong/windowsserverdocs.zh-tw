---
title: Hyper-v 的第1代虛擬機器安全性設定
description: 說明適用于第1代虛擬機器之 Hyper-v 管理員的安全性設定
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: ceb3c2628546815f9b0af35946e173f4276130d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392784"
---
# <a name="generation-1-virtual-machine-security-settings"></a>第1代虛擬機器安全性設定

>適用於：Windows Server 2016，Microsoft Hyper-v Server 2016，Windows Server 2019，Microsoft Hyper-v Server 2019

使用 [Hyper-v 管理員] 中的第1代虛擬機器安全性設定，協助保護虛擬機器的資料與狀態。

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Hyper-v 管理員中的加密支援設定

您可以選取下列 [加密支援] 選項，以協助保護虛擬機器的資料和狀態。

- **加密狀態和虛擬機器遷移流量**-當虛擬機器的儲存狀態寫入磁片和即時移轉流量時，將其加密。

若要啟用此選項，您必須為虛擬機器新增金鑰存放磁片磁碟機。

## <a name="key-storage-drive-in-hyper-v-manager"></a>Hyper-v 管理員中的金鑰存放磁片磁碟機

金鑰存放磁片磁碟機提供小型磁片磁碟機給虛擬機器，以儲存 BitLocker 金鑰。 這可讓虛擬機器加密其作業系統磁片，而不需要虛擬化的信賴平臺模組（TPM）晶片。 金鑰存放磁片磁碟機的內容會使用金鑰保護裝置進行加密。 金鑰保護裝置會 authories Hyper-v 主機來執行虛擬機器。 金鑰存放磁片磁碟機和金鑰保護裝置的內容都會儲存為虛擬機器的執行時間狀態的一部分。

若要將金鑰存放磁片磁碟機的內容解密並啟動虛擬機器，Hyper-v 主機必須是：

- 此虛擬機器之已授權的受防護網狀架構的一部分，或
- 讓其中一個虛擬機器的監護人擁有私密金鑰。

若要深入瞭解受防護網狀架構，請參閱[安全性和保證](../../../security/Security-and-Assurance.md)中的受防護 vm 簡介一節。

您可以在其中一個虛擬機器的 IDE 控制器上，將金鑰存放磁片磁碟機新增到空的插槽。 若要這麼做，請按一下 [**新增金鑰存放磁片磁碟機**]，將金鑰存放磁片磁碟機新增到此虛擬機器的第一個可用 IDE 控制器插槽。

## <a name="see-also"></a>另請參閱

- [Hyper-v 管理員中的第2代虛擬機器安全性設定](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [安全性和保證](../../../security/Security-and-Assurance.md)