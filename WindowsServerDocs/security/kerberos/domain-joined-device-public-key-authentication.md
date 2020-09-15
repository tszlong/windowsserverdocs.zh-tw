---
title: 加入網域的裝置公開金鑰驗證
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
author: justinha
ms.author: Justinha
ms.date: 08/18/2017
ms.openlocfilehash: d9603cde90ca4a689d6d5993c8e131ba5a2e7d40
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077595"
---
# <a name="domain-joined-device-public-key-authentication"></a>加入網域的裝置公開金鑰驗證

>適用于： Windows Server 2016、Windows 10

Kerberos 新增了支援加入網域的裝置，以使用從 Windows Server 2012 開始的憑證來登入，並 Windows 8。 這種變更可讓協力廠商廠商建立解決方案，以針對已加入網域的裝置布建和初始化憑證，以用於網域驗證。

## <a name="automatic-public-key-provisioning"></a>自動提供公開金鑰

從 Windows 10 1507 版和 Windows Server 2016 開始，已加入網域的裝置會自動將系結公開金鑰布建到 Windows Server 2016 網域控制站 (DC) 。 布建金鑰之後，Windows 就可以使用網域的公開金鑰驗證。

### <a name="key-generation"></a>金鑰產生
如果裝置正在執行 Credential Guard，則會建立公開/私密金鑰組，並受到 Credential Guard 保護。

如果 Credential Guard 無法使用，且 TPM 為，則會建立公開/私密金鑰組來保護 TPM。

如果兩者皆無法使用，則不會產生金鑰組，且裝置只能使用密碼進行驗證。

### <a name="provisioning-computer-account-public-key"></a>布建電腦帳戶公開金鑰
當 Windows 啟動時，它會檢查是否已針對其電腦帳戶布建公開金鑰。 如果沒有，則會產生系結的公開金鑰，並使用 Windows Server 2016 或更新版本的 DC，在 AD 中為其帳戶設定該公開金鑰。 如果所有 Dc 都在下層，則不會布建任何金鑰。

### <a name="configuring-device-to-only-use-public-key"></a>正在將裝置設定為只使用公開金鑰
如果使用 [憑證] 的 [ **裝置驗證** ] 群組原則設定支援設定為 [ **強制**]，則裝置必須尋找執行 Windows Server 2016 或更新版本的 DC 以進行驗證。 此設定位於系統管理範本 > System > Kerberos。

### <a name="configuring-device-to-only-use-password"></a>正在將裝置設定為只使用密碼
如果已停用 [ **使用憑證的裝置驗證** ] 群組原則設定支援，則一律使用 [密碼]。 此設定位於系統管理範本 > System > Kerberos。

## <a name="domain-joined-device-authentication-using-public-key"></a>使用公開金鑰進行已加入網域的裝置驗證
當 Windows 具有已加入網域之裝置的憑證時，Kerberos 會先使用憑證來進行驗證，並以密碼重試失敗。 這可讓裝置向下層 Dc 進行驗證。

由於自動布建的公開金鑰具有自我簽署憑證，因此在不支援金鑰信任帳戶對應的網域控制站上，憑證驗證會失敗。 根據預設，Windows 會使用裝置的網域密碼重試驗證。


