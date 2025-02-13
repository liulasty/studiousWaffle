要导出Git提交记录，可以使用 `git log` 命令。以下是一些常用的选项：

1. 导出所有提交记录到文本文件：
```
git log > commit_records.txt
```

2. 限制导出的提交记录数量：
```
git log -n <number> > commit_records.txt
```
其中，`<number>`是你想导出的提交记录数量。

3. 以图形化方式导出提交记录：
```
git log --graph > commit_records.txt
```
这将以图形化的方式显示提交记录。

注意，在执行上述命令前，确保你已经在要导出提交记录的Git仓库目录下。导出的提交记录将保存在名为`commit_records.txt`的文本文件中。