---
title: 搭配 GitHub 使用的一般 Git Bash 命令
description: 使用 GitHub 時，Git Bash 中一些最常使用的命令清單。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 4ce5d4d8ce382e9ba421c20595715ec473cca241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865003"
---
# <a name="common-git-bash-commands"></a>一般 Git Bash 命令

這些是 Git Bash 中一些最常用的命令，根據您在內容建立和編輯程式中使用它們的時機。

## <a name="master-branch-related"></a>Master 分支相關

您一定要使用 master 做為任何新分支的基礎。

| 命令 | 說明 |
|---------|-------------|
| `git checkout master` | 從任何其他分支切換到主機 |
| `git pull upstream master` | 從生產存放庫更新 master 的本機複本 |

## <a name="branch-related"></a>分支相關

| 命令 | 描述 |
|---------|-------------|
| `git branch` | 查看您現有的分支 |
| `git checkout -B <name-of-branch>` | 建立新的分支 |
| `git checkout <name-of-branch>` | 變更為另一個分支 |
| `git status` | 檢查您的分支中發生了什麼事 |
| `git branch -D <name-of-branch>` | 刪除現有分支（請確定您不在其中） |

## <a name="check-in-related-done-as-a-group-in-order"></a>簽入相關（依序以群組方式完成）

| 命令 | 描述 |
|---------|-------------|
| `git add --all` | 在您儲存工作之後，請將它新增至分支 |
| `git commit -m “public comment, including quotes”` | 將變更認可至您的分支 |
| `git pull upstream master` | 從生產存放庫更新 master 的本機複本 |
| `git push origin <name-of-branch>` | 推送至本機分支的遠端版本 |