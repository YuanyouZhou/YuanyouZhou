# Git与GitHub的入门指南

## Git/GitHub简介

Git是由Linus Torvalds（开发Linux内核的那位）开发的开源分布式版本控制软件，最早用于管理维护Linux内核的代码，现在也被广泛地应用在各种开源软件的管理当中。目前最为通行的版本控制软件中还有集中式的SVN，这里主要介绍Git。

版本控制软件的作用在于管理软件代码的不同版本，并对软件源代码的变化进行跟踪。它可以储存记录项目中文件的变化情况与不同版本，并且在有需要时实现不同版本中文件的合并，以及出现BUG时进行回溯等等。这一点对于团队协作而言至关重要，而对于个人开发者而言，使用Git进行管理相对而言也更加便于维护，且可以提高代码编辑的安全性。Git会跟踪工作区中所有文件的变动情况，包括代码、媒体素材等等，用在游戏开发当中也是相当合适的。Git本身只是作为一个版本控制软件内核进行开发的，它可以被独立用作一个命令行工具，也可以配合相应的前端进行工作。

对于一个具有一定规模的软件工程项目而言，将Git仓库推送到网页上进行托管可以带来许多便利。GitHub就是一个软件项目的托管平台，它只支持使用Git作为唯一的版本库进行控制，由此得名GitHub。GitHub可以帮助托管开源或私有两种类型的仓库，其中公开仓库免费而私有仓库会收费。与此同时，GitHub上面还提供了一系列团队协作工具，可以帮助团队更好地开展工作。

这里我们主要结合GitHub对Git的使用做一些简单的介绍。如果要进行闭源商用项目开发的话，也可以付费使用GitHub的私有仓库进行工作，或者自行搭建Git服务器进行协作等。

