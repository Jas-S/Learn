# 何为Git Flow
Git Flow是一种成功的分支模式，非常适用于产品交付类型的工作。

有其它的分支模式吗？还有一种GitHub Flow, 适用于服务交付型的开发工作，比如互联网公司。主要区别在于Git Flow使用develop分支作为开发人员的集成分支，Github Flow则直接在master/main分支上集成代码。

# 分支类型
Git Flow的分支类型：
|   名称    |   说明   |     命名      | 类型  |
|---------|--------|-------------|-----|
|master分支 |  发布分支  | master或main |长期分支（在该分支打版本标签） |
|develop分支|  开发分支  |   develop   |长期分支 |
|feature分支|  功能分支  |feature/FOO-1|临时分支 |
|bugfix分支 |  修复分支  |bugfix/FOO-2 |临时分支 |
|hotfix分支 | 紧急修复分支 |hotfix/FOO-3 |临时分支 |
|release分支|发布前的准备工作|release/1.0.2|临时分支 |

注：所有临时分支对应结束后要同时合并到master及develop分支。

还有一个老版本维护分支，这个在规范上没有定义，实际上也很常见：
|   名称    | 说明 |      命名      |
|---------|----|--------------|
|support分支|维护分支|support/1.0.0 |

实验环境：
https://k.swd.cc/learnGitBranching-ja/?NODEMO

