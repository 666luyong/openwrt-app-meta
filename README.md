# OpenWRT应用程序元数据

## 目录结构

* `applications`
    * `app-meta-*`
        * `Makefile` - 描述文件，参考下文约定
        * `logo.png` - 图标文件，目前只支持png
* `fake-top` - 辅助文件，无需关注
* `Makefile` - 辅助文件，无需关注
* `meta.mk` - 辅助文件，无需关注

## Makefile书写约定

1. `META_TITLE:=` 使用官方名称，如果有中文就用中文
2. `META_DESCRIPTION:=` 中文描述
3. `META_AUTHOR:=` 是作者名称，如果是非个人作者则填组织或者公司名称
4. `META_DEPENDS:=` 的第一个应该是能代表这个APP的主包，方便作为版本更新的依据，不要将其他插件可能依赖的包加到这里，以免卸载时将其他插件也卸载了，如果需要添加额外的包，参考下面的[#依赖额外的包](#依赖额外的包)
5. `META_ARCH:=` 用来填写支持的处理器架构，如果是架构无关的应用则不需要这一行
6. `PKG_VERSION:=` 尽量跟主包保持一致
7. `META_WEBSITE:=` 官网链接（注意如果链接包含#，需要转义成\\#）
8. `META_TUTORIAL:=` 教程链接（注意如果链接包含#，需要转义成\\#）

### 国际化
1. `META_TITLE.en:=` 英文名称，如果跟`META_TITLE`一样就不用填
2. `META_DESCRIPTION.en:=` 英文描述

## 依赖额外的包
1. 编辑`dummy/Makefile`，添加一行`$(eval $(call DummyPackage,插件名-deps,+额外依赖,,0.00))`，例如`serverchan`添加额外的`+iputils-arping +curl +jq`依赖：
    ```Makefile
    $(eval $(call DummyPackage,serverchan-deps,+iputils-arping +curl +jq,,0.00))
    ```
2. 在之前的`Makefile`的`META_DEPENDS:=`位置添加`+插件名-deps`，例如`serverchan`：
    ```Makefile
    META_DEPENDS:=+luci-app-serverchan +serverchan-deps
    ```

# 在线提交流程

**以下操作都在github上进行**

## 准备工作

1. Fork 本项目，如果之前Fork过，那先删掉自己的项目重新Fork

## 修改元数据

1. 切换到 `main` 分支
2. 同步上游仓库：点击 `Fetch upstream`，再点击 `Fetch and merge` 即可，这一步如有疑问请参考[官方文档](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/syncing-a-fork#syncing-a-fork-from-the-web-ui) 
3. 创建PR用的临时分支：切换到 `main` 分支，点击分支下拉菜单，在输入框输入新分支的名称（也就是不存在的分支名称），例如 `add_app_jellyfin` ，搜索结果会变成 `Create branch: *** from 'main'` ，点这个搜索结果，稍等会自动创建并切换到新分支
4. 在新分支中进行更改，完成以后提交PR
5. 等PR合并以后，可以在 `branches` 页面删除临时分支，也可以保留临时分支，但是不要再进行变更和PR
