---
title: 使用PowerShell批量重命名文件的实用技巧
date: 2024-12-15 00:47:33
tags:
    - PowerShell
categories:
    - 技术分享
---

# PowerShell 批量重命名文件的实用技巧

在日常工作中,批量重命名文件是一个常见需求。使用 PowerShell 可以轻松实现这个任务,本文将介绍几种实用的重命名方法。

## 基础语法说明

PowerShell 中进行文件重命名主要用到以下 cmdlet:

-   `Get-ChildItem`(别名:`dir`) - 获取文件列表
-   `Rename-Item`(别名:`ren`) - 重命名文件
-   `Where-Object`(别名:`?`) - 过滤条件
-   `ForEach-Object`(别名:`%`) - 循环处理

## 常用重命名示例

### 1. 替换文件名中的字符

```powershell
# 将文件名中的 "old" 替换为 "new"
dir -Recurse | ? name -Like "*.txt" | % {
    ren -NewName { $_.Name -replace 'old','new' }
}
```

这个命令会递归搜索当前目录下所有的.txt 文件，将文件名中的"old"替换为"new"。

-   `-Recurse` 表示包含所有子目录
-   `-replace` 是 PowerShell 的字符串替换操作符
-   `$_.Name` 表示当前文件的名称

### 2. 添加序号前缀

```powershell
# 为文件添加序号前缀(01,02,03...)
$i=1
dir -Recurse | ? name -Like "*.txt" | % {
    ren -NewName { '{0:d2}_{1}' -f $i++, $_.Name }
}
```

这个命令会为所有.txt 文件添加两位数的序号前缀：

-   `$i=1` 初始化计数器
-   `{0:d2}` 表示将第一个参数格式化为两位数字(如 01,02 等)
-   `$i++` 每处理一个文件自动加 1
-   `_` 作为序号和原文件名之间的分隔符

### 3. 修改文件扩展名

```powershell
# 将.txt文件改为.log
dir -Recurse | ? name -Like "*.txt" | % {
    ren -NewName { $_.Name -replace '\.txt$','.log' }
}
```

这个命令将所有.txt 文件的扩展名改为.log：

-   `\.txt$` 是正则表达式，表示文件名末尾的.txt
-   `$` 确保只匹配文件名末尾的.txt
-   `\.` 中的反斜杠用于转义点号，因为点号在正则表达式中有特殊含义

### 4. 删除文件名中的特定字符

```powershell
# 删除文件名中的空格
dir -Recurse | ? name -Like "*.txt" | % {
    ren -NewName { $_.Name -replace ' ','' }
}
```

这个命令删除文件名中的所有空格：

-   `-replace ' ',''` 将空格替换为空字符串
-   可以替换任何不需要的字符，不仅限于空格

### 5. 转换大小写

```powershell
# 转换为小写
dir -Recurse | ? name -Like "*.txt" | % {
    ren -NewName { $_.Name.ToLower() }
}

# 转换为大写
dir -Recurse | ? name -Like "*.txt" | % {
    ren -NewName { $_.Name.ToUpper() }
}
```

这两个命令分别将文件名转换为小写或大写：

-   `ToLower()` 将字符串转换为小写
-   `ToUpper()` 将字符串转换为大写
-   这些是.NET 字符串方法，可以直接在 PowerShell 中使用

## 注意事项

1. 在执行批量重命名前,建议先使用`-WhatIf`参数测试:

```powershell
dir | ren -NewName { $_.Name.ToUpper() } -WhatIf
```

2. 重命名时注意避免文件名冲突
3. 使用`-Recurse`参数时要小心,它会影响子文件夹中的文件
4. 建议在重要文件操作前先备份

## 高级技巧

### 使用正则表达式

```powershell
# 使用正则表达式提取部分文件名
dir | ? name -Match "(\d{4})-.*\.txt" | % {
    ren -NewName { $_.Name -replace "(\d{4})-.*\.txt",'$1.txt' }
}
```

### 批量处理特定日期的文件

```powershell
# 重命名最近7天修改的文件
$date = (Get-Date).AddDays(-7)
dir | ? LastWriteTime -gt $date | % {
    ren -NewName { "new_" + $_.Name }
}
```

## 总结

PowerShell 提供了强大而灵活的文件重命名功能,通过组合不同的 cmdlet 和参数,我们可以实现各种复杂的重命名需求。在使用时要注意测试和备份,避免误操作导致文件丢失。

希望本文介绍的这些方法能帮助你更高效地处理文件重命名工作!
