---
title: gpresult
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0acd4563225bc1c413b2096387ad8c14f6009a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835969"
---
# <a name="gpresult"></a>gpresult

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示遠端使用者和電腦的原則結果組 (RSoP) 資訊。
若要使用 RSoP 報告針對遠端目標電腦通過防火牆，您必須啟用輸入的網路流量的連接埠上的防火牆規則。
## <a name="syntax"></a>語法
```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```
## <a name="parameters"></a>參數
> [!NOTE]
> 但當您使用 **/？**，您必須包含輸出選項，或是 **/r**， **/v**， **/z**， **/x**，或 **/h**。

|參數|描述|
|-------|--------|
|/s \<system\>|指定的名稱或遠端電腦的 IP 位址。 請勿使用反斜線。 預設是本機電腦。|
|/u \<USERNAME\>|使用指定的使用者認證來執行命令。 預設的使用者是發出命令的電腦登入的使用者。|
|/p [\<PASSWOrd\>]|指定使用者帳戶中所提供的密碼 **/u**參數。 如果 **/p**省略，則**gpresult**會提示輸入密碼。 **/p**不能搭配 **/x**或是 **/h**。|
|/user [\<TARGETDOMAIN\>\\]\<TARGETUSER\>|指定的 RSoP 資料都要顯示的遠端使用者。|
|/ 範圍 {使用者&#124;電腦}|顯示使用者或電腦的 RSoP 資料。 如果 **/範圍**省略，則**gpresult**顯示 使用者和電腦的 RSoP 資料。|
|[/x &#124; /h] <FILENAME>|將報表儲存在 XML 中 (**/x**) 或 HTML (**/h**) 的位置和檔案名稱所指定的格式*FILENAME*參數。 不能搭配 **/u**， **/p**， **/r**， **/v**，或 **/z**。|
|/f|會強制**gpresult**覆寫中指定的檔案名稱 **/x**或是 **/h**選項。|
|/r|顯示 RSoP 摘要資料。|
|/v|顯示詳細資訊的原則資訊。 這包括已套用優先順序為 1 的詳細的設定。|
|/z|顯示所有可用群組原則的相關資訊。 這包括已套用的 1 到較高優先順序的詳細的設定。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   群組原則是定義和控制的程式、 網路資源，以及作業系統的運作方式的組織中使用者和電腦的主要系統管理工具。 在 active directory 環境，群組原則會套用到使用者或站台、 網域或組織單位中的成員資格為基礎的電腦。
-   因為您可以將重疊的原則設定套用到任何電腦或使用者，群組原則功能的使用者登入時，就會產生原則設定的結果集。 **gpresult**顯示產生的原則設定之前強制執行一組指定的使用者電腦上，當使用者登入。
-   因為 **/v**並 **/z**產生許多的詳細資訊，最好將輸出重新導向至文字檔案 (例如**gpresult/z > policy.txt**)。
-   **Gpresult**命令適用於 Windows Server 2012、 Windows Server 2008 R2、 Windows server 2008、 Windows 8、 Windows 7 和 Windows Vista。
## <a name="BKMK_Examples"></a>範例
下列範例會擷取為遠端使用者的 RSoP 資料**targetusername**的電腦**srvmain**，並顯示有關僅限使用者的 RSoP 資料。 使用者的認證執行命令**maindom\hiropln**，並**p@ssW23**輸入該使用者的密碼。
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```
下列範例會儲存所有可用資訊的相關群組原則遠端使用者**targetusername**的電腦**srvmain**檔案，稱為**policy.txt**. 包含電腦的相關資料。 使用者的認證執行命令**maindom\hiropln**，並**p@ssW23**輸入該使用者的密碼。
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```
下列範例會顯示電腦的 RSoP 資料**srvmain**和登入的使用者。 包含使用者和電腦的相關資料。 使用者的認證執行命令**maindom\hiropln**，並**p@ssW23**輸入該使用者的密碼。
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```
## <a name="additional-references"></a>其他參考資料
-   [群組原則 TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)

-   [命令列語法關鍵](command-line-syntax-key.md)
