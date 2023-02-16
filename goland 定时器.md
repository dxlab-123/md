# goland 定时器

平时开发过程中，我们经常会遇到在将来某个时间点或者间隔一段时间重复执行函数。这个时候我们就可以考虑使用定时器，这篇文章将会与大家一起，总结下 Go 语言里 time.Timer、time.Ticker、time.After 和 time.AfterFunc 的相关用法及需要注意的地方。

## Timer 定时器

### 00

Go 语言除了可以使用 time.Sleep() 实现延时之外，Timer 也可以实现，就像下面这样：

func main() {
    // fmt.Println("公众号：Golang来啦")
    fmt.Println("now time: ",time.Now().Format("2006-01-02 15:04:05"))
    timer := time.NewTimer(3 * time.Second)
    <-timer.C
    fmt.Println("now time: ",time.Now().Format("2006-01-02 15:04:05"))
}

```go
func main() {
    // fmt.Println("公众号：Golang来啦")
    fmt.Println("now time: ",time.Now().Format("2006-01-02 15:04:05"))
    timer := time.NewTimer(3 * time.Second)
    <-timer.C
    fmt.Println("now time: ",time.Now().Format("2006-01-02 15:04:05"))
}
```

输出：

```go
now time:  2021-11-15 13:40:51
now time:  2021-11-15 13:40:55
```

### 01

我们需要深究下，实现延时的原理是什么？

从源码可以看到，timer.NewTimer() 返回的是 Timer 类型：

```go
type Timer struct {
   C <-chan Time
   r runtimeTimer
}
```

传参是告诉 Timer 需要等待多长时间，Timer 是带有一个缓冲的 channal，在定时时间到达之前，没有数据写入 Timer.C，读取操作会阻塞当前协程，到达定时时间时，会向 channel 写入数据（当前时间），阻塞解除，被阻塞的协程得以恢复运行，达到延时或者定时执行的目的。


```go
func main() {
    // fmt.Println("公众号：Golang来啦")
    fmt.Println("now time: ", time.Now().Format("2006-01-02 15:04:05"))
    timer1 := time.NewTimer(2 * time.Second)
    go func() {
        curTime := <-timer1.C           // 超时时间还没到时，协程会阻塞；超时时间到了之后会返回当前时间
        // 定时时间到了之后执行打印操作，实际开发时可以实现具体的业务逻辑
        fmt.Println("Timer 1 fired, now time: ", curTime.Format("2006-01-02 15:04:05"))
    }()


    timer2 := time.NewTimer(2 * time.Second)
    go func() {
        <-timer2.C
        fmt.Println("Timer 2 fired")
    }()
    // 取消定时器
    stop2 := timer2.Stop()
    if stop2 {
        fmt.Println("Timer 2 stop")
    }
    time.Sleep(5 * time.Second)
}
```
上面的代码，timer1 还没过期之前，协程会阻塞，超时时间到了之后会返回当前时间。timer2 过期时间是 2s，还没过期之前调用 time.Stop() 取消了定时器，所以 Timer 2 fired 是不会打印的。执行结果如下：

```go
now time:  2021-01-23 14:43:15
Timer 2 stop
Timer 1 fired, now time:  2021-01-23 14:43:17
```

### 02

Timer 定时器表示在一段时间后执行，默认情况下只会执行一次；如果想再次执行的话需要调用 timer.Reset() 方法。

```go
func main() {
    // fmt.Println("公众号：Golang来啦")
    fmt.Println("now time: ", time.Now().Format("2006-01-02 15:04:05"))
    timer1 := time.NewTimer(5 * time.Second)
    // 重新设置为 2s
    ok := timer1.Reset(2 * time.Second)
    fmt.Println("ok: ", ok)
    curTime := <-timer1.C
    fmt.Println("now time: ", curTime.Format("2006-01-02 15:04:05"))
}
```

上面的代码，调用 timer.Reset() 将过期时间重新设置为 2s，输出如下：

```go
now time:  2021-01-23 14:56:27
ok:  true
now time:  2021-01-23 14:56:29
```

还可以设置执行次数或者无限次调用：

