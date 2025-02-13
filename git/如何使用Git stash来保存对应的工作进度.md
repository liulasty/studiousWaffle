### 1. 保存当前工作进度

当你在一个分支上工作，但临时需要切换到另一个分支时，你可以使用 `git stash` 命令来保存当前的工作进度。这个命令会将你所有未提交的修改（包括工作目录和暂存区的改动）保存到一个栈中，让你的工作目录回到上一次提交的状态。

```bash
git stash
```

或者，如果你想为这次 stash 添加一个描述性信息，可以使用：

```bash
git stash save "你的描述信息"
```

### 2. 查看已保存的 stash 列表

你可以使用 `git stash list` 命令来查看所有已经保存的 stash 条目。这个命令会列出所有的 stash，以及每个 stash 的描述信息和唯一标识符（如 stash@{0}, stash@{1} 等）。

```bash
git stash list
```

### 3. 应用或恢复 stash

- **应用 stash 并保留它**：使用 `git stash apply` 命令可以将一个 stash 应用到当前的工作目录。如果你不指定 stash 的标识符，它将默认应用最新的 stash（即 stash@{0}）。应用后，stash 仍然保留在栈中，你可以稍后再次使用它。

```bash
git stash apply# 或者指定一个特定的 stashgit stash apply stash@{num}
```

- **应用 stash 并删除它**：使用 `git stash pop` 命令与应用 stash 类似，但在应用后，该 stash 会从栈中删除。

```bash
git stash pop
# 或者指定一个特定的 stashgit stash pop stash@{num}
```

### 4. 删除 stash

- **删除特定的 stash**：使用 `git stash drop` 命令可以删除一个指定的 stash。

```bash
git stash drop stash@{num}
```

- **删除所有 stash**：使用 `git stash clear` 命令可以删除栈中的所有 stash。

```bash
git stash clear
```

### 5. 查看 stash 的内容

如果你想查看一个 stash 包含哪些具体的修改，可以使用 `git stash show` 命令。

```bash
git stash show# 或者查看一个特定的 stashgit stash show stash@{num}
```

### 6. 从 stash 创建新分支

有时候，你可能想从一个特定的 stash 开始一个新的分支进行开发。这时，你可以使用 `git stash branch` 命令。这个命令会创建一个新的分支，并将指定的 stash 应用到这个新分支上。

```bash
git stash branch new-branch-name stash@{num}
```

如果不指定 stash 的标识符，它将默认使用最新的 stash。

### 注意事项

- 在应用 stash 之前，确保你的工作目录是干净的（即没有未提交的修改），这样可以避免潜在的冲突。
- 如果在应用 stash 时遇到冲突，Git 会提示你解决这些冲突。解决后，你需要使用 `git add` 命令来标记已解决的冲突文件，然后才能继续你的工作。
- 谨慎使用 `git stash clear` 命令，因为它会删除栈中的所有 stash，而这些 stash 可能包含你重要的未提交修改。

通过遵循这些步骤和注意事项，你可以有效地利用 Git stash 功能来管理你的工作进度和版本控制流程。