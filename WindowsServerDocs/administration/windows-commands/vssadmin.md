---
title: Vssadmin
description: Vssadmin 命令的總覽。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: f3618841eb2f511323873d2ea962838f9ab777d0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954690"
---
# <a name="vssadmin"></a>Vssadmin

> 適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

顯示目前的磁片區陰影複本備份，以及所有已安裝的陰影複製寫入器和提供者。 在下表中選取命令名稱，以查看其命令語法。

|命令|描述|可用性
|---|---|---
|[Vssadmin add shadowstorage](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788051(v%3dws.11))|新增磁片區陰影複製儲存體關聯。| 僅限伺服器
|[Vssadmin 建立陰影](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788055(v%3dws.11))|建立新的磁片區陰影複製。| 僅限伺服器
|[Vssadmin 刪除陰影](vssadmin-delete-shadows.md)|刪除磁片區陰影複製。| 用戶端與伺服器
|[Vssadmin delete shadowstorage](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc785461(v%3dws.11))|刪除磁片區陰影複製儲存體關聯。| 僅限伺服器
|[Vssadmin list providers](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788108(v%3dws.11))|列出已註冊的磁片區陰影複製提供者。| 用戶端與伺服器
|[Vssadmin 清單陰影](vssadmin-list-shadows.md)|列出現有的磁片區陰影複製。| 用戶端與伺服器
|[Vssadmin list shadowstorage](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788045(v%3dws.11))|列出系統上的所有陰影複製儲存區關聯。| 用戶端與伺服器
|[Vssadmin 清單磁片區](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788064(v%3dws.11))|列出適用于陰影複製的磁片區。| 用戶端與伺服器
|[Vssadmin list 寫入器](vssadmin-list-writers.md)|列出系統上所有已訂閱的磁片區陰影複製寫入器。| 用戶端與伺服器
|[Vssadmin resize shadowstorage](vssadmin-resize-shadowstorage.md)|調整陰影複製儲存區關聯的大小上限。| 用戶端與伺服器
