# Apple Store 预约助手

## 支持 iPhone 17 系列

![UI](screenshot.png)

## 重要提示
* *这不是外挂，不能全自动一劳永逸*
* *提前登录*
* *提前将需要购买的型号加入购物车，检测有货会打开购物车页面，需要在购物车页面手动选择门店*

## 关于开发
* 代码不优雅, 注释不完善, review须谨慎
* GUI框架 [fyne](https://github.com/fyne-io/fyne)

### 运行
```shell script
go run main.go
```

### 打包

#### 快速打包（推荐）
```shell script
# 使用打包脚本（自动处理签名问题）
./build.sh
```

#### 手动打包
```shell script
# Mac OS 环境下打包
go install fyne.io/fyne/v2/cmd/fyne 
go install github.com/fyne-io/fyne-cross

# 基础打包命令
fyne-cross darwin -arch=amd64,arm64 -app-id=apple.store.helper -name="Apple Store Helper"
fyne-cross windows -arch=amd64,386 -app-id=apple.store.helper -name="Apple Store Helper"

# macOS ARM64 版本需要额外处理签名
xattr -cr "fyne-cross/dist/darwin-arm64/Apple Store Helper.app"
codesign --force --deep --sign - "fyne-cross/dist/darwin-arm64/Apple Store Helper.app"
```

如果提示 `fyne-cross: command not found`，请配置 GO 环境变量
添加以下内容到 `~/.zshrc` 或 `~/.bashrc` 中
```shell script
# GOLANG
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
GOROOT 为 GO 安装目录，根据实际安装位置修改

### 使用 GitHub Actions 自动打包

如果你不熟悉命令行，也可以直接使用项目仓库自带的 GitHub Actions 自动生成 Windows 和 macOS 的安装包。以下步骤以一个完全没有经验的新手为例，尽可能详细地说明每一步要做什么。

1. **准备仓库**
   1. 打开项目在 GitHub 上的页面，点击右上角的 `Fork` 将项目复制到自己的账号下。
   2. 在自己的仓库页面中，点击绿色的 `Code` 按钮，选择 `Local`→`HTTPS` 并复制仓库地址（后面如果要在本地提交修改会用到）。如果只是打包，留在网页上即可。

2. **启用 Actions 功能**
   1. 在自己的仓库页面顶部，点击 `Actions` 标签。如果是第一次使用，会看到提示“Workflows aren’t being run on this forked repository”。
   2. 点击页面上的 `I understand my workflows, go ahead and enable them` 按钮。完成后，左侧会出现一个名为 **Build Desktop Packages** 的工作流。

3. **手动触发打包流程**
   1. 在左侧列表中点击 **Build Desktop Packages**，右侧会显示该工作流的详情页面。
   2. 点击页面中间偏右的灰色按钮 `Run workflow`。会弹出一个下拉框。
   3. `Use workflow from` 默认是 `main` 分支。如果想打包其它分支或标签，可以在 `ref` 输入框中输入对应的名字；如果不知道，就保持为空。
   4. 再次点击绿色的 `Run workflow` 按钮，GitHub 就会开始执行打包任务。

4. **等待并下载构建产物**
   1. 等待几分钟，刷新页面即可看到运行中的任务。需要注意 Windows 和 macOS 会分别运行两个作业，请等待它们都显示绿色的对勾。
   2. 当作业完成后，点击页面上方的最新一次运行记录（通常会显示一个绿色的对勾）。
   3. 在新页面的最底部可以看到 `Artifacts` 区域，其中包含 `windows-packages` 和 `macos-packages` 两个压缩包。
   4. 点击对应的名字即可下载压缩包。下载后解压，就能在 `fyne-cross/dist` 子目录中找到 `.exe`（Windows）和 `.app`（macOS）应用。

5. **再次运行或清理历史记录（可选）**
   * 如果要重新打包，只需再次点击 `Run workflow` 按钮即可。
   * 不需要的历史构建可以在 `Actions` 页面中点击具体运行记录右上角的 `⋯`，选择 `Delete workflow run` 来删除。

> 提示：自动打包会使用 GitHub 免费提供的计算资源。公共仓库默认即可使用；私有仓库会消耗每月的 Actions 分钟数，请注意账户额度。

## 使用方法

1. 前往 [release](https://github.com/hteen/apple-store-helper/releases) 页面下载对应系统的程序，启动 
2. 在 Apple 官网将需要购买的型号加入购物车
3. 选择地区、门店和型号，点击`添加`按钮，将需要监听的型号添加到监听列表
4. 点击`开始`按钮开始监听，检测到有货时会自动打开购物车页面
5. 匹配到有货后会自动暂停监听，直到再次点击 `开始`

### 有货时推送通知到 iOS 设备
1. App Store 下载并安装 App 「Bark」，并允许「Bark」进行推送
2. 打开「Bark」，复制应用中代表你自己设备的地址（格式如` https://api.day.app/xxxxxxxxx `），粘贴至本应用的`Bark 通知地址`栏
3. 点击 `测试 Bark 通知`，确认应用能够通知到你的 iOS 设备
4. 更多内容请参考 `https://bark.day.app/`

## Contributors
- [@Hteen](https://github.com/hteen)
- [@Timssse](https://github.com/Timssse)
- [@Black-Hole](https://github.com/BlackHole1)
- [@RayJason](https://github.com/RayJason)
- [@Warkeeper](https://github.com/Warkeeper)

