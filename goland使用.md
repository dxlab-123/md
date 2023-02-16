# goland使用

***go get 找不到 google.golang.org/protobuf解决办法***

```go
// 找到你的 GOPATH/src 目录，新建 google.golang.org 文件夹
// 在 google.golang.org 目录下执行
git clone https://e.coding.net/robinqiwei/googleprotobuf.git protobuf
```

***go mod***

```go
// 运行
go mod init xxx
go run xxx.go
//添加需要用到但go.mod中查不到的模块,删除未使用的模块
go mod tidy
```

***go build***

生成可执行文件

*创建go build配置文件*
Run kind 选Directory
Directory 选你的main包所在文件夹
Output directory设置与go build -o 不相容，所以不用设置，我们使用-o参数来控制可执行文件的路径以及名字
Working directory保持默认就好
Go tool arguments 就是go build 的参数

```go
 go build -o E:\go_env\src\A-project\es-log\es-log.exe
```

