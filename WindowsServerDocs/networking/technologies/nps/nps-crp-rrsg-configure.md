---
title: 設定遠端 RADIUS 伺服器群組
description: 本主題提供如何設定遠端 RADIUS 伺服器群組中的 Windows Server 2016 的網路原則伺服器的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f293fd18176115365e5e243a90a034676b3262f9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-remote-radius-server-groups"></a>設定遠端 RADIUS 伺服器群組

>適用於：Windows Server（以每年次管道）、Windows Server 2016

當您想要設定 NPS 做為 proxy 伺服器並向前連接要求處理的其他 NPS 伺服器設定遠端 RADIUS 伺服器群組，您可以使用此主題。

## <a name="add-a-remote-radius-server-group"></a>新增遠端 RADIUS 伺服器群組

若要新增新的遠端 RADIUS 伺服器群組的網路原則 Server (NPS) 嵌入式管理單元，您可以使用此程序。

當您設定 NPS RADIUS proxy 為時，您建立新連接要求原則 NPS 用來判斷哪一個連接要求轉寄給其他 RADIUS 伺服器。 此外，連接要求原則藉由遠端 RADIUS 伺服器群組，其中包含一個或多個 RADIUS 伺服器、告訴 NPS 傳送連接要求符合連接要求原則的位置。

>[!NOTE]
>您也可以在建立新連接要求原則的程序期間設定新的遠端 RADIUS 伺服器群組。

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

### <a name="to-add-a-remote-radius-server-group"></a>若要新增的遠端 RADIUS 伺服器群組 

1. 在伺服器管理員中，按一下**工具**，然後按一下 [**的網路原則伺服器**打開 NPS 主機。
2. 在主控台按兩下 [ **RADIUS 戶端與伺服器**，以滑鼠右鍵按一下**遠端 RADIUS 伺服器群組**，，然後按一下 [**新**。
3. **新遠端 RADIUS 伺服器群組**對話方塊。 在**群組名稱**，輸入遠端 RADIUS 伺服器群組的名稱。
4. **中 RADIUS 伺服器]**，按一下 [**新增]**。 **新增 RADIUS 伺服器]**對話方塊。 輸入您想要加入該群組，或輸入 RADIUS 伺服器的完整網域名稱 \(FQDN\)，然後按一下 RADIUS 伺服器的 IP 位址**確認**。
5. 在**新增 RADIUS 伺服器**，按一下 [**驗證日計量**索引標籤。在**共用密碼**和**確認共用的密碼**，輸入共用的密碼。 當您將會在本機電腦設定為 RADIUS client 遠端 RADIUS 伺服器上，您必須使用相同的共用的密碼。
6. 如果您不使用的驗證延伸驗證通訊協定 (EAP)，請按一下**要求必須包含訊息 authenticator 屬性**。 EAP 使用預設的郵件-Authenticator 屬性。
7. 請確認驗證及計量連接埠號碼的正確的部署。
8. 如果您使用不同的分享的密碼，請在**計量**，清除**使用相同的共用的密碼驗證及計量**核取方塊，並輸入中的計量共用的密碼，然後**共用密碼**和**確認共用的密碼**。
9. 如果您不想轉送網路存取伺服器開始和停止訊息，以遠端 RADIUS 伺服器、清除**向前網路存取伺服器 [開始] 畫面與停止此伺服器通知**核取方塊。

如需有關管理 NPS 的詳細資訊，請查看[管理的網路原則伺服器]](nps-manage-top.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。

