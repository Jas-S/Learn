# 何为Git Flow
Git Flow是一种成功的分支模式，非常适用于产品交付类型的工作。

有其它的分支模式吗？还有一种GitHub Flow, 适用于服务交付型的开发工作，比如互联网公司。主要区别在于Git Flow使用develop分支作为开发人员的集成分支，Github Flow则直接在master/main分支上集成代码。

# 分支类型
Git Flow的分支类型：
|   名称    |   说明   |     命名      | 类型  | 备注   
|---------|--------|-------------|-----|----|
|master分支 |  发布分支  | master或main |长期分支 | 在该分支打发布版本标签（基线化）|
|develop分支|  开发分支  |   develop   |长期分支 | 不接受任何修改，只接受合并|
|feature分支|  功能分支  |feature/FOO-1|临时分支 | 基于develop分支创建，完成后合并到develop分支|
|bugfix分支 |  修复分支  |bugfix/FOO-2 |临时分支 | 基于develop分支创建，完成后合并到develop分支|
|hotfix分支 | 紧急修复分支 |hotfix/FOO-3 |临时分支 | 基于基线创建分支，完成后需合并到develop及master分支并发布（0.0.1），该发布不需要新建release分支|
|release分支|发布前的准备工作|release/1.0.2|临时分支 | 基于develop分支创建，准备工作和测试工作完成后需合并到develop及master分支|

还有一个老版本维护分支，这个在规范上没有定义，实际上也很常见：
|   名称    | 说明 |      命名      | 类型|备注
|---------|----|--------------|---|---|
|support分支|维护分支|support/1.x | 创建后即为长期分支|作为老版本的master分支来使用|

实验环境：
https://k.swd.cc/learnGitBranching-ja/?NODEMO

