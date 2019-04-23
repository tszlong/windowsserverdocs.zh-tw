---
title: finger
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42622fdf19cdd50b76d32989769874cbd05e9f4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826939"
---
# <a name="finger"></a>finger

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

指定遠端電腦上 （通常是執行 UNIX 的電腦） 執行的指服務或精靈中顯示一或多位使用者的相關資訊。 在遠端電腦指定的格式和輸出使用者資訊顯示。 不含參數，**手指**顯示說明。 
## <a name="syntax"></a>語法
```
finger [-l] [<User>] [@<Host>] [...]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|-l|長的清單格式顯示使用者資訊。|
|<User>|指定您想要了解哪種資訊的使用者。 如果您省略*使用者*參數**手指**顯示指定電腦上的所有使用者的相關資訊。|
|@<Host>|指定遠端電腦執行的手指服務，您要尋找的使用者資訊。 您可以指定電腦名稱或 IP 位址。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
多個User@Host可以指定參數。
您必須在前面**手指**連字號 （-），而不是斜線 （/） 的參數。
此命令才會提供網際網路通訊協定 (TCP/IP) 通訊協定安裝為在 網路連線的網路介面卡的內容中的元件。
Windows Server 2003 不提供手指服務。
## <a name="BKMK_Examples"></a>範例
若要在電腦 users.microsoft.com 顯示 user1 的資訊，請輸入：
```
finger user1@users.microsoft.com
```
若要在電腦 users.microsoft.com 上顯示的所有使用者的資訊，請輸入：
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
