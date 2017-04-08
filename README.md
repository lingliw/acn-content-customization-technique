# 序

这是我总结这两年工作所得到的所有技巧。文章通篇都关于技巧，然而技巧并不重要，重要的是发现技巧的思维。这样的思维主要有 4 个步骤，[发现问题](#step1)，[寻找问题的根本原因](#step2)，[根据原因找到解决的方案](#step3)，[最后是分享](#step4)。以下通过举例，来分析说明这 5 个步骤分别如何完成。如果对这部分的内容不感兴趣，可以直接跳到文档的[目录](#toc)

## <a name="step1"></a> 1. 发现问题

工作时，总是会遇到各式各样的问题。一般的思维是，碰到了问题，解决当下的问题，然后继续当前的工作。然而，这样并不足够。因为如果同样的问题重复出现，每一次都只解决当前的状况，就有可能极大地影响工作效率。发现技巧的思维需要想得更多，而这里说的“发现问题”并不是指简单的发现，而是要发现问题是否会重复出现，是否会影响工作效率，是否可以有一劳永逸的解决方法，解决方法是否有副作用，副作用是否可以接受。

前面两个“是否”是简单的。一般来说，问题出现第二遍，第三遍，就基本可以断定它会重复出现。当然，有些关键问题，出现一遍就基本可以断定它会重复出现。至于是否影响工作效率，有两个评判标准：

1. 是否影响工具的运行，“是”既有影响；
1. 是否可以在日常工作中毫不费功夫地顺带解决，“否”既有影响。

前面两项是必要性，而后面四项是可行性。可行性的判断需要常年累月的积累，除了需要各种知识的积累（如对编程语言的了解、对系统资源的了解、对现有工具的了解等），还需要对自身能力的判断，以及对自己工作时间的把握。原则只有一个，**一切工具的开发都以不影响本职工作为前提，换句话说，请做好文档的定制再考虑工具的问题**。

以下是一个关于 Beyond Compare 的编码问题的发现以及解决。这里就以这个小问题为例子。更多关于 Beyond Compare 和文档编码，请查看 [Beyond Compare 使用技巧](./5-common-tools/1-beyond-compare.md) 以及[文档编码技巧](./7-other-techniques/1-document-encodings.md)

自从 azure-content-pr (现在是 azure-docs-pr) 里的文档全都改成使用 UTF8 编码之后，我就放弃了在我写的工具里使用编码检测方法，因为实在是太费时费力了，而且还不能保证 100% 准确。取而代之，我假设所有文档都是用 UTF8，于是我所有工具都只读写 UTF8。当然，在这样子做之前，我已经一次性地把我们的所有文档都转成了 UTF8。

然而，最近在为翻译组的 OL 系统准备源时，我发现有一些 azure-content-mooncake-pr (已被 mc-docs-pr.en-us 代替) 里的文档变成不是 UTF8。开始并没有在意，因为这个有可能是组员在编辑文档的时候，使用了中文输入法，不小心加入了全角符号，并保存为系统默认的 GBK 编码。于是我只是把那些文档又改成了 UTF8，并提醒了组员们要注意使用 UTF8 编码。然而同样的问题很快又发生了，通过与组员的沟通，也知道他们并没有主动编辑相关文档，而只是在 Beyond Compare 中把内容复制过去而已，所以我推断这并不是简单的输入法与全角符号的问题。

首先是必要性：它出现了第二遍，而且并不是简单的人为疏漏，所以可以断定它会重复出现。而且由于编码不是 UTF8，工具根本读取不了，确实影响了其运行，已经是符合第一个标准。至于第二个标准，这次可以跳过，不过后来在寻找原因的时候也发现了，这是不能在日常工作中毫不费功夫地顺带解决。

然后是可行性：通过沟通，我已经知道大概率是编辑器的问题，也就是很可能是 Beyond Compare 的问题。而改变 Beyond Compare 的默认打开编码应该是可以的，而且这样的改变应该是可以在一个小时以内完成。

必要性和可行性的辩证已经完成，而问题原因的大概方向也已经知道。接下来就是朝着这个方向去寻找根本原因，然后根据这样的原因给出解决方法。

## <a name="step2"></a> 2. 寻找问题的根本原因

根本原因很重要，直接决定了解决方案的实现。寻找根本原因需要一颗刨根问底的心，做技术支持的都应该知道，如果没有一颗刨根问底的心，就不可能找到根本原因，那么给出来的永远都不是 solution，而只能算是 workaround。而刨根问底需要以下方法。

### I. 沟通

沟通是很重要的，显而易见。虽然我沟通基本靠吼，你们也许已经厌倦了我这狂躁的脾气。需要沟通出于两个考虑：

1. 别人很可能也遇到一样的问题，收集更多的使用场景，对于寻找原因是非常重要的。
1. 你碰到的问题很有可能是别人的失误导致的，如果不去沟通，导致同样问题的原因可能有千千万万。

还是以之前的编码问题为例，我确认 mc-docs-pr.en-us 中两篇 SQL Database 的文档编码又变成了不是 UTF8。而 SQL Database 的文档当前是由 Johnny 负责的。也就是说，大概率是 Johnny 在某一次操作中又把文档覆盖成不是 UTF8。联系到同样是他负责的服务，Media Services，里面有一个文档在源 azure-docs-pr 就已经不是 UTF8。所以其中一个可能是，Johnny 用源里的非 UTF8 文件覆盖了我在 mc-docs-pr.en-us 中已改成 UTF8 的文件。另外，他还曾经在旧版 ACN md 文档的 XML 格式的 Meta Data 里使用了全角引号，所以还有另一种可能性，他在定制文档的时候使用了中文输入法输入了全角符号，并保存为系统默认的 GBK。

然而跟 Johnny 沟通过以后，以上两种可能都被否定了。首先，他说除了新文件会直接复制文件过去之外，其他文件都是通过 Beyond Compare 逐行复制的，也就是说，“覆盖”论不对。并且，他坚持说相关的两个文件都没有过手动编辑，也就是说“输入法”论不可能。而当他在他的机器用 Beyond Campare 打开那些文档的时候，我已经大概明白问题的所在。左边是 azure-docs-pr 使用的是 UTF8，右边是 mc-docs-pr.en-us 使用的是 ASCII。更加确定了“覆盖”论和“输入法”论的不正确。因为源是 UTF8，覆盖之后应该依然是 UTF8；而如果是输入法，保存下来的应该是 GBK，而不是 ASCII。现在已经得出是 Beyond Compare 的问题，然而具体的场景应然需要更精细的琢磨。

### II. 安静而辩证的思考

这样的思考貌似有点随缘，经常是想了很久，想来想去都想不出来，然而突然一个想法，或者突然别人的一句话就会让你想通了。所以，以下只是几个提高想通速度和概率的方法。

1. 对于不紧急的问题不要过于执着，收集好必要的信息之后，可以选择继续日常的工作。
1. 而如果是紧急的问题，可以选择问别人，多一个人，多一个角度，多一个想法，对于思考都是很有帮助的。
1. 尽量罗列出各种可能性，然后逐个排除，最后得出答案。

然而，这三个方法都不一定管用。第一个方法经常是放着放着就忘掉了，或者很久很久之后才想通。第二个方法有时最终也没有得出满意的答案，而且还多浪费了一个人的时间。第三个方法很经常是各种可能性都被排除，最后得不出任何答案。但，这一切都是值得的，只有不断地思考才能最终解决一切可以解决的问题，而不是对问题视而不见，或者用蛮力解决，浪费更多的时间。

回到之前的例子，这个问题的原因是在当天睡觉前突然就想到了的。排除法没有得到的答案就突然的想通了。

关于这个问题，首先要想的是为什么左边是 UTF8，而右边是 ASCII。

### III. 问题重现

## <a name="step3"></a> 3. 根据原因找到解决的方案

## <a name="step4"></a> 4. 分享

## <a name="toc"></a> 目录

* [本地环境](./1-local-environment/)
* [内容的定制](./2-content-customization/)
* [python 与正则表达式](./3-python-and-regular-expression/)
* [我写的工具](./4-self-written-tools/)
* [其他常用工具](./5-common-tools/)
    * [Beyond Compare 使用技巧](./5-common-tools/1-beyond-compare.md)
    * [VS Code 使用技巧](./5-common-tools/2-vs-code.md)
    * [Git Shell 使用技巧](./5-common-tools/3-git-shell.md)
    * [SmartGit 使用技巧](./5-common-tools/4-smartgit.md)
    * [Source Tree 使用技巧](./5-common-tools/5-source-tree.md)
* [Azure 服务](./6-azure-services/)
* [其他技巧](./7-other-techniques/)
    * [文档编码技巧](./7-other-techniques/1-document-encodings.md)