# CrashDetail

{
    "app_name":"WeChat",
    "timestamp":"2021-02-18 12:36:54.00 +0800",
    "app_version":"8.0.2",
    "slice_uuid":"13d116da-7a09-339d-98cc-63d8adb4040f",
    "adam_id":414478124,
    "build_version":"8.0.2.35",
    "platform":2,
    "bundleID":"com.tencent.xin",
    "share_with_app_devs":0,
    "is_first_party":0,
    "bug_type":"109",
    "os_version":"iPhone OS 14.4 (18D52)",
    "incident_id":"CB576498-5D1C-4E72-AAF6-C1C8BCBDE1DD",
    "name":"WeChat"
}


//!!!!: Header 部分
Incident Identifier: CB576498-5D1C-4E72-AAF6-C1C8BCBDE1DD //崩溃报告的唯一编号
CrashReporter Key:   9a80d7e230ecbdafa25cf054e8933ccb5ace12a3 //崩溃设备的唯一标识
Hardware Model:      iPhone8,1 //Apple 硬件模型
Process:             WeChat [77952] //崩溃的进程名称
Path:                /private/var/containers/Bundle/Application/87640D0D-A567-4C11-8CDA-72D1757E5418/WeChat.app/WeChat //设备文件系统上崩溃程序的完整路径名称
Identifier:          com.tencent.xin //来自 info.plist 的 Bundle identifier
Version:             8.0.2.35 (8.0.2) //CFBundleVersion 括号中有 CFBundleVersionString
AppStoreTools:       12D4d
AppVariant:          1:iPhone8,1:13
Code Type:           ARM-64 (Native) //崩溃进程的目标体系结构
Role:                Non UI //进程 task_role 如果我们在后台, 前台或控制台应用程序中,都会显示一个指示器, 主要影响进程的调度优先级
Parent Process:      launchd [1] //崩溃进程的父级 launchd 是一个进程启动程序. 通常是父进程
Coalition:           com.tencent.xin [350] // 任务分组合并 然后他们就可以把资源消耗集中起来


//!!!!: Date 和 Version 部分
Date/Time:           2021-02-18 12:36:50.9803 +0800 //崩溃发生的时间
Launch Time:         2021-02-18 12:36:39.6326 +0800 //崩溃前最初启动该进程的时间
OS Version:          iPhone OS 14.4 (18D52) //操作系统版本(内部版本号)
Release Type:        User
Baseband Version:    8.40.01  //蜂窝调制解调器固件版本号(用于电话呼叫) 或 n/a (如果设备没有蜂窝调制解调器)
Report Version:      104 //生成报告的 ReportCrash版本


//!!!!:崩溃报告的异常部分
Exception Type:  EXC_CRASH (SIGKILL) //Mach OS 中的异常类型
Exception Codes: 0x0000000000000000, 0x0000000000000000 //异常类型的编码 例如尝试访问无效的地址以及支持信息
Exception Note:  EXC_CORPSE_NOTIFY //如果进程被看门狗计时器杀死, 会显示 (SIMULATED)(这个不是崩溃) 进程崩溃则显示 EXC_CORPSE_NOTIFY
Termination Reason: Namespace SPRINGBOARD, Code 0x8badf00d //视情况而定, 他给出一个命名空间, (数字或子系统名称) 和一个 magic number (通常是一个看起来像英语单词的十六进制数字)
Termination Description: SPRINGBOARD, <RBSTerminateContext| domain:10 code:0x8BADF00D explanation:scene-create watchdog transgression: application<com.tencent.xin>:77952 exhausted CPU time allowance of 2.32 seconds | ProcessVisibility: Background | ProcessState: Running | WatchdogEvent: scene-create | WatchdogVisibility: Background | WatchdogCPUStatistics: ( | "Elapsed total CPU time (seconds): 8.020 (user 8.020, system 0.000), 100% CPU", | "Elapsed application CPU time (seconds): 2.501, 31% CPU" | ) reportType:CrashLog maxTerminationResistance:Interactive>
Triggered by Thread:  0  //导致崩溃的进程中的线程

//!!!!: Code xxxxx 崩溃原因
Termiantion Code                      Meaning
0xdead10cc                           我们在挂起之前持有的文件或sqlite数据库锁, 我们应该在挂起之前释放锁

0xbaaaaaad                           通过侧面和两个音量按钮对整个系统进行了 stackshot.

0xbad22222                           可能是 VOIP 应用被频繁唤起导致的崩溃, 也可以注意一下我们后台调用网络的代码, 如果我们的TCP连接连续被唤醒多次, (例如:300秒内唤醒15次) 就会导致此崩溃

0x8badf00d                           我们的应用程序执行状态更改(启动, 关闭, 处理系统 消息等) 花费了太长的时间, 与看门狗的时间策略发生冲突(超时)并导致终止,最常见的罪魁祸首是在主线程进行同步网络连接

0xc00010ff                           系统检测的设备发烫而终止了我们的应用程序, 如果只有少量设备上(几个)发生, 那就可能是由于硬件问题, 而不是我们的App问题, 但是如果发生在其他的设备上, 我们应该使用
                                     Instrument去检查我们App的耗电问题

