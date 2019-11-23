---
title: 設定遠端 RADIUS 伺服器群組
description: 本主題提供有關如何在 Windows Server 2016 的網路原則伺服器中設定遠端 RADIUS 伺服器群組的資訊。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe34d25d2b54b02bb56fcad99c433054a309f60b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405455"
---
# <a name="configure-remote-radius-server-groups"></a>設定遠端 RADIUS 伺服器群組

>適用於：Windows Server (半年通道)、Windows Server 2016

當您想要設定 NPS 作為 proxy 伺服器，並將連線要求轉送到其他 Nps 進行處理時，可以使用本主題來設定遠端 RADIUS 伺服器群組。

## <a name="add-a-remote-radius-server-group"></a>新增遠端 RADIUS 伺服器群組

您可以使用這個程式，在網路原則伺服器（NPS）嵌入式管理單元中新增遠端 RADIUS 伺服器群組。

當您將 NPS 設定為 RADIUS proxy 時，您會建立新的連線要求原則，NPS 會使用它來判斷要轉送至其他 RADIUS 伺服器的連線要求。 此外，您可以指定包含一或多個 RADIUS 伺服器的遠端 RADIUS 伺服器群組來設定連線要求原則，這會告訴 NPS 在何處傳送符合連線要求原則的連線要求。

>[!NOTE]
>您也可以在建立新連線要求原則的過程中，設定新的遠端 RADIUS 伺服器群組。

若要完成此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-add-a-remote-radius-server-group"></a>新增遠端 RADIUS 伺服器群組 

1. 在伺服器管理員中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**] 以開啟 NPS 主控台。
2. 在主控台樹中，按兩下 [ **RADIUS 用戶端和伺服器**]，在 [**遠端 RADIUS 伺服器群組**] 上按一下滑鼠右鍵，然後按一下 [**新增**]。
3. [**新增遠端 RADIUS 伺服器群組**] 對話方塊隨即開啟。 在 [**組名**] 中，輸入遠端 RADIUS 伺服器群組的名稱。
4. **在 [RADIUS 伺服器] 中**，按一下 [**新增**]。 [**新增 RADIUS 伺服器**] 對話方塊隨即開啟。 輸入您想要新增至群組的 RADIUS 伺服器的 IP 位址，或輸入 RADIUS 伺服器 \(FQDN\) 的完整功能變數名稱，然後按一下 [**驗證**]。
5. 在 [**新增 RADIUS 伺服器**] 中，按一下 [**驗證/帳戶**處理] 索引標籤。在 [**共用密碼**] 和 [**確認共用密碼**] 中，輸入共用密碼。 當您將本機電腦設定為遠端 RADIUS 伺服器上的 RADIUS 用戶端時，必須使用相同的共用密碼。
6. 如果您不是使用可延伸的驗證通訊協定（EAP）進行驗證，請按一下 **[要求必須包含訊息驗證者屬性**]。 根據預設，EAP 會使用訊息驗證器屬性。
7. 請確認您的部署的驗證和帳戶處理埠號碼是否正確。
8. 如果您使用不同的共用密碼進行帳戶處理，請在 [**帳戶**處理] 中，清除 [**使用相同的共用密碼進行驗證和帳戶**處理] 核取方塊，然後在 [**共用密碼**] 和 [**確認共用密碼**] 中輸入帳戶共用密碼。
9. 如果您不想要轉寄網路存取伺服器啟動和停止訊息到遠端 RADIUS 伺服器，請清除 [**轉寄網路存取伺服器啟動和停止通知到這部伺服器**] 核取方塊。

如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。

