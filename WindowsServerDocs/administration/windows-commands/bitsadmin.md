---
title: bitsadmin
description: Bitsadmin 命令的參考主題，這是用來建立、下載或上傳作業及監視其進度的命令列工具。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94a829ce21c4571188fb5ffeb9a0a1d991637d07
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710039"
---
# <a name="bitsadmin"></a>bitsadmin

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10

Bitsadmin 是一種命令列工具，可用來建立、下載或上傳作業，以及監視其進度。 Bitsadmin 工具會使用參數來識別要執行的工作。 您可以呼叫`bitsadmin /?`或`bitsadmin /help`來取得參數的清單。

大部分的`<job>`交換器都需要參數，您可以將它設定為作業的顯示名稱或 GUID。 作業的顯示名稱不需要是唯一的。 **/Create**和 **/list**參數會傳回作業的 GUID。

根據預設，您可以存取自己作業的相關資訊。 若要存取其他使用者工作的資訊，您必須具有系統管理員許可權。 如果作業是在提高許可權的狀態下建立，則您必須從提高許可權的視窗執行**bitsadmin** ;否則，您將會擁有該作業的唯讀存取權。

許多參數都會對應至[BITS 介面](https://docs.microsoft.com/windows/win32/bits/bits-interfaces)中的方法。 如需使用參數時可能相關的其他詳細資料，請參閱對應的方法。

使用下列參數來建立作業、設定和取出作業的屬性，以及監視作業的狀態。 如需示範如何使用這些參數來執行工作的範例，請參閱[bitsadmin 範例](bitsadmin-examples.md)。

## <a name="available-switches"></a>可用的交換器

- [bitsadmin/addfile](bitsadmin-addfile.md)
- [bitsadmin /addfileset](bitsadmin-addfileset.md)
- [bitsadmin /addfilewithranges](bitsadmin-addfilewithranges.md)
- [bitsadmin/cache](bitsadmin-cache.md)
- [bitsadmin/cache/delete](bitsadmin-cache-and-delete.md)
- [bitsadmin/cache/deleteurl](bitsadmin-cache-and-deleteurl.md)
- [bitsadmin/cache/getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
- [bitsadmin/cache/getlimit](bitsadmin-cache-and-getlimit.md)
- [bitsadmin/cache/help](bitsadmin-cache-and-help.md)
- [bitsadmin/cache/info](bitsadmin-cache-and-info.md)
- [bitsadmin/cache/list](bitsadmin-cache-and-list.md)
- [bitsadmin/cache/setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
- [bitsadmin/cache/setlimit](bitsadmin-cache-and-setlimit.md)
- [bitsadmin/cache/clear](bitsadmin-cache-clear.md)
- [bitsadmin /cancel](bitsadmin-cancel.md)
- [bitsadmin /complete](bitsadmin-complete.md)
- [bitsadmin/create](bitsadmin-create.md)
- [bitsadmin /examples](bitsadmin-examples.md)
- [bitsadmin /getaclflags](bitsadmin-getaclflags.md)
- [bitsadmin /getbytestotal](bitsadmin-getbytestotal.md)
- [bitsadmin /getbytestransferred](bitsadmin-getbytestransferred.md)
- [bitsadmin /getclientcertificate](bitsadmin-getclientcertificate.md)
- [bitsadmin /getcompletiontime](bitsadmin-getcompletiontime.md)
- [bitsadmin /getcreationtime](bitsadmin-getcreationtime.md)
- [bitsadmin /getcustomheaders](bitsadmin-getcustomheaders.md)
- [bitsadmin /getdescription](bitsadmin-getdescription.md)
- [bitsadmin /getdisplayname](bitsadmin-getdisplayname.md)
- [bitsadmin /geterror](bitsadmin-geterror.md)
- [bitsadmin /geterrorcount](bitsadmin-geterrorcount.md)
- [bitsadmin /getfilestotal](bitsadmin-getfilestotal.md)
- [bitsadmin /getfilestransferred](bitsadmin-getfilestransferred.md)
- [bitsadmin /gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
- [bitsadmin /gethelpertokensid](bitsadmin-gethelpertokensid.md)
- [bitsadmin /getHTTPmethod](bitsadmin-gethttpmethod.md)
- [bitsadmin /getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
- [bitsadmin /getminretrydelay](bitsadmin-getminretrydelay.md)
- [bitsadmin /getmodificationtime](bitsadmin-getmodificationtime.md)
- [bitsadmin /getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
- [bitsadmin /getnotifycmdline](bitsadmin-getnotifycmdline.md)
- [bitsadmin /getnotifyflags](bitsadmin-getnotifyflags.md)
- [bitsadmin /getnotifyinterface](bitsadmin-getnotifyinterface.md)
- [bitsadmin /getowner](bitsadmin-getowner.md)
- [bitsadmin /getpeercachingflags](bitsadmin-getpeercachingflags.md)
- [bitsadmin /getpriority](bitsadmin-getpriority.md)
- [bitsadmin /getproxybypasslist](bitsadmin-getproxybypasslist.md)
- [bitsadmin /getproxylist](bitsadmin-getproxylist.md)
- [bitsadmin /getproxyusage](bitsadmin-getproxyusage.md)
- [bitsadmin /getreplydata](bitsadmin-getreplydata.md)
- [bitsadmin /getreplyfilename](bitsadmin-getreplyfilename.md)
- [bitsadmin /getreplyprogress](bitsadmin-getreplyprogress.md)
- [bitsadmin /getsecurityflags](bitsadmin-getsecurityflags.md)
- [bitsadmin /getstate](bitsadmin-getstate.md)
- [bitsadmin /gettemporaryname](bitsadmin-gettemporaryname.md)
- [bitsadmin /gettype](bitsadmin-gettype.md)
- [bitsadmin /getvalidationstate](bitsadmin-getvalidationstate.md)
- [bitsadmin/help](bitsadmin-help.md)
- [bitsadmin/info](bitsadmin-info.md)
- [bitsadmin/list](bitsadmin-list.md)
- [bitsadmin /listfiles](bitsadmin-listfiles.md)
- [bitsadmin /makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
- [bitsadmin/監視](bitsadmin-monitor.md)
- [bitsadmin /nowrap](bitsadmin-nowrap.md)
- [bitsadmin /peercaching](bitsadmin-peercaching.md)
- [bitsadmin /peercaching /getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
- [bitsadmin/peercaching/help](bitsadmin-peercaching-and-help.md)
- [bitsadmin /peercaching /setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
- [bitsadmin /peers](bitsadmin-peers.md)
- [bitsadmin/peers/clear](bitsadmin-peers-and-clear.md)
- [bitsadmin /peers /discover](bitsadmin-peers-and-discover.md)
- [bitsadmin/peers/help](bitsadmin-peers-and-help.md)
- [bitsadmin/peers/list](bitsadmin-peers-and-list.md)
- [bitsadmin /rawreturn](bitsadmin-rawreturn.md)
- [bitsadmin /removeclientcertificate](bitsadmin-removeclientcertificate.md)
- [bitsadmin /removecredentials](bitsadmin-removecredentials.md)
- [bitsadmin /replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
- [bitsadmin/reset](bitsadmin-reset.md)
- [bitsadmin/resume](bitsadmin-resume.md)
- [bitsadmin /setaclflag](bitsadmin-setaclflag.md)
- [bitsadmin /setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
- [bitsadmin /setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
- [bitsadmin /setcredentials](bitsadmin-setcredentials.md)
- [bitsadmin /setcustomheaders](bitsadmin-setcustomheaders.md)
- [bitsadmin /setdescription](bitsadmin-setdescription.md)
- [bitsadmin /setdisplayname](bitsadmin-setdisplayname.md)
- [bitsadmin /sethelpertoken](bitsadmin-sethelpertoken.md)
- [bitsadmin /sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
- [bitsadmin /setHTTPmethod](bitsadmin-sethttpmethod.md)
- [bitsadmin /setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
- [bitsadmin /setminretrydelay](bitsadmin-setminretrydelay.md)
- [bitsadmin /setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
- [bitsadmin /setnotifycmdline](bitsadmin-setnotifycmdline.md)
- [bitsadmin /setnotifyflags](bitsadmin-setnotifyflags.md)
- [bitsadmin /setpeercachingflags](bitsadmin-setpeercachingflags.md)
- [bitsadmin /setpriority](bitsadmin-setpriority.md)
- [bitsadmin /setproxysettings](bitsadmin-setproxysettings.md)
- [bitsadmin /setreplyfilename](bitsadmin-setreplyfilename.md)
- [bitsadmin /setsecurityflags](bitsadmin-setsecurityflags.md)
- [bitsadmin /setvalidationstate](bitsadmin-setvalidationstate.md)
- [bitsadmin /suspend](bitsadmin-suspend.md)
- [bitsadmin /takeownership](bitsadmin-takeownership.md)
- [bitsadmin /transfer](bitsadmin-transfer.md)
- [bitsadmin /util](bitsadmin-util.md)
- [bitsadmin /util /enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
- [bitsadmin /util /getieproxy](bitsadmin-util-and-getieproxy.md)
- [bitsadmin/util/help](bitsadmin-util-and-help.md)
- [bitsadmin /util /repairservice](bitsadmin-util-and-repairservice.md)
- [bitsadmin /util /setieproxy](bitsadmin-util-and-setieproxy.md)
- [bitsadmin/util/version](bitsadmin-util-and-version.md)
- [bitsadmin /wrap](bitsadmin-wrap.md)
