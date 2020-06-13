---
title: Windows 命令
description: 參考
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/09/2020
ms.prod: windows-server
ms.openlocfilehash: 66de80652f764840af70e88dfd39df2398ae495c
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721112"
---
# <a name="windows-commands"></a>Windows 命令

所有支援的 Windows 版本（伺服器和用戶端）都有一組內建的 Win32 主控台命令。

這一組檔描述可讓您使用腳本或腳本工具來自動化工作的 Windows 命令。

若要尋找特定命令的相關資訊，請在下列 a-z 功能表中，按一下命令開頭的字母，然後按一下命令名稱。

[A](#a)  |
[B](#b)  |
[C](#c)  |
[D](#d)  |
[E](#e)  |
[F](#f)  |
[G](#g)  |
[H](#h)  |
[I](#i)  |
[J](#j)  |
[K](#k)  |
[L](#l)  |
[M](#m)  |
[N](#n)  |
[O](#o)  |
[P](#p)  |
[問](#q)  |
[R](#r)  |
[S](#s)  |
[T](#t)  |
[U](#u)  |
[V](#v)  |
[W](#w)  |
[X](#x) |Y |Z

## <a name="prerequisites"></a>Prerequisites

本主題所包含的資訊適用于：

- Windows Server 2019
- Windows Server (半年通道)
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows 10
- Windows 8.1

### <a name="command-shell-overview"></a>命令 shell 總覽

命令 shell 是 Windows 內建的第一個 shell，可使用 batch （.bat）檔案，將例行工作（例如使用者帳戶管理或夜間備份）自動化。 使用 Windows Script Host，您可以在命令 shell 中執行更複雜的腳本。 如需詳細資訊，請參閱[cscript](cscript.md)或[wscript.echo](wscript.md)。 您可以使用腳本，而不是使用使用者介面來更有效率地執行作業。 腳本會接受命令列上可用的所有命令。

Windows 有兩個命令執行介面：命令 shell 和[PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6)。 每個 shell 都是一種軟體程式，可提供您與作業系統或應用程式之間的直接通訊，並提供環境來自動化 IT 作業。

PowerShell 的設計目的是要擴充命令 shell 的功能，以執行稱為 Cmdlet 的 PowerShell 命令。 Cmdlet 類似于 Windows 命令，但提供更可擴充的指令碼語言。 您可以在 Powershell 中執行 Windows 命令和 PowerShell Cmdlet，但命令 shell 只能執行 Windows 命令，而不是 PowerShell Cmdlet。

為了充分發揮最新的 Windows 自動化功能，我們建議您針對 Windows 自動化使用 PowerShell，而不是 Windows 命令或 Windows Script Host。

> [!NOTE]
>您也可以下載並安裝 powershell [Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6)，也就是 powershell 的開放原始碼版本。

> [!CAUTION]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 在對登錄進行下列變更之前，您應該先備份電腦上任何重要的資料。

> [!NOTE]
> 若要在電腦或使用者登入會話上啟用或停用命令 shell 中的檔案和目錄名稱自動完成，請執行**regedit.exe**並設定下列**reg_DWOrd 值**：
>
> HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\completionChar\ reg_DWOrd
>
> 若要設定**reg_DWOrd**值，請針對特定函式使用控制字元的十六進位值（例如， **0 9**為 Tab， **0 08**為倒退鍵）。 使用者指定的設定會優先于電腦設定，而命令列選項的優先順序高於登錄設定。

## <a name="command-line-reference-a-z"></a>命令列參考 A-Z

若要尋找特定 Windows 命令的相關資訊，請在下列 a-z 功能表中，按一下命令開頭的字母，然後按一下命令名稱。

[A](#a)  |
[B](#b)  |
[C](#c)  |
[D](#d)  |
[E](#e)  |
[F](#f)  |
[G](#g)  |
[H](#h)  |
[I](#i)  |
[J](#j)  |
[K](#k)  |
[L](#l)  |
[M](#m)  |
[N](#n)  |
[O](#o)  |
[P](#p)  |
[問](#q)  |
[R](#r)  |
[S](#s)  |
[T](#t)  |
[U](#u)  |
[V](#v)  |
[W](#w)  |
[X](#x) |Y |Z

### <a name="a"></a>A

- [active](active.md)
- [add](add.md)
- [add alias](add-alias.md)
- [add volume](add-volume.md)
- [append](append.md)
- [arp](arp.md)
- [assign](assign.md)
- [assoc](assoc.md)
- [at](at.md)
- [atmadm](atmadm.md)
- [attach-vdisk](attach-vdisk.md)
- [attrib](attrib.md)
- [attributes](attributes.md)
  - [attributes disk](attributes-disk.md)
  - [attributes volume](attributes-volume.md)
- [auditpol](auditpol.md)
  - [auditpol backup](auditpol-backup.md)
  - [auditpol clear](auditpol-clear.md)
  - [auditpol get](auditpol-get.md)
  - [auditpol list](auditpol-list.md)
  - [auditpol remove](auditpol-remove.md)
  - [auditpol resourcesacl](auditpol-resourcesacl.md)
  - [auditpol restore](auditpol-restore.md)
  - [auditpol set](auditpol-set.md)
- [autochk](autochk.md)
- [autoconv](autoconv.md)
- [autofmt](autofmt.md)
- [automount](automount.md)

### <a name="b"></a>B

- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
  - [bdehdcfg driveinfo](bdehdcfg-driveinfo.md)
  - [bdehdcfg newdriveletter](bdehdcfg-newdriveletter.md)
  - [bdehdcfg quiet](bdehdcfg-quiet.md)
  - [bdehdcfg restart](bdehdcfg-restart.md)
  - [bdehdcfg size](bdehdcfg-size.md)
  - [bdehdcfg target](bdehdcfg-target.md)
- [begin backup](begin-backup.md)
- [begin restore](begin-restore.md)
- [bitsadmin](bitsadmin.md)
  - [bitsadmin addfile](bitsadmin-addfile.md)
  - [bitsadmin addfileset](bitsadmin-addfileset.md)
  - [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  - [bitsadmin cache](bitsadmin-cache.md)
    - [bitsadmin cache and delete](bitsadmin-cache-and-delete.md)
    - [bitsadmin cache and deleteurl](bitsadmin-cache-and-deleteurl.md)
    - [bitsadmin cache and getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
    - [bitsadmin cache and getlimit](bitsadmin-cache-and-getlimit.md)
    - [bitsadmin cache and help](bitsadmin-cache-and-help.md)
    - [bitsadmin cache and info](bitsadmin-cache-and-info.md)
    - [bitsadmin cache and list](bitsadmin-cache-and-list.md)
    - [bitsadmin cache and setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
    - [bitsadmin cache and setlimit](bitsadmin-cache-and-setlimit.md)
    - [bitsadmin cache and clear](bitsadmin-cache-clear.md)
  - [bitsadmin cancel](bitsadmin-cancel.md)
  - [bitsadmin complete](bitsadmin-complete.md)
  - [bitsadmin create](bitsadmin-create.md)
  - [bitsadmin examples](bitsadmin-examples.md)
  - [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  - [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  - [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  - [bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)
  - [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  - [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  - [bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)
  - [bitsadmin getdescription](bitsadmin-getdescription.md)
  - [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  - [bitsadmin geterror](bitsadmin-geterror.md)
  - [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  - [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  - [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  - [bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
  - [bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)
  - [bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
  - [bitsadmin getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
  - [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  - [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  - [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  - [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  - [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  - [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  - [bitsadmin getowner](bitsadmin-getowner.md)
  - [bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)
  - [bitsadmin getpriority](bitsadmin-getpriority.md)
  - [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  - [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  - [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  - [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  - [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  - [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  - [bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)
  - [bitsadmin getstate](bitsadmin-getstate.md)
  - [bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)
  - [bitsadmin gettype](bitsadmin-gettype.md)
  - [bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)
  - [bitsadmin help](bitsadmin-help.md)
  - [bitsadmin info](bitsadmin-info.md)
  - [bitsadmin list](bitsadmin-list.md)
  - [bitsadmin listfiles](bitsadmin-listfiles.md)
  - [bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
  - [bitsadmin monitor](bitsadmin-monitor.md)
  - [bitsadmin nowrap](bitsadmin-nowrap.md)
  - [bitsadmin peercaching](bitsadmin-peercaching.md)
    - [bitsadmin peercaching and getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
    - [bitsadmin peercaching and help](bitsadmin-peercaching-and-help.md)
    - [bitsadmin peercaching and setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
  - [bitsadmin peers](bitsadmin-peers.md)
    - [bitsadmin peers and clear](bitsadmin-peers-and-clear.md)
    - [bitsadmin peers and discover](bitsadmin-peers-and-discover.md)
    - [bitsadmin peers and help](bitsadmin-peers-and-help.md)
    - [bitsadmin peers and list](bitsadmin-peers-and-list.md)
  - [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  - [bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)
  - [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  - [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  - [bitsadmin reset](bitsadmin-reset.md)
  - [bitsadmin resume](bitsadmin-resume.md)
  - [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  - [bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
  - [bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
  - [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  - [bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)
  - [bitsadmin setdescription](bitsadmin-setdescription.md)
  - [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  - [bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)
  - [bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
  - [bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
  - [bitsadmin setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
  - [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  - [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  - [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  - [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  - [bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)
  - [bitsadmin setpriority](bitsadmin-setpriority.md)
  - [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  - [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  - [bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)
  - [bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)
  - [bitsadmin suspend](bitsadmin-suspend.md)
  - [bitsadmin takeownership](bitsadmin-takeownership.md)
  - [bitsadmin transfer](bitsadmin-transfer.md)
  - [bitsadmin util](bitsadmin-util.md)
    - [bitsadmin util and enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
    - [bitsadmin util and getieproxy](bitsadmin-util-and-getieproxy.md)
    - [bitsadmin util and help](bitsadmin-util-and-help.md)
    - [bitsadmin util and repairservice](bitsadmin-util-and-repairservice.md)
    - [bitsadmin util and setieproxy](bitsadmin-util-and-setieproxy.md)
    - [bitsadmin util and version](bitsadmin-util-and-version.md)
  - [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  - [bootcfg addsw](bootcfg-addsw.md)
  - [bootcfg copy](bootcfg-copy.md)
  - [bootcfg dbg1394](bootcfg-dbg1394.md)
  - [bootcfg debug](bootcfg-debug.md)
  - [bootcfg default](bootcfg-default.md)
  - [bootcfg delete](bootcfg-delete.md)
  - [bootcfg ems](bootcfg-ems.md)
  - [bootcfg query](bootcfg-query.md)
  - [bootcfg raw](bootcfg-raw.md)
  - [bootcfg rmsw](bootcfg-rmsw.md)
  - [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C

- [cacls](cacls.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  - [change logon](change-logon.md)
  - [change port](change-port.md)
  - [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [clean](clean.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [compact vdisk](compact-vdisk.md)
- [convert](convert.md)
  - [convert basic](convert-basic.md)
  - [convert dynamic](convert-dynamic.md)
  - [convert gpt](convert-gpt.md)
  - [convert mbr](convert-mbr.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [create](create.md)
  - [create partition efi](create-partition-efi.md)
  - [建立[分割區擴充](create-partition-extended.md)
  - [create partition logical](create-partition-logical.md)
  - [create partition msr](create-partition-msr.md)
  - [建立磁碟分割主要](create-partition-primary.md)
  - [建立磁片區鏡像](create-volume-mirror.md)
  - [create volume raid](create-volume-raid.md)
  - [create volume simple](create-volume-simple.md)
  - [create volume stripe](create-volume-stripe.md)
- [cscript](cscript.md)

### <a name="d"></a>D

- [date](date.md)
- [dcgpofix](dcgpofix.md)
- [defrag](defrag.md)
- [del](del.md)
- [delete](delete.md)
  - [刪除磁片](delete-disk.md)
  - [delete partition](delete-partition.md)
  - [刪除陰影](delete-shadows.md)
  - [delete volume](delete-volume.md)
- [卸離 vdisk](detach-vdisk.md)
- [資訊](detail.md)
  - [detail disk](detail-disk.md)
  - [detail partition](detail-partition.md)
  - [詳細資料 vdisk](detail-vdisk.md)
  - [detail volume](detail-volume.md)
- [dfsdiag](dfsdiag.md)
  - [dfsdiag testdcs](dfsdiag-testdcs.md)
  - [dfsdiag testdfsconfig](dfsdiag-testdfsconfig.md)
  - [dfsdiag testdfsintegrity](dfsdiag-testdfsintegrity.md)
  - [dfsdiag testreferral](dfsdiag-testreferral.md)
  - [dfsdiag testsites](dfsdiag-testsites.md)
- [dfsrmig](dfsrmig.md)
- [diantz](diantz.md)
- [dir](dir.md)
- [diskcomp](diskcomp.md)
- [diskcopy](diskcopy.md)
- [diskpart](diskpart.md)
- [diskperf](diskperf.md)
- [diskraid](diskraid.md)
- [diskshadow](diskshadow.md)
- [dispdiag](dispdiag.md)
- [dnscmd](dnscmd.md)
- [doskey](doskey.md)
- [driverquery](driverquery.md)

### <a name="e"></a>E

- [echo](echo.md)
- [edit](edit.md)
- [endlocal](endlocal.md)
- [結束還原](end-restore.md)
- [erase](erase.md)
- [eventcreate](eventcreate.md)
- [eventquery](eventquery.md)
- [eventtriggers](eventtriggers.md)
- [Evntcmd](evntcmd.md)
- [exec](exec.md)
- [exit](exit_2.md)
- [expand](expand.md)
- [展開 vdisk](expand-vdisk.md)
- [向](expose.md)
- [延遲](extend.md)
- [extract](extract.md)

### <a name="f"></a>F

- [fc](fc.md)
- [filesystems](filesystems.md)
- [find](find.md)
- [findstr](findstr.md)
- [finger](finger.md)
- [flattemp](flattemp.md)
- [fondue](fondue.md)
- [for](for.md)
- [forfiles](forfiles.md)
- [format](format.md)
- [freedisk](freedisk.md)
- [fsutil](fsutil.md)
  - [fsutil 8dot3name](fsutil-8dot3name.md)
  - [fsutil behavior](fsutil-behavior.md)
  - [fsutil dirty](fsutil-dirty.md)
  - [fsutil file](fsutil-file.md)
  - [fsutil fsinfo](fsutil-fsinfo.md)
  - [fsutil hardlink](fsutil-hardlink.md)
  - [fsutil objectid](fsutil-objectid.md)
  - [fsutil quota](fsutil-quota.md)
  - [fsutil repair](fsutil-repair.md)
  - [fsutil reparsepoint](fsutil-reparsepoint.md)
  - [fsutil resource](fsutil-resource.md)
  - [fsutil sparse](fsutil-sparse.md)
  - [fsutil tiering](fsutil-tiering.md)
  - [fsutil transaction](fsutil-transaction.md)
  - [fsutil usn](fsutil-usn.md)
  - [fsutil volume](fsutil-volume.md)
  - [fsutil wim](fsutil-wim.md)
- [ftp](ftp.md)
  - [ftp append](ftp-append.md)
  - [ftp ascii](ftp-ascii.md)
  - [ftp bell](ftp-bell_1.md)
  - [ftp binary](ftp-binary.md)
  - [ftp bye](ftp-bye.md)
  - [ftp cd](ftp-cd.md)
  - [ftp close](ftp-close_1.md)
  - [ftp debug](ftp-debug.md)
  - [ftp delete](ftp-delete.md)
  - [ftp dir](ftp-dir.md)
  - [ftp disconnect](ftp-disconnect_1.md)
  - [ftp get](ftp-get.md)
  - [ftp glob](ftp-glob_1.md)
  - [ftp hash](ftp-hash_1.md)
  - [ftp lcd](ftp-lcd.md)
  - [ftp literal](ftp-literal_1.md)
  - [ftp ls](ftp-ls_1.md)
  - [ftp mget](ftp-mget.md)
  - [ftp mkdir](ftp-mkdir.md)
  - [ftp mls](ftp-mls_1.md)
  - [ftp mput](ftp-mput_1.md)
  - [ftp open](ftp-open_1.md)
  - [ftp prompt](ftp-prompt_1.md)
  - [ftp put](ftp-put.md)
  - [ftp pwd](ftp-pwd_1.md)
  - [ftp quit](ftp-quit.md)
  - [ftp quote](ftp-quote.md)
  - [ftp recv](ftp-recv.md)
  - [ftp remotehelp](ftp-remotehelp_1.md)
  - [ftp rename](ftp-rename.md)
  - [ftp rmdir](ftp-rmdir.md)
  - [ftp send](ftp-send_1.md)
  - [ftp status](ftp-status.md)
  - [ftp trace](ftp-trace_1.md)
  - [ftp type](ftp-type.md)
  - [ftp user](ftp-user.md)
  - [ftp verbose](ftp-verbose_1.md)
  - [ftp mdelete](ftp-.mdelete_1.md)
  - [ftp mdir](ftp.mdir.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G

- [getmac](getmac.md)
- [gettype](gettype.md)
- [goto](goto.md)
- [gpfixup](gpfixup.md)
- [gpresult](gpresult.md)
- [gpt](gpt.md)
- [gpupdate](gpupdate.md)
- [graftabl](graftabl.md)

### <a name="h"></a>H

- [說明](help.md)
- [helpctr](helpctr.md)
- [hostname](hostname.md)

### <a name="i"></a>I

- [icacls](icacls.md)
- [if](if.md)
- [匯入（shadowdisk）](import.md)
- [匯入（diskpart）](import_1.md)
- [inactive](inactive.md)
- [inuse](inuse.md)
- [ipconfig](ipconfig.md)
- [ipxroute](ipxroute.md)
- [irftp](irftp.md)

### <a name="j"></a>J

- [jetpack](jetpack.md)

### <a name="k"></a>K

- [klist](klist.md)
- [ksetup](ksetup.md)
  - [ksetup addenctypeattr](ksetup-addenctypeattr.md)
  - [ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md)
  - [ksetup addkdc](ksetup-addkdc.md)
  - [ksetup addkpasswd](ksetup-addkpasswd.md)
  - [ksetup addrealmflags](ksetup-addrealmflags.md)
  - [ksetup changepassword](ksetup-changepassword.md)
  - [ksetup delenctypeattr](ksetup-delenctypeattr.md)
  - [ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md)
  - [ksetup delkdc](ksetup-delkdc.md)
  - [ksetup delkpasswd](ksetup-delkpasswd.md)
  - [ksetup delrealmflags](ksetup-delrealmflags.md)
  - [ksetup domain](ksetup-domain.md)
  - [ksetup dumpstate](ksetup-dumpstate.md)
  - [ksetup getenctypeattr](ksetup-getenctypeattr.md)
  - [ksetup listrealmflags](ksetup-listrealmflags.md)
  - [ksetup mapuser](ksetup-mapuser.md)
  - [ksetup removerealm](ksetup-removerealm.md)
  - [ksetup server](ksetup-server.md)
  - [ksetup setcomputerpassword](ksetup-setcomputerpassword.md)
  - [ksetup setenctypeattr](ksetup-setenctypeattr.md)
  - [ksetup setrealm](ksetup-setrealm.md)
  - [ksetup setrealmflags](ksetup-setrealmflags.md)
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L

- [label](label.md)
- [list](list.md)
  - [清單提供者](list-providers.md)
  - [清單陰影](list-shadows.md)
  - [清單寫入器](list-writers.md)
- [載入中繼資料](load-metadata.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  - [logman create](logman-create.md)
  - [logman 建立警示](logman-create-alert.md)
  - [logman 建立 api](logman-create-api.md)
  - [logman 建立 cfg](logman-create-cfg.md)
  - [logman 建立計數器](logman-create-counter.md)
  - [logman 建立追蹤](logman-create-trace.md)
  - [logman delete](logman-delete.md)
  - [logman 匯入和 logman 匯出](logman-import-export.md)
  - [logman query](logman-query.md)
  - [logman start 和 logman stop](logman-start-stop.md)
  - [logman update](logman-update.md)
  - [logman 更新警示](logman-update-alert.md)
  - [logman 更新 api](logman-update-api.md)
  - [logman 更新 cfg](logman-update-cfg.md)
  - [logman 更新計數器](logman-update-counter.md)
  - [logman 更新追蹤](logman-update-trace.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M

- [macfile](macfile.md)
- [makecab](makecab.md)
- [管理 manage-bde](manage-bde.md)
  - [管理 manage-bde 狀態](manage-bde-status.md)
  - [在上管理 manage-bde](manage-bde-on.md)
  - [管理 manage-bde](manage-bde-off.md)
  - [管理 manage-bde 暫停](manage-bde-pause.md)
  - [管理 manage-bde resume](manage-bde-resume.md)
  - [管理 manage-bde 鎖定](manage-bde-lock.md)
  - [管理 manage-bde 解除鎖定](manage-bde-unlock.md)
  - [管理再次的 manage-bde](manage-bde-autounlock.md)
  - [管理 manage-bde 保護裝置](manage-bde-protectors.md)
  - [管理 manage-bde tpm](manage-bde-tpm.md)
  - [管理 setidentifier 的 manage-bde](manage-bde-setidentifier.md)
  - [管理 forcerecovery 的 manage-bde](manage-bde-forcerecovery.md)
  - [管理 manage-bde changepassword](manage-bde-changepassword.md)
  - [管理 changepin 的 manage-bde](manage-bde-changepin.md)
  - [管理 changekey 的 manage-bde](manage-bde-changekey.md)
  - [管理 keypackage 的 manage-bde](manage-bde-keypackage.md)
  - [管理 manage-bde 升級](manage-bde-upgrade.md)
  - [管理 wipefreespace 的 manage-bde](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [md](md.md)
- [合併 vdisk](merge-vdisk.md)
- [mkdir](mkdir.md)
- [mklink](mklink.md)
- [mmc](mmc.md)
- [mode](mode.md)
- [more](more.md)
- [mount](mount.md)
- [mountvol](mountvol.md)
- [move](move.md)
- [mqbkup](mqbkup.md)
- [mqsvc](mqsvc.md)
- [mqtgsvc](mqtgsvc.md)
- [msdt](msdt.md)
- [msg](msg.md)
- [msiexec](msiexec.md)
- [msinfo32](msinfo32.md)
- [mstsc](mstsc.md)

### <a name="n"></a>N

- [nbtstat](nbtstat.md)
- [netcfg](netcfg.md)
- [net print](net-print.md)
- [netsh](netsh.md)
- [netstat](netstat.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  - [nslookup exit Command](nslookup-exit-command.md)
  - [nslookup finger Command](nslookup-finger-command.md)
  - [nslookup help](nslookup-help.md)
  - [nslookup ls](nslookup-ls.md)
  - [nslookup lserver](nslookup-lserver.md)
  - [nslookup root](nslookup-root.md)
  - [nslookup server](nslookup-server.md)
  - [nslookup set](nslookup-set.md)
  - [nslookup set all](nslookup-set-all.md)
  - [nslookup set class](nslookup-set-class.md)
  - [nslookup set d2](nslookup-set-d2.md)
  - [nslookup set debug](nslookup-set-debug.md)
  - [nslookup set domain](nslookup-set-domain.md)
  - [nslookup set port](nslookup-set-port.md)
  - [nslookup set querytype](nslookup-set-querytype.md)
  - [nslookup set recurse](nslookup-set-recurse.md)
  - [nslookup set retry](nslookup-set-retry.md)
  - [nslookup set root](nslookup-set-root.md)
  - [nslookup set search](nslookup-set-search.md)
  - [nslookup set srchlist](nslookup-set-srchlist.md)
  - [nslookup set timeout](nslookup-set-timeout.md)
  - [nslookup set type](nslookup-set-type.md)
  - [nslookup set vc](nslookup-set-vc.md)
  - [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O

- [離線](offline.md)
  - [離線磁片](offline-disk.md)
  - [離線磁片區](offline-volume.md)
- [線上](online.md)
  - [線上磁片](online-disk.md)
  - [線上磁片區](online-volume.md)
- [openfiles](openfiles.md)

### <a name="p"></a>P

- [pagefileconfig](pagefileconfig.md)
- [path](path.md)
- [pathping](pathping.md)
- [pause](pause.md)
- [pbadmin](pbadmin.md)
- [pentnt](pentnt.md)
- [perfmon](perfmon.md)
- [ping](ping.md)
- [pnpunattend](pnpunattend.md)
- [pnputil](pnputil.md)
- [popd](popd.md)
- [powershell](powershell.md)
- [powershell ise](powershell_ise.md)
- [print](print.md)
- [prncnfg](prncnfg.md)
- [prndrvr](prndrvr.md)
- [prnjobs](prnjobs.md)
- [prnmngr](prnmngr.md)
- [prnport](prnport.md)
- [prnqctl](prnqctl.md)
- [prompt](prompt.md)
- [pubprn](pubprn.md)
- [pushd](pushd.md)
- [pushprinterconnections](pushprinterconnections.md)
- [pwlauncher](pwlauncher.md)

### <a name="q"></a>Q

- [qappsrv](qappsrv.md)
- [qprocess](qprocess.md)
- [查詢](query.md)
  - [查詢處理序](query-process.md)
  - [query session](query-session.md)
  - [查詢 termserver](query-termserver.md)
  - [query user](query-user.md)
- [quser](quser.md)
- [qwinsta](qwinsta.md)

### <a name="r"></a>R

- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [復原磁片群組](recover_1.md)
- [reg](reg.md)
  - [reg 新增](reg-add.md)
  - [reg 比較](reg-compare.md)
  - [reg copy](reg-copy.md)
  - [reg delete](reg-delete.md)
  - [reg 匯出](reg-export.md)
  - [reg 匯入](reg-import.md)
  - [reg 載入](reg-load.md)
  - [reg 查詢](reg-query.md)
  - [reg 還原](reg-restore.md)
  - [reg 儲存](reg-save.md)
  - [reg unload](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [rem 批次檔](rem.md)
- [rem 腳本](rem_1.md)
- [remove](remove.md)
- [ren](ren.md)
- [rename](rename.md)
- [修復](repair.md)
  - [repair](repair-bde.md)
- [replace](replace.md)
- [l](rescan.md)
- [reset](reset.md)
  - [reset session](reset-session.md)
- [幻燈片](retain.md)
- [還原](revert.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [route ws2008](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [rundll32.exe printui.dll](rundll32-printui.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S

- [聖約瑟](san.md)
- [sc config](sc-config.md)
- [sc 建立](sc-create.md)
- [sc delete](sc-delete.md)
- [sc query](sc-query.md)
- [schtasks](schtasks.md)
- [scwcmd](scwcmd.md)
  - [scwcmd 分析](scwcmd-analyze.md)
  - [scwcmd 設定](scwcmd-configure.md)
  - [scwcmd register](scwcmd-register.md)
  - [scwcmd 復原](scwcmd-rollback.md)
  - [scwcmd 轉換](scwcmd-transform.md)
  - [scwcmd 視圖](scwcmd-view.md)
- [secedit](secedit.md)
  - [secedit 分析](secedit-analyze.md)
  - [secedit 設定](secedit-configure.md)
  - [secedit 匯出](secedit-export.md)
  - [secedit generaterollback](secedit-generaterollback.md)
  - [secedit 匯入](secedit-import.md)
  - [secedit 驗證](secedit-validate.md)
- [select](select.md)
  - [select disk](select-disk.md)
  - [select partition](select-partition.md)
  - [選取 vdisk](select-vdisk.md)
  - [select volume](select-volume.md)
- [serverceipoptin](serverceipoptin.md)
- [servermanagercmd](servermanagercmd.md)
- [serverweroptin](serverweroptin.md)
- [設定環境變數](set_1.md)
- [設定陰影複製](set_2.md)
  - [設定內容](set-context.md)
  - [設定識別碼](set-id.md)
  - [setlocal](setlocal.md)
  - [設定中繼資料](set-metadata.md)
  - [set 選項](set-option.md)
  - [設定詳細資訊](set-verbose.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shrink](shrink.md)
- [shutdown](shutdown.md)
- [模擬還原](simulate-restore.md)
- [sort](sort.md)
- [開始](start.md)
- [子命令組裝置](subcommand-set-device.md)
- [子命令集 drivergroup](subcommand-set-drivergroup.md)
- [子命令集 drivergroupfilter](subcommand-set-drivergroupfilter.md)
- [子命令集 driverpackage](subcommand-set-driverpackage.md)
- [子命令集影像](subcommand-set-image.md)
- [子命令集 imagegroup](subcommand-set-imagegroup.md)
- [子命令集伺服器](subcommand-set-server.md)
- [子命令集 transportserver](subcommand-set-transportserver.md)
- [子命令集 multicasttransmission](subcommand-start-multicasttransmission.md)
- [子命令開始命名空間](subcommand-start-namespace.md)
- [子命令啟動伺服器](subcommand-start-server.md)
- [子命令開始 transportserver](subcommand-start-transportserver.md)
- [子命令停止伺服器](subcommand-stop-server.md)
- [子命令停止 transportserver](subcommand-stop-transportserver.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)


### <a name="t"></a>T

- [takeown](takeown.md)
- [tapicfg](tapicfg.md)
- [taskkill](taskkill.md)
- [tasklist](tasklist.md)
- [tcmsetup](tcmsetup.md)
- [telnet](telnet.md)
  - [telnet 關閉](telnet-close.md)
  - [telnet 顯示](telnet-display.md)
  - [telnet 開啟](telnet-open.md)
  - [telnet 結束](telnet-quit.md)
  - [telnet 傳送](telnet-send.md)
  - [telnet 集合](telnet-set.md)
  - [telnet 狀態](telnet-status.md)
  - [telnet 未設定](telnet-unset.md)
- [tftp](tftp.md)
- [time](time.md)
- [timeout](timeout_1.md)
- [title](title_1.md)
- [tlntadmn](tlntadmn.md)
- [tpmtool](tpmtool.md)
- [tpmvscmgr](tpmvscmgr.md)
- [tracerpt](tracerpt_1.md)
- [tracert](tracert.md)
- [tree](tree.md)
- [tscon](tscon.md)
- [tsdiscon](tsdiscon.md)
- [tsecimp](tsecimp_1.md)
- [tskill](tskill.md)
- [tsprof](tsprof.md)
- [type](type.md)
- [typeperf](typeperf.md)
- [tzutil](tzutil.md)

### <a name="u"></a>U

- [diskshadow.exe 取消公開](unexpose.md)
- [uniqueid](uniqueid.md)
- [unlodctr](unlodctr_1.md)

### <a name="v"></a>V

- [ver](ver.md)
- [verifier](verifier.md)
- [verify](verify_1.md)
- [vol](vol.md)
- [vssadmin](vssadmin.md)
  - [vssadmin delete shadows](vssadmin-delete-shadows.md)
  - [vssadmin list shadows](vssadmin-list-shadows.md)
  - [vssadmin list writers](vssadmin-list-writers.md)
  - [vssadmin resize shadowstorage](vssadmin-resize-shadowstorage.md)

### <a name="w"></a>W

- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  - [wbadmin delete catalog](wbadmin-delete-catalog.md)
  - [wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  - [wbadmin 停用備份](wbadmin-disable-backup.md)
  - [wbadmin 啟用備份](wbadmin-enable-backup.md)
  - [wbadmin 取得磁片](wbadmin-get-disks.md)
  - [wbadmin 取得專案](wbadmin-get-items.md)
  - [wbadmin get 狀態](wbadmin-get-status.md)
  - [wbadmin get 版本](wbadmin-get-versions.md)
  - [wbadmin restore catalog](wbadmin-restore-catalog.md)
  - [wbadmin 開始備份](wbadmin-start-backup.md)
  - [wbadmin start recovery](wbadmin-start-recovery.md)
  - [wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)
  - [wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  - [wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  - [wbadmin 停止作業](wbadmin-stop-job.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [winsat 記憶體](winsat-mem.md)
- [winsat mfmedia](winsat-mfmedia.md)
- [wmic](wmic.md)
- [編寫](writer.md)
- [wscript](wscript.md)

### <a name="x"></a>X

- [xcopy](xcopy.md)
