---
title: 設定遠端 RADIUS 伺服器群組
description: 本主題提供有關如何在 Windows Server 2016 中設定網路原則伺服器的遠端 RADIUS 伺服器群組的資訊。
manager: brianlic
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 7513b09a6be162894645fb5211563cfa7c558ad0
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949014"
---
# <a name="configure-remote-radius-server-groups"></a>設定遠端 RADIUS 伺服器群組

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您想要將 NPS 設定為 proxy 伺服器，並將連線要求轉送到其他 NPSs 進行處理時，可以使用本主題來設定遠端 RADIUS 伺服器群組。

## <a name="add-a-remote-radius-server-group"></a>新增遠端 RADIUS 伺服器群組

您可以使用此程序在網路原則伺服器 (NPS) 嵌入式管理單元中新增遠端 RADIUS 伺服器群組。

將 NPS 設定為 RADIUS Proxy 時，您會建立新的連線要求原則，NPS 使用此原則決定將哪些連線要求轉送到其他 RADIUS 伺服器。 此外，您也可以指定包含一或多個 RADIUS 伺服器的遠端 RADIUS 伺服器群組來設定連線要求原則，告訴 NPS 要傳送符合連線要求原則的連接要求。

>[!NOTE]
>您也可以在建立新連線要求原則程序期間，設定新的遠端 RADIUS 伺服器群組。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

### <a name="to-add-a-remote-radius-server-group"></a>新增遠端 RADIUS 伺服器群組

1. 在伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器** ] 以開啟 NPS 主控台。
2. 在主控台樹中，按兩下 [ **RADIUS 用戶端和伺服器**]，在 [ **遠端 RADIUS 伺服器群組**] 上按一下滑鼠右鍵，然後按一下 [ **新增**]。
3. [ **新增遠端 RADIUS 伺服器群組** ] 對話方塊隨即開啟。 在 [ **組名**] 中，輸入遠端 RADIUS 伺服器群組的名稱。
4. **在 [RADIUS 伺服器] 中**，按一下 [ **新增**]。 [ **新增 RADIUS 伺服器** ] 對話方塊隨即開啟。 輸入您想要新增至群組之 RADIUS 伺服器的 IP 位址，或輸入 RADIUS 伺服器的完整功能變數名稱稱 \( FQDN \) ，然後按一下 [ **驗證**]。
5. 在 [ **新增 RADIUS 伺服器**] 中，按一下 [ **驗證/帳戶** 處理] 索引標籤。在 [ **共用密碼** ] 和 [ **確認共用密碼**] 中，輸入共用密碼。 將本機電腦設定為遠端 RADIUS 伺服器上的 RADIUS 用戶端時，您必須使用相同的共用密碼。
6. 如果您不是使用可延伸的驗證通訊協定 (EAP) 進行驗證，請按一下 **[要求必須包含訊息驗證者屬性**]。 EAP 預設會使用 Message-Authenticator 屬性。
7. 確認您所部署的驗證和帳戶處理連接埠號碼是正確的。
8. 如果您使用不同的共用密碼進行計量，請在 [ **帳戶** 處理] 中清除 [ **使用相同的共用密碼進行驗證和帳戶** 處理] 核取方塊，然後在 [ **共用密碼** ] 中輸入帳戶共用密碼並 **確認共用機密**。
9. 如果您不想要將網路存取伺服器啟動和停止訊息轉寄到遠端 RADIUS 伺服器，請清除 [將 **網路存取伺服器啟動和停止通知到這部伺服器** ] 核取方塊。

如需有關管理 NPS 的詳細資訊，請參閱 [管理網路原則伺服器](nps-manage-top.md)。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。

