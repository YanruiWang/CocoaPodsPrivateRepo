# iOS CocoaPods 私有库使用方法
## 相关概念
1. `pod spec`: 相关`pod`项目的索引`repo`,比如官方的 https://github.com/CocoaPods/Specs.git
2. `podspec` 文件: 用于说明某个`pod`版本的详尽信息，比如git地址，版本，iOS版本信息，文件目录等

## 私有库创建前期准备
1. 安装`CocoaPods`
2. 建立一个放置私有库索引的库(测试就先放在本地)
3. 建立一个放置功能代码的库

## 添加私有 `Pod Spec` 仓库
### 添加仓库
打开终端, 使用`pod repo add spec_name spec_url`命令, 将私有库索引加入  `spec` 列表中, 比如 `pod repo add HHCPrivate http://github.com/xxx/xxx.git`
### 检查是否添加成功:
```shell
cd ~/.cocoapods/repos/XXX
pod repo lint
```

## 编辑私有组件的 `tag`，`podspec` 文件
### 文件形式:
```ruby
Pod::Spec.new do |s|
	s.name         = 'name'
	s.version      = '0.4.1'
	s.summary      = 'summary'
	s.homepage     = 'http://github.com/xxx/xxx.git'
	s.authors      = { 'auther' => 'xxx@example.com'}
	s.license      = { :type => "MIT", :file => "LICENSE" }
	s.platform     = :ios, '9.0'
	s.ios.deployment_target = '9.0'
	s.source       = { :git => 'http://github.com/xxx/xxx.git', :tag => '0.4.1'}
	s.source_files = 'HHCQRCode/lib/*.{swift}','HHCQRCode/lib/Media.xcassets/*.png'
	s.requires_arc = true
end
```

1. 将其放在工程项目的同级目录下，上传`podspec`文件，标记本次`tag`(注意，每次修改提交代码都要修改`tag`，写明修改原因，更改`podspec`文件)
2. 在私有组件的根目录下执行 `pod lib lint`查看`podspec`文件是否正确
3. 执行`pod repo push repo_name spec_name.podspec`将组件信息上传到索引仓库, 比如`pod repo push HHCPrivate /Users/wangyanrui/Developer/HHCPrivate/HHCQRCode/HHCQRCode.podspec`
4. 执行`pod search pod 组件名称` 来检查是否加入到索引中

## 引用私有组件
### 打开需要引用私有组件工程的Podfile文件，在文件最上面加入:
```ruby
source 'https://github.com/CocoaPods/Specs.git' # 官方库
source 'http://github.com/xxx/xxx.git'
```
- 在私有组件的Podfile中加入`pod xxx`引用私有组件，与官方库引用方式一致
- 执行`pod install`引用私有组件

## 相关资料
- https://guides.cocoapods.org/making/private-cocoapods.html
- http://blog.wtlucky.com/blog/2015/02/26/create-private-podspec/