```go
func main() {
    // fmt.Println("公众号：Golang来啦")
    timer1 := time.NewTimer(5 * time.Second)
    fmt.Println("开始时间: ", time.Now().Format("2006-01-02 15:04:05"))

    go func() {
        count := 0
        for {
            <-timer1.C
            fmt.Println("timer", time.Now().Format("2006-01-02 15:04:05"))

            count++
            fmt.Println("调用 Reset() 重新设置过期时间，将时间修改为 2s")
            timer1.Reset(2*time.Second)
            if count > 2 {
                fmt.Println("调用 Stop() 停止定时器")
                timer1.Stop()
            }
        }
    }()
    time.Sleep(15 * time.Second)
    fmt.Println("结束时间：", time.Now().Format("2006-01-02 15:04:05"))
}
```
第一次过期时间是 5s，过期之后调用 time.Reset() 函数重置成 2s，重置了 3 次，最后调用 time.Stop() 取消了定时器，输出如下：

```go
开始时间:  2021-01-23 15:08:59
timer 2021-01-23 15:09:04
调用 Reset() 重新设置过期时间，将时间修改为 2s
timer 2021-01-23 15:09:06
调用 Reset() 重新设置过期时间，将时间修改为 2s
timer 2021-01-23 15:09:08
调用 Reset() 重新设置过期时间，将时间修改为 2s
调用 Stop() 停止定时器
结束时间： 2021-01-23 15:09:14
```

至于如何实现无限次定时调用，对你来说应该不难，可以结合上述代码思考下再动手实践下。

### 03

需要注意一点，如果调用 time.Reset() 和 time.Stop() 时，timer 已过期或者已停止，则会返回 false。

```go
func main() {
    // fmt.Println("公众号：Golang来啦")

    // timer 过期
    timer := time.NewTimer(1 * time.Second)
    time.Sleep(2 * time.Second)
    ret := timer.Reset(2 * time.Second)
    fmt.Println(ret)

    // timer 停止
    timer = time.NewTimer(1 * time.Second)
    timer.Stop()
    ret = timer.Reset(1 * time.Second)
    fmt.Println(ret)
}

```
输出如下：

```go
false
false
ok
```

## Ticker 定时器

### 00

Ticker 定时器原理与 Timer 类似，Ticker 是一个周期触发定时的定时器，按给定时间间隔往 channel 发送系统当前时间，而 channel 的接收者可以以固定的时间间隔从 channel 中读取。

```go
type Ticker struct {
   C <-chan Time    // The channel on which the ticks are delivered.
   r runtimeTimer
}
```

相对于 Timer，Ticker 没有 Reset() 方法，但是也可以实现定时几次或者无限次数定时。

```go
func main() {
    // fmt.Println("公众号：Golang来啦")
    fmt.Println(time.Now())
    ticker1 := time.NewTicker(2*time.Second)
    for {
        curTime := <-ticker1.C
        fmt.Println(curTime)
    }
}
```

上面的代码，创建定时器，每隔 2 秒后，定时器就会给 channel 发送一个事件，即当前时间。上面代码会循环执行，可以看到间隔时间是相同的：

```go
2021-01-23 15:58:44.622531 +0800 CST m=+0.000158164
2021-01-23 15:58:46.627912 +0800 CST m=+2.005479594
2021-01-23 15:58:48.623097 +0800 CST m=+4.000604383
2021-01-23 15:58:50.623892 +0800 CST m=+6.001339741
...
```

可以结合 select 使用：

