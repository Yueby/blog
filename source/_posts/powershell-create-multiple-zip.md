---
title: PowerShell批量压缩文件：内置命令vs 7-Zip方案对比
date: 2024-12-14 15:34:11
tags:
    - PowerShell
    - 7-Zip
categories:
    - 技术分享
---

# PowerShell 批量创建压缩包

在日常工作中，我们可能需要将多个文件分别打包成独立的压缩文件。这里介绍两种方法：使用 PowerShell 内置命令和使用 7-Zip 来实现。

## 方法一：使用 PowerShell 内置命令

使用 PowerShell 的`Compress-Archive`命令可以直接实现压缩功能：

```powershell
Get-ChildItem -Path "C:\源文件夹" -File | ForEach-Object {
    Compress-Archive -Path $_.FullName -DestinationPath "C:\目标文件夹\$($_.BaseName).zip" -Force
}
```

### 代码说明

-   `Get-ChildItem`: 获取指定路径下的所有文件
-   `-Path`: 指定源文件夹路径
-   `-File`: 只获取文件，不包括文件夹
-   `ForEach-Object`: 对每个文件执行压缩操作
-   `Compress-Archive`: PowerShell 的压缩命令
-   `-Force`: 如果目标 zip 文件已存在，则覆盖它

## 方法二：使用 7-Zip

如果需要更多压缩选项或更好的压缩率，可以使用 7-Zip：

```powershell
Get-ChildItem *.unitypackage | ForEach-Object {
    &"E:\7-Zip\7z.exe" a -tzip "$($_.BaseName).zip" $_.Name
}
```

### 代码说明

-   `Get-ChildItem *.unitypackage`: 获取当前目录下所有 .unitypackage 文件
-   `&"E:\7-Zip\7z.exe"`: 调用 7-Zip 程序（需要根据实际安装路径调整）
-   `a`: 添加文件到压缩包
-   `-tzip`: 指定压缩格式为 zip
-   `$($_.BaseName).zip`: 使用原文件名（不含扩展名）作为 zip 文件名
-   `$_.Name`: 要压缩的源文件名

也可以使用更简短的命令形式：

```powershell
dir | ? name -Like "*.unitypackage" | % {
    &"E:\7-Zip\7z.exe" a -tzip "$($_.BaseName).zip" $_.Name
}
```

## 使用注意事项

1. 确保有足够的磁盘空间
2. 路径中避免特殊字符
3. 确保具有相应文件夹的访问权限
4. 使用 7-Zip 方法时，需要先安装 7-Zip 并确保路径正确

## 总结

这两种方法各有特点：

-   PowerShell 内置命令使用简单，无需额外软件
-   7-Zip 方法提供更多压缩选项，压缩率通常更好

根据实际需求选择合适的方法即可。
