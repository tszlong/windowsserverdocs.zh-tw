---
title: "加入網域的裝置公開金鑰驗證"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 8/18/2017
ms.openlocfilehash: 65f39f4900fcd99ef90b160d3db7099edb73bc1c
ms.sourcegitcommit: e94838df702ff0135ebe7c179b228aa67b772d35
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="domain-joined-device-public-key-authentication"></a>加入網域的裝置公開金鑰驗證

>適用於︰ Windows Server 2016 年，在 Windows 10

Kerberos 加入網域的裝置登入憑證開始使用 Windows Server 2012 和 Windows 8 的支援。 這項變更可讓您建立來提供及初始化加入網域的裝置使用網域驗證憑證的方案 3 廠商。 

## <a name="automatic-public-key-provisioning"></a>自動公用金鑰提供

開始使用 Windows 10 1507 版和 Windows Server 2016、加入網域的裝置會自動提供 Windows Server 2016 網域控制站 DC 繫結公用鍵。 一旦按鍵時，Windows 可以網域使用公開金鑰驗證。

### <a name="public-key-generation"></a>公開的主要代
如果您在裝置執行的 Credential Guard，公用按鍵會建立受 Credential Guard。 

如果不是可用 Credential Guard，TPM 是公用按鍵會建立 tpm 受保護。 

如果兩者都可使用，也不按鍵，然後裝置僅驗證使用密碼。

### <a name="provisioning-computer-account-public-key"></a>提供電腦 account 公用鍵
Windows 時，它會檢查是否公用按鍵提供的。 如果無法使用，然後產生結合公開金鑰，並將它設定中使用 Windows Server 2016 或更高版本俠 AD 其 account。 如果所有網域控制站向下層級，會不提供任何按鍵。

### <a name="configuring-device-to-only-use-public-key"></a>設定裝置只能使用公用鍵
如果群組原則設定**裝置使用進行驗證憑證的支援**設為**推動**，然後使用，裝置需要驗證尋找執行 Windows Server 2016 DC 或更新版本。 設定可在 [系統管理範本] > 系統 > Kerberos。

### <a name="configuring-device-to-only-use-password"></a>設定裝置只能使用密碼
如果群組原則設定**裝置使用進行驗證憑證的支援**已停用，然後永遠使用密碼。 設定可在 [系統管理範本] > 系統 > Kerberos。

## <a name="domain-joined-device-authentication-using-public-key"></a>使用公用鍵加入網域的裝置驗證
當 Windows 已經加入網域的裝置的憑證時，Kerberos 先行驗證使用的憑證和失敗重試次數密碼。 這可以讓裝置舊版 Dc 驗證。

由於自動提供公用按鍵自動簽署的憑證，憑證驗證失敗網域控制站不支援鍵信任 account 對應。 根據預設，Windows 會重試驗證使用裝置的網域密碼。