0x2bad45ec                           发生安全冲突, 如果 Termination Description 显示为 Process detected doing insecure drawing while insecure mode 则意味着我们的应用尝试在不允许的
                                     情况下进行绘制, 例如在锁定屏幕的的情况下

//!!!!:崩溃类型
EXception Type                        Meaning
EXC_CRASH (SIGABRT)               我们的程序处罚了一个程序语言异常,例如失败的断言,这导致操作系统中终止我们的应用程序
EXC_CRASH (SIGQUIT)               一个进程从另一个正在管理他的进程接收到退出信号,通常,意味着某个拓展程序花费了太长的时间或者消费了太多的内存, pp的拓展程序仅能够获得优先的内存
EXC_CRASH (SIGKILL)               系统终止了我们的App(或App的拓展程序) 通常是因为已经到达了某种资源限制,我们需要研究终止原因, 已确定违反了摸个政策是终止原因.

EXC_BAD_ACCESS (SIGSEGV)          我们的程序很可能试图访问错误的或者是没有权限访问的内存地址,或由于内存压力,该内存被释放
EXC_BAD_ACCESS (SIGBUS)           数据内存未对齐访问问题.

EXC_BREAKPOINT (SIGTRAP)         这可能是由于触发了一个 NSException(可能是我们自己的库触发的) 或者是调用了 _NSLockError 或者 objc_exception_throw 方法
                                 例如: 可能是因为 swift 检测到异常,例如是强制展开 nil 可选

EXC_BAD_INSTRUCT (SIGILL)        这是程序代码本身有问题,而不是因为错误的内存访问,这在iOS设备上应该很少见,可能是编译器或优化器错误, 或者是错误的手写汇编代码,但在模拟器上是不一样的.因为使用未定义
                                的操作码是 swift 运行时用来停止访问僵尸对象(已分配对象)的一种技术

EXC_GUARD                       这个问题发生在程序去关闭一个受保护的文件,例如: 关闭系统使用的SQLite库.


Thread 0 name:  Dispatch queue: com.apple.main-thread //崩溃的线程名称 (如果有名称的话)
Thread 0 Crashed: //崩溃的线程编号

                                          崩溃处的地址      加载起始位置    偏移量
0   libMobileGestalt.dylib            0x0000000193523b08 0x19351e000 + 23304
1   libMobileGestalt.dylib            0x0000000193523964 0x19351e000 + 22884
2   libMobileGestalt.dylib            0x0000000193523964 0x19351e000 + 22884
3   libMobileGestalt.dylib            0x0000000193524734 0x19351e000 + 26420
4   libMobileGestalt.dylib            0x0000000193524c58 0x19351e000 + 27736
5   WeChat                            0x00000001009b86e8 0x1008bc000 + 1033960
6   WeChat                            0x00000001009b8478 0x1008bc000 + 1033336

Thread 1:
0   libsystem_kernel.dylib            0x00000001b047461c 0x1b044d000 + 161308
1   libsystem_pthread.dylib           0x00000001cce2945c 0x1cce27000 + 9308
2   matrixreport                      0x000000010e67f558 0x10e630000 + 324952
3   matrixreport                      0x000000010e647790 0x10e630000 + 96144
4   matrixreport                      0x000000010e68201c 0x10e630000 + 335900
5   libsystem_pthread.dylib           0x00000001cce2fc9c 0x1cce27000 + 35996
6   libsystem_pthread.dylib           0x00000001cce34880 0x1cce27000 + 55424

Binary Images:
    加载的位置               名称   架构      UUID                              路径
0x1008bc000 - 0x10b2d7fff WeChat arm64  <13d116da7a09339d98cc63d8adb4040f> /var/containers/Bundle/Application/87640D0D-A567-4C11-8CDA-72D1757E5418/WeChat.app/WeChat
0x10e548000 - 0x10e55ffff mm_dart_cpp arm64  <5e64289d381b39528c905b0777915c73> /var/containers/Bundle/Application/87640D0D-A567-4C11-8CDA-72D1757E5418/WeChat.app/Frameworks/mm_dart_cpp.framework/mm_dart_cpp
0x10e56c000 - 0x10e59ffff Lottie arm64  <4be045b05a1d334a8987ae92643d8eff> /var/containers/Bundle/Application/87640D0D-A567-4C11-8CDA-72D1757E5418/WeChat.app/Frameworks/Lottie.framework/Lottie
0x10e5fc000 - 0x10e617fff ProtobufLite arm64  <1c6bb68496b83cdcad1f6ae8899c63a8> /var/containers/Bundle/Application/87640D0D-A567-4C11-8CDA-72D1757E5418/WeChat.app/Frameworks/ProtobufLite.framework/ProtobufLite
0x10e630000 - 0x10e693fff matrixreport arm64  <1e7f06d2dd3631a8af3b00d62054e1f9> /var/containers/Bundle/Application/87640D0D-A567-4C11-8CDA-72D1757E5418/WeChat.app/Frameworks/matrixreport.framework/matrixreport
0x10e79c000 - 0x10e807fff dyld arm64  <b92286197bbb32eeb90d689ac449aa60> /usr/lib/dyld

EOF

