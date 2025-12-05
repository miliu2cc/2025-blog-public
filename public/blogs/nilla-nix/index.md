# 解决方案
使用nilla替代flake
- 使用npins固定版本

使用hjem替代home-manager
# 探索
## 原版
原配置为snowfall+home-manager, 每次更改hm中的任何配置，都要对整个hm进行评估，导致速度非常慢
## 第二版
使用npins+原生nix+hjem,速度非常之快，hjem使用软链接的方式将.config下的内容转移到你的nix配置路径下，但有一点小问题，使用了原生nix对于github上使用flake分发的包不太友好，需要将派生文件写入配置中，一点点麻烦
## 当前状态
### nilla
nilla 基于aux lib开发，auxolotl是一个社区维护的nix生态系统的替代方案，我一直以为这个项目进展缓慢（但好像还真是）但还是做出nilla了

**features**
![](/blogs/nilla-nix/8477effdfcf6b7fc.png)
虽说感觉有一部分硬凑的嫌疑，但简单比较下来还算可以
- 使用npins进行版本固定
- 相比传统nix,nilla对flake和nixos模块的支持更好
- 支持使用colmena,nilla-utils等工具（详见[config](https://github.com/jakehamilton/config)，[nilla-utils](https://github.com/arnarg/nilla-utils))
### hjem
使用软链接的方式管理$HOME的模块系统，使用其自制的[smfh](https://github.com/feel-co/smfh)实现软链接，主要由rust编写，另一个类似的工具nix-maid则使用python编写的连接器，至于两者有什么区别，我没感觉到...
# 特点
nilla-utils提供了一个比nilla cli更好的命令行，但其对配置格式进行了一些要求，虽说显得更整齐但丢失了一定的自由度，colmena可能更适合多设备配置。

nilla提供了多种加载器，包括：nilla,nixpkgs,flake,legacy,raw。nilla通常会自动设置加载器，但也可以手动配置，对于在flake下需要，`inputs.follow = "nixpkgs"`的输入，可能需要重新以flake模式加载nixpkgs,再单独设置，写法被分离。

虽说nilla的文档中写了可以创建本地包，但我看到的使用nilla的配置文件，在打包时还是在使用flake模式。
shell好像也是这样，我还没适应吧...