本文仅作为简单的入门指南使用，两工具更加详细的使用方法还请查阅官方文档或帮助页面（[Git](https://book.git-scm.com/doc), [GitHub](https://support.github.com)）或者查找其他相应的资料。

## 开发环境配置

> Git软件的安装。

Git的安装方式有很多，以下仅根据不同操作系统分别给出一些简便的安装方式。

#### Windows

Windows版本的Git可以直接通过[Git官网](https://git-scm.com)下载，并按照指导完成安装。通常其中的选项全部保持默认即可，也可以按照自己的偏好进行更改。

Windows版本的Git自带有一个Git Bash终端，在默认的安装选项当中也可以通过Windows自带的命令行使用Git。

#### macOS

我们推荐直接安装`Xcode Command Line Tools`，其中包括了软件开发所需要使用的编译器、管理工具等等一系列工具，当然也包括Git。这可以直接前往App Store安装Xcode，初次打开后会指导安装这个工具集。如果不想同时使用Xcode，也可以直接打开终端运行

```shell
xcode-select --install
```

即可完成`Xcode Command Line Tools`的安装工作。

#### Linux

在Linux平台上可以使用包管理器apt-get或者yum完成安装工作。因为Git在运行过程当中需要使用到一些外部库中的代码，所以应当首先安装依赖。具体指令如下：

```shell
apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
apt-get install git
```

（不过人都用上Linux了，装个Git自己随便怎么整都行吧。

安装完毕之后，可以在终端输入指令

```shell
git --version
```

如果成功输出Git的版本号，则说明安装工作已经顺利完成。

> 环境变量的设定工作。

Git当中提供了`git config`来对环境变量进行设置，通常而言当终端位置处在某一个Git工作目录之下时，这个命令会根据当前所在目录修改当前仓库下的设置。我们也可以通过添加选项`--global`来进行全局设置。当全局设置与当前工作目录下的设置不相同时，会优先使用当前目录下的设置。我们首先设置Git当中的用户名和邮箱信息：

```shell
git config --global user.name "你的名字"
git config --global user.email 你的邮箱地址
```

由于Git是一个分布式版本控制软件，git中的用户名和邮箱同时也会起到标记的作用，方便后期对源代码的贡献进行溯源。

我们也可以随时通过指令`git config --list`查看当前的设置情况。当有需要时也可以随时在命令行输入`git`查看常用指令，也可以使用`git help`查看更加详细的说明。

> GitHub注册

接着我们前往[GitHub官网](https://www.github.com)注册一个账号。这只是一个常规的账号注册流程，在此不多赘述。账号注册完成之后可以尝试进一步完善个人信息，逐步摸索GitHub当中的各种功能等等。我们在本文中仅能介绍到GitHub的少数核心功能，更多内容还需要在长期的使用中逐渐熟悉。

注意GitHub当前在中国国内处于半墙状态，需要使用科学上网手段才能流畅访问。

> 完成Git与GitHub的绑定

我们希望通过Git与GitHub实现项目的版本控制，此时还需要将本地的Git与自己的GitHub账号进行绑定，以便通过Git操作远程仓库。为确保信息安全，Git与GitHub之间的通信与操作都是通过SSH进行加密授权的，如果先前的Git安装过程一切正常，此时你的设备上应该也已经完成了SSH的安装。只需在命令行中输入`ssh`，此时应该会输出如下结果：

![SSH输出结果示例](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/5MHTrL.png)

如此一来我们就可以开始后续操作了。

在命令行中输入如下指令：

```shell
ssh-keygen -t rsa
```

指定通过rsa算法生成密钥。此后连敲三下回车，期间不需要输入密码。此后会在默认目录下生成两个文件`id_rsa`和`id_rsa.pub`，前者是密钥，后者是公钥。我们需要将公钥中的内容放到GitHub上。根据使用操作系统的不同，指令生成的密钥和公钥的默认路径也有所区别，分别是：

* Linux/macOS: ~/.ssh
* Windows: C:\Documents and Settings\username\\.ssh

需要注意的是它们都位于以`.`开头的隐藏文件夹当中，可以通过终端命令进入这个文件夹当中，也可以打开显示隐藏文件夹之后通过图形界面进行操作。总之，我们需要的是文件夹当中公钥的文本内容，这可以通过使用记事本打开文件`id_rsa.pub`得到。

接着我们进入GitHub，打开个人设置，在左侧选项卡中找到`SSH and GPG keys`。

![GitHub设置](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/GitHub设置.png)

![SSH-key](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/SSH-key.png)

Title可以按照喜好随意选取，将公钥中文本内容粘贴到key一栏即可。

接着我们可以进行测试，确认绑定是否正确完成。打开终端输入

```shell
ssh -T git@github.com
```

如果输出内容中有`You've successfully authenticated`则说明环境已配置完成！

## Git/GitHub基本概念

我们首先对整个GitHub工作流当中的基本概念做出一些解释，此后会在实际操作中熟悉各个环节的具体操作。Git/GitHub工作流当中的主要概念以及关系大致如下图所示。

![Git仓库概念图](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/Git仓库.png)

> Git仓库(Repository, repo)

我们的整个Git的工作流都围绕着「Git仓库」展开。一个Git仓库其实就是一个文件夹，其中添加了一个隐藏的子文件夹`.git`，并受到Git的管理。Git仓库当中可以存储任何类型的文件。根据仓库存储位置及内容的不同，我们工作流中主要使用的仓库有三类，分别是Remote远程仓库，Repository本地仓库以及工作区。其中远程仓库可以有许多个，包括项目的主仓库以及自己的Fork等。

在使用`git init`新建Git仓库或者从远程拉仓库新建时，Git会同步在工作区之外新建一个本地仓库，其中存储着项目开发过程中所迭代的各个版本的源文件以及相关信息。这个仓库通常仅使用Git进行访问，一方面对本地工作进行管理，同时也起到一个备份的作用防止意外。

在项目的开发过程中，这些仓库当中的内容可能会存在很多不同的版本，而我们可以通过Git的各种命令等（如图所示）来实现仓库中内容的流转。例如工作区是我们正常编写程序以及调试的位置，假如在工作中突然遭遇了未知原因的严重BUG，则可以通过`git checkout`这一指令拉取本地中上一个稳定版本的源文件并进行覆盖回滚。

> Fork

「Fork」原意为「叉子」，在GitHub工作流当中引申含义为项目的版本分支，即项目的源代码从某一处开始分离成了不同的版本。项目的不同开发者会以某一个版本为基础进行独立开发，由此产生了内容上的差异。当独立开发工作完成后，我们会通过Pull Request申请将代码合并进入主分支。这通常会产生很多冲突，需要开发者手动解决，并最终完成整合工作。

「Fork」的作用和后文中所提到的「分支」在很大程度上是重叠的，实际上尽管GitHub、GitLab等平台都同时支持两种开发模式，但是对于简单的项目而言，我们只需要使用其中一种就已经完全足够了。在这里我们主要介绍Fork的用法。

在GitHub上，仓库页面的右上角可以找到Fork选项：

![Fork示意图](../_media/Fork.png)

Fork会在你自己的名下新建一个原仓库的副本。通常来说仓库的所有者不会允许你直接对原仓库进行修改，因为这样做的后果将难以预料。此时你就需要进行Fork，并在原仓库的副本中进行工作，完成后再通过Pull Request申请合并进入主仓库。

> Pull/Push

Pull/Push的意义分别是「拉取」和「推送」，分别对应于从远端仓库获取文件以及将自己的工作成果推送到远端仓库两种操作。Git用户常说的拉仓库也就是这么一回事。需要注意的是Pull是直接将远程仓库当中的内容拉取到本地工作区当中，同时也会记录进本地仓库，而Push则是将本地仓库当中的内容推送到远程仓库当中，两者的操作对象并不是对应的，push之前还隔着add和commit两个步骤。通常而言，这两项指令所涉及的远程仓库仅是主仓库的一个Fork。

> Add/Commit

这两个Git指令的作用分别是将相应的文件添加到暂存区，以及确认暂存区当中的更改并形成一个「commit」添加到本地仓库当中去。

暂存区，顾名思义，其用途就是暂时存储项目中的更改。它的主要作用是通过多一级缓存提高Git的运行效率，同时也使得我们可以选择性地提交更改。

在经过一定测试确保当前工作区内容稳定性之后，就可以将文件添加进暂存区并进行commit。commit是GitHub工作流当中一个叫为核心的概念。建立commit的完整指令如下：

```shell
git commit -m "一些文字说明"
```

commit是对开发工作阶段性成果的确认，后面会要求附上一段文字说明作为附加信息。这段文字说明以及commit中对源文件所做的改动、作者信息以及时间信息等都会被完整地记录在Git仓库当中，并且在推送后在云端长期保存。在进行后续代码回溯操作时，Git基本也是以commit作为定位标准的。可以将commit理解为开发流程当中确认的较为稳定的小版本，在Git相关的图形化界面当中，commit也基本是以工作流中的「节点」的形象呈现的。commit数量也是软件版本的基本计数单位。

> Pull Request(PR)

PR也是整个工作流当中的核心操作之一。它的命名初看时会相当令人费解，因为这是从自己的Fork向主仓库进行推送的操作，但它却被称作「拉取申请」。GitHub对此的解释是，通常而言贡献者自己实际上是没有权限直接向主仓库进行推送的，所以只能申请原仓库的所有者或核心开发者对你的仓库进行拉取，所以称作Pull Request。

要对原仓库发起Pull Request，可以首先进入自己的Fork当中，这时就可以看到上方选项当中的Pull Request按钮。而对于Fork而来的仓库而言，仓库顶部还会有这样一栏提示：

![PR示例](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/2fVcUs.png)

其中选择「Contribute」即可看到自己的与主仓库的进度差异情况（以Commit数量计算），并且可以在此选择发起Pull Request。后面一项「Sync Fork」的作用则是同步当前仓库与主仓库的进度，属于反向的的Pull Request。因为Fork仓库在建立之后会维持原状，避免代码冲突，因而不会同步主仓库的开发进度。在申请向原仓库进行代码推送之前，还需要首先进行同步，并手动解决可能的冲突问题。

## 基本使用实例

在对Git/GitHub的工作流当中的一些基本概念有了一定了解之后，我们就可以开始在工作中进行一些实际操作了。实际上Git的基本使用并不复杂，稍微进行一些实操了解它的工作方式之后，它并不会给后续工作造成太多的困难。当前我们的使用示例中并不会使用到Git的分支功能。

### 建立Git工作区

这其实并没有什么复杂的地方。如同前文所述，一个Git仓库就是一个文件夹。我们首先需要选定一个合适的文件目录，在此新建一个文件夹，在这个文件夹中打开终端。此时我们可以输入指令

```shell
git status
```

查看文件夹当前的状况。当然，因为目前这个文件夹中还没有任何内容，所以Git会提示这里不是一个Git仓库。此时我们可以在当前目录下执行

```shell
git init
```

这将会对当前目录进行初始化，新建一个名为`.git`的子目录，并将当前文件夹转变为一个Git仓库纳入Git进行管理。我们可以在本地完成初始化之后将这个本地仓库与远程仓库建立关联，不过这个操作稍有些复杂，我们放到「更多基本使用技巧」当中进行讲解。其实对于通常的项目而言，我们可以使用GitHub的图形化界面完成仓库的创建工作。GitHub本身也提供了很多便利的工具帮助我们快速实现这一过程。这样可能会显得逼格略低，但是有效。

我们直接进入到GitHub主页当中，点击右上角的加号，下方第一个选项就是新建仓库。

![新建repo](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/新建repo.png)

点选之后我们会进入到仓库的创建设置页面。最前面是基本的仓库名、简介、公开性等常规选项，可以根据情况填写与选择。在此之后，下方会有三个非常实用的仓库初始化选项，需要稍微做一些解释。

![repo初始化设置](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/repo初始化设置.png)

* Add a README file

这个选项会在仓库建立时在根目录下新建一个`README.md`文件。GitHub会在仓库目录中寻找`README.md`，并将它渲染展示在仓库下方，起到一个简介和提示的作用。通常的公开项目都会配备这样一个说明文档。在GitHub当中，`README.md`可以使用Markdown格式进行书写，也支持插入HTML语法。

* Add .gitignore

这一项会根据你的选择在根目录下新建一个`.gitignore`文件。`.gitignore`文件是跟Git指令`git add *`等大量添加文件的指令配合使用的。例如上面这一条指令会将当前目录下的所有文件都添加到暂存区当中，但很多时候我们只希望添加项目相关的一部分文件，而忽略掉项目编译、测试中间产物等无关内容。`.gitignore`就起到了设定忽略项这样一个功能。

我们可以自行编写`.gitignore`，但是GitHub本身在新建仓库时提供了大量的模版可供选择，会方便很多。例如如果我们想要新建一个Godot引擎的游戏开发项目，就可以在GitHub提供的模版当中选定Godot，这会为后面的工作省去很多麻烦。

* Choose a license

选择一个开源许可证。通常开源项目都需要选择一个许可证，用以规范项目发展过程中可能遭遇的著作权益等问题。等需要用到的时候自会了解其用法，这里可以不用特别在意。

以上三个初始化选项都可以留空，等到后续有需要时再手动添加。不过GitHub建立仓库的初始化选项确实给我们提供了不少便利。

新建完毕之后我们就会进入到仓库页面当中，接下来我们将GitHub上的远程仓库拉取到本地。选择仓库上方的Code选项，找到Clone链接，点击复制。

![gitclone演示](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/gitclone演示.png)

此后回到本地预定的工作文件夹当中，打开终端，使用指令：

```shell
git clone 复制来的链接
```

即可将GitHub上面的仓库复制到当前目录之下，这一过程也会同时在本地建立Git本地仓库。可以看到原路径下方多出了一个文件夹。接着我们可以让终端进入这个文件夹，再次输入

```shell
git status
```

查看当前目录的状态。不出意外的话此时命令行中就可以正常输出Git仓库信息了。

### 修改与推送操作

仓库信息确认正常后，我们就可以开始进行一些进一步的仓库操作了。首先我们可以对仓库当中的文件进行一些更改，例如在`README.md`当中加上一句「Hello Git!」

![Hello Git](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/VpTlsK.png)

注意看编辑器行号左侧区域！绝大多数IDE/编辑器都会内置Git的支持，当你的文件正处于一个Git仓库当中时，它们就会开始访问本地仓库中对应的仓库并进行比对。我们对其中文件的增删修改等都会通过不同地形式呈现在编辑界面上。

我们保存更改并退出。此时再输入命令`git status`，它就会提示我们有未提交的更改项。

![修改尚未加入提交](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/DqpIrp.png)

接下来我们确认修改并推送到远程仓库当中。首先应当使用`git add`将修改后的文件添加到暂存区：

```shell
git add README.md
```

它的基本用法是`git add`后跟文件名/路径，支持正则表达式匹配。也可以使用这一命令一次性提交全部更改：

```shell
git add *
```

当然，这里全部提交的同时会忽略掉`.gitignore`当中的内容。

紧接着我们通过commit确认更改。

```shell
git commit -m "updated README.md"
```

此时命令行中会提示此次commit中所做出的改动。我们可以直接将这一更改推送到GitHub仓库中，使用：

```shell
git push
```

初次向GitHub远程仓库提交代码时会要求输入GitHub账号及密码进行确认。此后不出意外的话推送应该已经完成，可以回到GitHub仓库页面刷新检查，就可以在下方看到我们所做出的更改了。

![GitHub改动](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/m2Dlk1.png)

#### 版本回退

最后，在工作过程中，如果你不幸遭遇了不明原因的错误而需要手动回退，则可以使用如下指令：

```shell
git reset HEAD				#回退所有内容到上一次commit时的状态
git reset HEAD README.md 	#将某一个文件(这里是README.md)回退到上一次commit时的状态
git reset HEAD^3 			#将所有内容回退到三个版本前（三个commit前）
```

以此类推。注意这三个指令的操作对象都是暂存区中的文件，要将暂存区中的回退拉回到工作区中，我们可以使用这一指令：

```shell
git checkout . 		#将工作区中的所有文件全部回退
git checkout <file> #将工作区中的某一个文件用暂存区中文件替代
```

我们也可以一步到位，直接使用指令：

```shell
git checkout HEAD .
git checkout HEAD <file>
```

这两个指令相当于连续执行了上述两种指令，会同时用上次commit的内容替换掉暂存区和工作区中的所有文件。这些版本回退操作都相当危险，一旦使用将会完全丢失已更改内容，并且无法追回，需要谨慎使用。例如我们对当前目录下的`README.md`进行修改，使其中内容变为：

![添加错误内容](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/8zQlBH.png)

此时再执行指令`git checkout HEAD .`，即可看到文件确实回退到了上次commit时的状态。

### 拉取仓库与合并冲突

由于我们在GitHub上的仓库是公开并与他人协同编辑的，由此可能会在开发进度同步上带来很多麻烦。例如，如果有他人修改了GitHub上远程仓库中的文件，而你的工作区中却没有同步修改，这会使得代码上传与合并遇到困难。Git提供了一个指令：

```shell
git pull
```

可以拉取远端仓库当中的内容并添加到本地仓库以及工作区当中。一般会建议多人协同开发时多多使用`pull`命令以及时发现冲突并尝试解决，如果冲突积累过多确实会给开发工作带来非常大的困难。

在拉取仓库过程中最大的麻烦来自于许多人修改了同一文件的状况。一般而言，在文件中新增加的内容可以由Git进行自动合并，但是如果多人修改了文件当中的同一段内容，这就只能由人工手动解决冲突了。接下来我们通过修改GitHub仓库中文件模拟这种状况（开发工作当中千万别干）。回到GitHub仓库当中，找到按钮直接修改远程文件

![编辑README](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/编辑README.png)

![Hello Conflict!](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/F0ibdc.png)

我们对文件内容进行修改之后，可以在底部直接commit确认变更。这时再回到工作区，对工作区中文件进行不同的更改，再进行add, commit两项操作，例如这样：

![No! Conflict!](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/nrZLNI.png)

接着使用`git pull`，会得到如下提示：

![提示](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/fyCJZH.png)

因为本地和云端两块的commit不同，因此代码出现了两个不同的分支。此时需要指定两个分支的调和方式，我们输入：

```shell
git merge
```

尝试合并冲突，但是因为本地和远端两个commit修改了同一行内容，Git不能分辨应该保留哪一个，因而无法进行自动合并。此时命令行中会显示

![merge提示信息](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/W0oHm2.png)

告知自动合并失败以及冲突位置。此时我们可以通过`git diff`指令在命令行中显示出冲突位置以及内容，效果如下：

![git diff](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/a4r4MC.png)

此时再回到`README.md`当中，我们会发现其中内容也已有了变动，与`git diff`一样指出了两边的冲突。

![README.md中的变动](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/wifcZI.png)

其中，HEAD指向的是当前的分支，另一边指向的是希望合并的分支。此后，我们需要在冲突文件中手动进行修改，保留我们希望得到的内容。例如我们可以修改成这样（因为不想保留「冲突」）：

![修正冲突](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/BG2BXu.png)

使用指令`git add README.md`将修改后的冲突文件添加到暂存区，然后再使用

```shell
git merge --continue
```

继续未完成的合并工作。因为我们已解决所有冲突，此次合并将能够顺利完成，并会输出合并信息。此时通常而言终端会进入vim编辑器当中，可以先后输入`: + x + Enter`，英文冒号+字母x+回车来退出编辑器。

至此，Git的冲突处理已经完成，可以继续在工作区中进行开发与测试工作了。后续的commit以及push等操作也都可以正常进行。

#### 冲突应该尽量避免！

尽管我们在冲突发生时有办法解决，但是实际工作中我们还是应该尽量避免代码冲突的发生。如果在一次合并当中出现过多冲突，手动修正起来会相当困难。而且因为开发者手上的代码存在过大差异，也很可能会由此引发大量的错误。

好在我们工作流当中的代码冲突是可以较好地规避的。其中最重要的一个方案就是明确分工。在开发工作当中分别明确每个人的工作范围，独立开发而避免交叉。通过将每一个功能的开发工作绝缘开来，将能够避免绝大多数情况下的冲突问题。除此以外，工作工程中多多commit多多pull，保持自己的代码处在最新的版本并且在遭遇冲突时及时解决并测试效果，这些都是很有效的预防措施。

### Fork与Pull Request

截至目前为止，我们所使用的Git都没有涉及到多个远程仓库协同开发的情形。这其实并不困难。我们在前文中已经大体介绍过Fork和Pull Request的操作方法，在这里我们可以来进行一些实际操作，就以对本文档的贡献为例。首先我们进入到项目的主仓库当中去，点击Fork，这会在自己名下复制一个相同的仓库。

此时我们进入到自己名下的仓库当中，可以看到其中显示这是原仓库的一个Fork。我们通过Code选项当中复制链接，并在预定的工作区中使用指令

```shell
git clone 复制来的链接
```

这就可以把自己名下的原仓库的Fork复制到本地，并直接与这一个Fork来的仓库建立联系。此后所进行的所有pull/push等操作都会围绕着这个Fork而来的仓库进行，而不会影响到原仓库。我们回到这个仓库的操作栏：

![PR示例](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/2fVcUs.png)

右侧两选项中，Contribute用于向原仓库发起PR，申请合并进入主仓库。后一项Sync fork则是用于同步fork仓库，保持fork仓库与主仓库进度同步。这里Sync fork的过程中也有可能发生代码冲突，还需要手动解决。在发起PR之前，永远记得要首先进行Sync fork，并解决可能出现的冲突的问题。

接着我们点选Contribute，进而进入到Open a pull request页面当中：

![Pull Request](https://cdn.jsdelivr.net/gh/Yellow-GGG/Pics@main/yhSWVp.png)

这里我们需要给自己的Pull Request取定一个标题，并对其中内容进行描述（支持markdown语法）。确认无误之后即可发起PR，待审核通过之后即可将自己的贡献纳入主仓库当中。

至此，我们已经基本了解了Git/GitHub工作流的基本状况。对于通常小型项目的开发工作而言，只掌握上述操作已经基本足够。当然除此以外，Git也提供了更多其他功能来帮助我们工作的进行，例如通过Git建立更多独立开发的分支（这也是Git最初的版本管理功能）、使用git remote操作远端仓库，而不需要每次都登陆到GitHub当中等等。

## 更多基本使用技巧

（未完待续）

### 分支切换

### Git Remote

### 其他常见指令