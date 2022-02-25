1. brew 安装脚本(自动选择软件源)
```sh
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```
2. brew 卸载脚本
```sh
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```
3. 常用的命令
```sh
	* 安装软件：brew install xxxx
	* 卸载软件：brew uninstall xxxx
	* 搜索软件：brew search xxxxx
	* 更新软件：brew upgrade xxx
	* 查看列表：brew list
	* 更新brew: brew update
	* 清理所有包的旧版本：brew cleanup
	* 清理指定的旧版本：brew cleanup $FORMULA
	* 查看可清理的旧版本包，不执行实际操作：brew cleanup -n
```