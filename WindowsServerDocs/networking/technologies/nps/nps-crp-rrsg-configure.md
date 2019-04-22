---
title: 設定遠端 RADIUS 伺服器群組
description: 本主題提供有關如何在 Windows Server 2016 中的網路原則伺服器設定遠端 RADIUS 伺服器群組的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 02088a35196c0bfadeb65e8971a47fdcc741258d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818759"
---
# <a name="configure-remote-radius-server-groups"></a>設定遠端 RADIUS 伺服器群組

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來設定遠端 RADIUS 伺服器群組，當您想要設定 NPS 做為 proxy 伺服器和連線要求轉寄給其他 NPSs 進行處理。

## <a name="add-a-remote-radius-server-group"></a>新增遠端 RADIUS 伺服器群組

您可以使用此程序，在 網路原則伺服器 (NPS) 嵌入式管理單元加入新的遠端 RADIUS 伺服器群組。

當您設定 NPS 做為 RADIUS proxy 時，您會建立新的連線要求原則，NPS 用來判斷哪些連線要求轉送到其他 RADIUS 伺服器。 此外，連線要求原則設定指定的遠端 RADIUS 伺服器群組，其中包含一或多個 RADIUS 伺服器，會告訴 NPS 將連線要求傳送到符合連線要求原則的位置。

>[!NOTE]
>您也可以在建立新的連線要求原則的程序期間設定新的遠端 RADIUS 伺服器群組。

若要完成此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-add-a-remote-radius-server-group"></a>若要新增遠端 RADIUS 伺服器群組 

1. 在 [伺服器管理員] 中，按一下**工具**，然後按一下**網路原則伺服器**開啟 NPS 主控台。
2. 在主控台樹狀目錄中，按兩下**RADIUS 用戶端和伺服器**，以滑鼠右鍵按一下**遠端 RADIUS 伺服器群組**，然後按一下**新增**。
3. **新增遠端 RADIUS 伺服器群組**對話方塊隨即開啟。 在 **群組名稱**，輸入遠端 RADIUS 伺服器群組的名稱。
4. **在 RADIUS 伺服器**，按一下**新增**。 **新增 RADIUS 伺服器**對話方塊隨即開啟。 輸入您想要新增至群組，RADIUS 伺服器的 IP 位址，或輸入完整網域名稱\(FQDN\)以及與的 RADIUS 伺服器，然後按一下**確認**。
5. 在 **新增 RADIUS 伺服器**，按一下**驗證/帳戶處理** 索引標籤。在 **共用祕密**並**確認共用的密碼**，輸入共用的密碼。 當您設定本機電腦上遠端 RADIUS 伺服器的 RADIUS 用戶端時，您必須使用相同的共用的密碼。
6. 如果您不會使用可延伸驗證通訊協定 (EAP) 驗證，請按一下**要求必須包含訊息驗證者屬性**。 根據預設，EAP 使用訊息驗證者屬性。
7. 驗證的驗證和帳戶處理連接埠號碼是正確的部署。
8. 如果您在使用計量，針對不同的共用的密碼**會計**，清除**驗證和帳戶處理使用的相同共用的密碼**核取方塊，，然後輸入 帳戶處理共用的密碼在**共用祕密**並**確認共用的密碼**。
9. 如果您不想要轉送網路存取伺服器啟動和停止訊息給遠端 RADIUS 伺服器上，清除**轉送網路存取伺服器啟動和停止通知到這個伺服器**核取方塊。

如需管理 NPS 的詳細資訊，請參閱 <<c0> [ 管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。

