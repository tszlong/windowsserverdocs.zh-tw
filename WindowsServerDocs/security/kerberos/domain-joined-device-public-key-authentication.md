---
title: 加入網域的裝置公開金鑰驗證
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 80906e7cfe3740200938704a4b4eaf0759af303a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885029"
---
# <a name="domain-joined-device-public-key-authentication"></a>加入網域的裝置公開金鑰驗證

>適用於：Windows Server 2016 中，Windows 10

Kerberos 已新增支援已加入網域的裝置登入 Windows Server 2012 和 Windows 8 中使用憑證開始。 這項變更可讓第 3 個合作對象廠商建立解決方案，來佈建，並初始化讓已加入網域的裝置使用的網域驗證的憑證。 

## <a name="automatic-public-key-provisioning"></a>自動公用金鑰佈建

從 Windows 10 1507年版和 Windows Server 2016 開始，已加入網域的裝置自動佈建 Windows Server 2016 網域控制站 (DC) 的繫結公開公鑰。 一旦佈建金鑰時，Windows 可以加入網域使用公用金鑰驗證。

### <a name="public-key-generation"></a>公用金鑰產生
如果裝置正在執行 Credential Guard，公開金鑰會建立受到 Credential Guard。 

如果不使用 Credential Guard 和 TPM 是，公開金鑰會建立受 TPM 所保護。 

如果兩者都沒有作用，然後不會產生一個金鑰和裝置都只能驗證使用的密碼。

### <a name="provisioning-computer-account-public-key"></a>佈建電腦帳戶的公用金鑰
當 Windows 啟動時，它會檢查公開金鑰已佈建其電腦帳戶。 如果沒有，然後產生的繫結的公用金鑰，並將它設定為其帳戶中使用 Windows Server 2016 或更新版本的 DC 的 AD。 如果所有網域控制站是舊版，則沒有索引鍵是佈建。

### <a name="configuring-device-to-only-use-public-key"></a>設定為只使用公開金鑰的裝置
如果群組原則設定**支援使用憑證進行裝置驗證**設為**強制**，則裝置必須尋找 DC，執行 Windows Server 2016 或更新版本來驗證。 此設定是在系統管理範本 > System > Kerberos。

### <a name="configuring-device-to-only-use-password"></a>設定為只使用密碼的裝置
如果群組原則設定**使用憑證進行裝置驗證支援**已停用，則一律會使用密碼。 此設定是在系統管理範本 > System > Kerberos。

## <a name="domain-joined-device-authentication-using-public-key"></a>使用公開金鑰已加入網域的裝置驗證
當 Windows 有已加入網域的裝置憑證時，Kerberos 首先會驗證使用憑證和密碼的失敗重試。 這可讓裝置向下層網域控制站。

由於自動佈建的公用金鑰有自我簽署的憑證，憑證驗證會失敗並不支援金鑰信任帳戶對應的網域控制站上。 根據預設，Windows 會重試使用裝置的網域密碼進行驗證。


