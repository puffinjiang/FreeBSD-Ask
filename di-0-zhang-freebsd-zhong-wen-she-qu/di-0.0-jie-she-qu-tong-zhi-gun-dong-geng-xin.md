# 第 0.0 节 社区通知（滚动更新）

> **该页面滚动更新。请随时关注。**

## 2023-07-16

### freebsd pkg 1.20 使用国内源会有错误

目前 pkg 源里使用的仍然是 pkg 1.19，这个版本是没有任何问题的，但是​一旦你使用了 ports 编译安装 pkg 就会出现问题：

```
[root@ykla /usr/ports/ports-mgmt/pkg]# pkg info pkg
pkg-1.20.4
Name           : pkg
Version        : 1.20.4
Installed on   : Sun Jul 16 16:02:26 2023 CST
Origin         : ports-mgmt/pkg
Architecture   : FreeBSD:13:amd64
Prefix         : /usr/local
Categories     : ports-mgmt
Licenses       : BSD2CLAUSE
Maintainer     : pkg@FreeBSD.org
WWW            : https://github.com/freebsd/pkg
Comment        : Package manager
Options        :
	DOCS           : on
Shared Libs provided:
	libpkg.so.4
Annotations    :
	FreeBSD_version: 1302001
Flat size      : 35.4MiB
Description    :
Package management tool

WWW: https://github.com/freebsd/pkg

[root@ykla /usr/ports/ports-mgmt/pkg]# 
```


```
[root@ykla /usr/ports/ports-mgmt/pkg]# pkg update -f
Updating FreeBSD repository catalogue...
pkg: No SRV record found for the repo 'FreeBSD'
Fetching meta.conf: 100%    163 B   0.2kB/s    00:01    
pkg: packagesite URL error for pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/FreeBSD:13:amd64/latest/packagesite.pkg -- pkg+:// implies SRV mirror type
pkg: packagesite URL error for pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/FreeBSD:13:amd64/latest/packagesite.txz -- pkg+:// implies SRV mirror type
Unable to update repository FreeBSD
Error updating repositories!
[root@ykla /usr/ports/ports-mgmt/pkg]# 
```

目前的临时解决办法（删去 `pkg+`）：

创建 `/usr/local/etc/pkg/repos/FreeBSD.conf`

```
# mkdir -p /usr/local/etc/pkg/repos/
# touch /usr/local/etc/pkg/repos/FreeBSD.conf
```

写入：


```
FreeBSD: { enabled: no }

USTC: {
  url: "http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
}
```

**quarterly** 可以替换为 **latest** 以使用滚动分支。

```
[root@ykla /usr/ports/ports-mgmt/pkg]# pkg update -f
Updating USTC repository catalogue...
Fetching meta.conf: 100%    163 B   0.2kB/s    00:01    
Fetching packagesite.pkg: 100%    7 MiB   7.0MB/s    00:01    
Processing entries: 100%
USTC repository update completed. 33793 packages processed.
All repositories are up to date.
[root@ykla /usr/ports/ports-mgmt/pkg]# 
```

问题临时解决。

## 2023-06-27

### 1、freebsd 14 发布周期更新

KBI 冻结： 2023 年 8 月 4 日

[......ALPHA 构建......]： 待定

stable/14 分支： 2023 年 8 月 18 日

[...ALPHA 构建...]： 待定

releng/14.0 分支：2023 年 9 月 8 日

BETA 1 构建开始：2023 年 9 月 8 日

doc/ tree slush： 2023 年 9 月 10 日

BETA2 构建开始： 2023 年 9 月 15 日

BETA3 构建开始： 2023 年 9 月 22 日 [*]

doc/： 2023 年 9 月 24 日

RC1 构建开始： 2023 年 9 月 29 日，

ports 季度分支： 2023 年 10 月 1 日

RC2 构建开始： 2023 年 10 月 6 日

RC3 构建开始： 2023 年 10 月 13 日 [*]

RELEASE 版本开始： 2023 年 10 月 20 日

RELEASE 公告： 2023 年 10 月 23 日

生命周期结束： 2028 年 10 月 31 日

### 2、OpenSSL 3.0 合并入主分支

来自邮件列表的消息，在昨天晚上 openssl 3.0 已经合并至主分支。​ 制约 freebsd 14.0 更新的一大阻碍已经清除。

## 2023.6.21

### 1、通知：14.0-RELEASE 发布周期延迟及原因

根据 14.0-RELEASE 的计划，主要代码的冻结和 KBI（内核二进制接口）的冻结原计划于 2023 年 4 月 25 日进行。正如你们中的一些人可能已经注意到的那样，这并没有发生。

首先，对于 14.0-RELEASE 来说，最重要的是 OpenSSL 升级到 3.0 版本的进展情况。这本身就足以延迟计划，直到取得一些实质性的进展为止。是的，有些人表达了在这个领域提供帮助的兴趣，然而目前来说，这是主要的阻碍因素。

其次，与最近从 OpenZFS 进行的上游合并相关的分支状态。虽然默认情况下禁用了 block_cloning【1】，但还发现了其他的回归问题（并进行了修复），但总体来说，我认为我们对于那些我们不了解的回归问题还没有一个明确的了解。

考虑到上述因素，我们无法按时完成稳定分支 stable/14 的分支点，原计划是 2023 年 5 月 12 日。这并不是一个包含所有原因的说明通知，但是是目前所我们关注的一些重要事项。

14.0-RELEASE 最新的发布计划将在不久的将来发布，但目前还没有确定的计划。

感谢你们的耐心等待，并感谢你们在任何解决这些未完成事项方面提供的帮助。

Glen
工程团队

【1】block_cloning 是一种在计算机系统中的功能或特性。它指的是在文件系统或存储系统中，通过创建文件或数据的快照（snapshot）而不实际复制数据块。这种技术可以在不占用额外存储空间的情况下，创建副本或备份数据，并在需要恢复时快速还原到之前的状态。block_cloning 常用于数据管理、备份和恢复等场景，可以提高效率并减少存储资源的使用。
