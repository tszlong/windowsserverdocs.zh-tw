---
title: cmstp
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ee5c5189b4eab21994def221dd757b0061ef22d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836379"
---
# <a name="cmstp"></a>cmstp

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

安裝或移除連接管理員的服務設定檔。 不含選擇性參數， **cmstp**使用適當的作業系統以及使用者的權限的預設設定安裝的服務設定檔。 
## <a name="syntax"></a>語法
1 的語法：
```
ServiceProfileFileName .exe /q:a /c:"cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]"
```
語法 2:
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf"
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|< ServiceProfileFileName >.exe|依名稱，指定包含您想要安裝的設定檔的安裝套件。<br /><br />必要項目語法 1，但不是有效的語法 2。|
|/q:a|指定設定檔應該不會提示使用者安裝。 仍然會出現安裝成功的驗證訊息。<br /><br />必要項目語法 1，但不是有效的語法 2。|
|[磁碟機:][路徑]<ServiceProfileFileName>.inf|必要。 依名稱，指定組態檔，判斷安裝的設定檔的方式。<br /><br />[磁碟機:] 語法 1 中的 [路徑] 參數無效。|
|/nf|指定未安裝支援檔案。|
|/ni|指定不應建立桌面圖示。 這個參數只適用於執行 Windows 95、 Windows 98、 Windows NT 4.0 或 Windows Millennium edition 的電腦。|
|/ns|指定不應建立桌面捷徑。 這個參數只適用於執行之成員的 Windows Server 2003 系列、 Windows 2000 或 Windows XP 的電腦。|
|/s|指定服務設定檔應該要安裝或解除安裝以無訊息模式 （而不提示使用者回應或顯示驗證訊息）。|
|/su|指定服務設定檔應該在單一使用者，而不是針對所有使用者安裝。 這個參數只適用於執行 Windows Server 2003、 Windows 2000 或 Windows XP 的電腦。|
|/au|指定服務設定檔應該會針對所有使用者安裝。 這個參數只適用於執行 Windows Server 2003、 Windows 2000 或 Windows XP 的電腦。|
|/u|指定應解除安裝服務設定檔。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
**/s**是您可以搭配使用的唯一參數 **/u**。
語法 1 是用在自訂安裝的應用程式的一般語法。 若要使用此語法，您必須執行**cmstp**目錄，其中包含從<ServiceProfileFileName>.exe 檔案。
## <a name="BKMK_Examples"></a>範例
若要安裝小說服務設定檔，而不需要任何支援檔案，請輸入：
```
fiction.exe /c:"cmstp.exe fiction.inf /nf"
```
若要以無訊息模式安裝為單一使用者故事服務設定檔，請輸入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /su"
```
若要以無訊息方式解除安裝小說服務設定檔，請輸入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /u"
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
