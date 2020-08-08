---
title: 加入網域的裝置公開金鑰驗證
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.date: 08/18/2017
ms.openlocfilehash: 37710710bc301173503450a46fc1ccc2b0614cf4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943948"
---
# <a name="domain-joined-device-public-key-authentication"></a>加入網域的裝置公開金鑰驗證

>適用于： Windows Server 2016、Windows 10

Kerberos 新增了已加入網域的裝置支援，以使用從 Windows Server 2012 和 Windows 8 開始的憑證進行登入。 這種變更可讓協力廠商的廠商建立解決方案，以布建及初始化已加入網域之裝置的憑證，以用於網域驗證。

## <a name="automatic-public-key-provisioning"></a>自動公開金鑰布建

從 Windows 10 版本1507和 Windows Server 2016 開始，已加入網域的裝置會自動將系結公開金鑰布建到 Windows Server 2016 網域控制站 (DC) 。 布建金鑰後，Windows 就可以對網域使用公開金鑰驗證。

### <a name="key-generation"></a>金鑰產生
如果裝置正在執行 Credential Guard，則會建立公開/私密金鑰組，並受到 Credential Guard 的保護。

如果 Credential Guard 無法使用，而且 TPM 為，則會建立受 TPM 保護的公開/私密金鑰組。

如果兩者都無法使用，則不會產生金鑰組，且裝置只能使用密碼進行驗證。

### <a name="provisioning-computer-account-public-key"></a>布建電腦帳戶公開金鑰
當 Windows 啟動時，它會檢查是否已為其電腦帳戶布建公開金鑰。 如果不是，則會產生系結的公開金鑰，並使用 Windows Server 2016 或更高的 DC 為其在 AD 中的帳戶進行設定。 如果所有 Dc 都已關閉層級，則不會布建任何金鑰。

### <a name="configuring-device-to-only-use-public-key"></a>正在將裝置設定為只使用公開金鑰
如果群組原則設定 [**使用憑證的裝置驗證支援**] 設定為 [**強制**]，則裝置必須尋找執行 Windows Server 2016 或更新版本的 DC 以進行驗證。 設定在系統管理範本 > 系統 > Kerberos 之下。

### <a name="configuring-device-to-only-use-password"></a>正在將裝置設定為只使用密碼
如果已停用 [**使用憑證的裝置驗證支援**] 群組原則設定，則一律會使用 [密碼]。 設定在系統管理範本 > 系統 > Kerberos 之下。

## <a name="domain-joined-device-authentication-using-public-key"></a>使用公開金鑰的已加入網域裝置驗證
當 Windows 具有已加入網域之裝置的憑證時，Kerberos 會先使用憑證和失敗的密碼重試進行驗證。 這可讓裝置向下層 Dc 進行驗證。

因為自動布建的公開金鑰具有自我簽署憑證，所以憑證驗證會在不支援金鑰信任帳戶對應的網域控制站上失敗。 根據預設，Windows 會使用裝置的網域密碼來重試驗證。


