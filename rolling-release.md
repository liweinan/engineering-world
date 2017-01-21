# 『阿男的工程世界』＊什么是Rolling Release的发行方式？＊

什么是Rolling Release？Arch Linux这个Linux发行版本就是一个典型的Rolling Release。

所谓Rolling Release，就是本身没有具体明确的version，而是整个系统不停地向前更新。我们知道一般软件都有自己的明确的每一个version，比如Windows 2000，XP，Windows 10，等等。然后Linux的各个发行版也有自己明确的version，比如Fedora 25，Ubuntu 16.04 LTS，等等。

但是Rolling Release就要求系统不断地更新，没有明确版本，Arch Linux就是这样，它里面的各个component总是在不停地更新。

## 这样做有什么好处？

首先我们总能获得最新的软件，因为没有明确的发行版本，因此整个系统的每一个comopnent都在随时更新。

然后我们不用考虑从一个版本升级到下一个版本，我们的系统本身就没有版本一说，所以系统里面每一个组件都是随时upgrade，没有什么整体升级到下一个版本的说法。

## 那么Rolling Release的坏处呢？

首先就是不可能做集成测试的。既然没有了具体的版本，那么也没法测试这个版本下的系统所有组件之间的互相兼容性。很可能我们升级了一个component，导致另外一个component就不可用了。

当然component之间的依赖性可以一定程度上来解决这个问题，但当我们把系统作为一个整体来看待时，很多需要从集成角度考虑的东西是做不了的

比如我们想给这个版本的系统做一些特定的功能定制和悬泉，因为没有所谓的系统版本，所以也就不可能做了。

简单来讲，Rolling Release一般用在比较前沿的系统里面。

比如Arch Linux，可以说对于Linux初学者是非常不友好的，安装方式及其原始，配置工具及其简陋。

## 阿男是Fedora社区的commiter，所以使用Fedora Rawhide。所谓Fedora Rawhide，就是Fedora的rolling release，里面就是所有Fedora最新的包。一般Fedora主要版本的发布，都是从Rawhide里面cut出来一个snapshot，在此基础上作集成和发布的工作，形成一个新版本。

Arch Linux就更激进，根本没有版本，只提供一个rolling release。

既然没有集成，也就没什么稳定性可言，当然Arch Linux和Fedora Rawhide其实都挺稳定的，但是不可能在生产环境，原因有这些：

首先生产环境里面不可能只考虑所有的软件都是最新的，往往生产环境要求某个软件是某个特定版本，不能老变，这是为了稳定性。

还有就是生产环境对软件安全性有高要求，可能最新的版本里面有安全漏洞，所以必须要求某些软件稳定在特定的version。

Rolling release不考虑这些，就是一股脑更新各个component，不断把最新的软件放进来。

所以这种rolling release既不适合初学者，也不适合生产环境。适合什么群体？

首先肯定适合发布系统的人，阿男是Fedora Commiter，必然要安装Fedora Rawhide来工作，因为自己就是在把Fedora里面的软件更新到最新的成员之一。

还有就是系统相关的开发人员，可能会需要自己的系统永远在最bleeding edge，因为就是在做这些开发工作，所以肯定要使用rolling release。
