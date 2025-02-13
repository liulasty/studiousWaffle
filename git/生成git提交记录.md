当使用 Git 进行版本控制时，以下是一些常见的 Git 命令用于生成、查看和格式化提交记录：

1. 最近的提交记录：
   - `git log`：显示所有的提交记录，最新的记录在最上面。
   - `git log -n <number>`：显示最近的 `<number>` 条提交记录。
   - `git log --since=<date>`：显示指定日期之后的提交记录。
   - `git log --until=<date>`：显示指定日期之前的提交记录。

2. 按特定格式显示提交：
   - `git log --oneline`：以简洁的方式显示每个提交的简短哈希值和提交信息。
   - `git log --pretty=format:"%h - %an, %ar : %s"`：自定义提交记录的显示格式。

3. 按时间线查看提交：
   - `git log --graph`：以图形化的方式显示提交记录的时间线和分支合并情况。

4. 查看指定分支的提交：
   - `git log <branch-name>`：仅显示指定分支的提交记录。

5. 搜索包含特定文本的提交：
   - `git log -S <search-text>`：搜索包含特定文本的提交记录。

6. 按作者查找提交：
   - `git log --author=<author-name>`：仅显示指定作者的提交记录。

7. 生成日志文件：
   - `git log > log.txt`：将提交记录导出到一个文本文件。

8. 自定义输出和格式化提交记录：
   - `git log --pretty=format:"%h - %an, %ar : %s" --date=iso`：自定义提交记录的输出格式，包括哈希值、作者、提交时间和信息。
