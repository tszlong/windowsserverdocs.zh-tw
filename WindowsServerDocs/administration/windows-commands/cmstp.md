---
title: cmstp
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fd2e8dbb08b41875335b35dd802007a0bd1fbd41
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379275"
---
# <a name="cmstp"></a>cmstp

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安裝或移除連線管理員服務設定檔。 使用時不含選擇性參數， **cmstp**會使用適用于作業系統的預設設定和使用者的許可權來安裝服務設定檔。 
## <a name="syntax"></a>語法
語法1：
```
ServiceProfileFileName .exe /q:a /c:"cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]"
```
語法2：
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf"
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|< ServiceProfileFileName > .exe|依名稱指定包含您要安裝之設定檔的安裝套件。<br /><br />語法為1的必要參數，但對語法2而言無效。|
|/q： a|指定應該在不提示使用者的情況下安裝設定檔。 安裝已成功的驗證訊息仍然會出現。<br /><br />語法為1的必要參數，但對語法2而言無效。|
|[磁片磁碟機：][path] @no__t 4.9.0-.inf|必要。 依名稱指定設定檔案，以決定要如何安裝設定檔。<br /><br />[Drive：] [path] 參數對語法1無效。|
|/nf|指定不應該安裝支援檔案。|
|/ni|指定不應建立桌面圖示。 此參數只對執行 Windows 95、Windows 98、Windows NT 4.0 或 Windows Millennium edition 的電腦有效。|
|/ns|指定不應建立桌面快捷方式。 此參數只對執行 Windows Server 2003 系列、Windows 2000 或 Windows XP 成員的電腦有效。|
|/s|指定應該以無訊息方式安裝或卸載服務設定檔（不提示使用者回應或顯示驗證訊息）。|
|/su|指定應為單一使用者安裝服務設定檔，而不是針對所有使用者。 此參數只對執行 Windows Server 2003、Windows 2000 或 Windows XP 的電腦有效。|
|/au|指定應為所有使用者安裝服務設定檔。 此參數只對執行 Windows Server 2003、Windows 2000 或 Windows XP 的電腦有效。|
|u|指定應卸載服務設定檔。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
**/s**是您可以搭配 **/u**使用的唯一參數。
語法1是自訂安裝應用程式中所使用的一般語法。 若要使用此語法，您必須從包含 <ServiceProfileFileName> .exe 檔案的目錄執行**cmstp** 。
## <a name="BKMK_Examples"></a>典型
若要安裝不含任何支援檔案的小說服務設定檔，請輸入：
```
fiction.exe /c:"cmstp.exe fiction.inf /nf"
```
若要以無訊息方式安裝單一使用者的小說服務設定檔，請輸入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /su"
```
若要以無訊息方式卸載小說服務設定檔，請輸入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /u"
```
## <a name="additional-references"></a>其他參考
-   [命令列語法關鍵](command-line-syntax-key.md)