func main() {
    // fmt.Println("公众号：Golang来啦")

```go
func main() {
    // fmt.Println("公众号：Golang来啦")

    ticker := time.NewTicker(2 * time.Second)
    done := make(chan bool)

    go func() {
        for {
            select {
            case <-done:
                return
            case t := <-ticker.C:
                fmt.Println("Tick at", t)
            }
        }
    }()

    time.Sleep(10 * time.Second)
    ticker.Stop()   // 取消定时器
    done <- true
    fmt.Println("Ticker stopped")
}

```
上面的代码输出：

```go
Tick at 2021-01-23 16:05:03.827385 +0800 CST m=+2.002393562
Tick at 2021-01-23 16:05:05.8287 +0800 CST m=+4.003648404
Tick at 2021-01-23 16:05:07.828627 +0800 CST m=+6.003515779
Tick at 2021-01-23 16:05:09.82684 +0800 CST m=+8.001669110
Tick at 2021-01-23 16:05:11.826863 +0800 CST m=+10.001632091
Ticker stopped
```

与 Timer 定时器类似，退出计数时需要调用 Stop() 方法清除定时器，释放相关资源。如果循环执行不需要清理的话可以使用更简便的 Time.Tick() 方法：

```go
func Tick(d Duration) <-chan Time {
    if d <= 0 {
        return nil
    }
    return NewTicker(d).C
}
```

可以看出其实它封装了 Ticker。

## time.After()

### 00

函数定义：

```go
func After(d Duration) <-chan Time {
    return NewTimer(d).C
}
```

可以看到，After() 底层是基于 Timer 实现的，该函数返回 time.Time 类型的 channel，当定时时间到了之后，会将数据（当前时间）写入 channel。

调用该函数不会阻塞当前协程，但如果执行读取操作时，还未达到定时时间，会发生阻塞。

```go
func main() {
    // fmt.Println("公众号：Golang来啦")

    fmt.Println(time.Now())
    after1 := time.After(2*time.Second)
    fmt.Println("before, time: ",time.Now())
    curTime1 := <-after1    // 读取
    fmt.Println("after , time: ",time.Now())
    fmt.Println(curTime1)

    fmt.Println()


    fmt.Println(time.Now())
    after2 := time.After(2*time.Second)
    time.Sleep(3*time.Second)         // 为了使定时时间过期
    fmt.Println("before, time: ",time.Now())
    curTime2 := <-after2   // 读取
    fmt.Println("after , time: ",time.Now())
    fmt.Println(curTime2)
}
```

输出：

```go
2021-01-23 17:05:44.35444 +0800 CST m=+0.000092252
before, time:  2021-01-23 17:05:44.354657 +0800 CST m=+0.000309148
after , time:  2021-01-23 17:05:46.357352 +0800 CST m=+2.002944041
2021-01-23 17:05:46.357326 +0800 CST m=+2.002917813

2021-01-23 17:05:46.357436 +0800 CST m=+2.003027509
before, time:  2021-01-23 17:05:49.357899 +0800 CST m=+5.003400796
after , time:  2021-01-23 17:05:49.357984 +0800 CST m=+5.003486046
2021-01-23 17:05:48.358176 +0800 CST m=+4.003707692

```

从输出可以看出，执行读取操作时，如果定时器还未过期，读取操作将会阻塞；否则不会阻塞。

### 01

应用场景，我们可以使用 time.After() 实现超时控制，比如我们调用一个服务的 api，要求超时时间是 1s，可以结合 select 实现。

```go
func main() {
    // fmt.Println("公众号：Golang来啦")

    ch := make(chan string, 1)
    go func() {
        // 假设我们在这里执行一个外部调用，2秒之后将结果写入 ch
        time.Sleep(time.Second * 2)
        ch <- "success"
    }()

    select {
    case res := <-ch:
        fmt.Println(res)
    case <-time.After(time.Second * 1):
        fmt.Println("timeout 1")
    }
}
```
上面的代码，我们在子协程里面模拟执行 api 调用，执行时间是 2s，超时 1s，所以 select 执行了超时的 case，打印 timeout 1；如果我们将超时时间设置成 3s，就可以正常接收 api 的返回数据。

## time.AfterFunc()

### 00

函数定义：

```go
func AfterFunc(d Duration, f func()) *Timer
```

time.AfterFunc() 用于延时执行自定义函数，会另起一个协程并等待指定时间过去，然后调用函数 f。它返回一个 Timer，可以通过调用其 Stop() 方法来取消等待和对 f 的调用。

```go
func main() {
    // fmt.Println("公众号：Golang来啦")

    ch := make(chan int, 1)

    time.AfterFunc(10*time.Second, func() {
        fmt.Println("10 seconds over....")
        ch <- 8
    })

    for {
        select {
        case n := <-ch:
            fmt.Println(n, "is arriving")
            fmt.Println("Done!")
            return
        default:
            time.Sleep(3 * time.Second)
            fmt.Println("time to wait")
        }
    }
}
```
上面的代码，创建了 10s 的定时器和回调函数，执行输出：

```go
time to wait
time to wait
time to wait
10 seconds over....
time to wait
8 is arriving
Done!
```

调用 Stop() 函数取消函数执行：

```go
func main() {
    // fmt.Println("公众号：Golang来啦")

    timer1 :=time.AfterFunc(3*time.Second, func() {
        fmt.Println("10 seconds over....")
    })
    time.Sleep(1*time.Second)

    timer1.Stop()
    fmt.Println("已取消等待和函数执行")
    time.Sleep(5*time.Second)
    fmt.Println("ok")
}
```
输出：

```go
已取消等待和函数执行
ok
```


关于定时器的一些使用方法，先总结这么多，欢迎大家补充更多好玩的用法。

这里有个练习，我们一起来思考下，巩固下学习成果，欢迎大家留言说说自己的看法。

下面这道题目输出什么，为什么？

```go
func main() {
    t := time.AfterFunc(8*time.Second, func() {
        fmt.Println("Golang来啦")
    })
    for {
        select {
        case <-t.C:
            fmt.Println("seekload")
            break
        }
    }
}
```

