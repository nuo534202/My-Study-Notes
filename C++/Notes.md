# C++ Notes

## 项目任务

- C++ 风格指南：[Google C++ 风格指南 - 内容目录](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents.html)
- Spdlog 日志库：[spdlog](https://github.com/gabime/spdlog)
	- 掌握 spdlog，学习 spdlog 日志库的使用，简单看看 spdlog 的源码。
	- 了解如何集成 spdlog。
	- 自己写个小 demo，可通过 spdlog 打印不同级别的日志。
	- 了解 spdlog 基本功能的代码实现。
	- 了解 spdlog 的设计架构。
	- 自己画出 spdlog 的架构设计图。
 - 了解常见日志系统的架构，掌握日志系统的架构
	- 理解每个模块的作用。
	- 理解各个模块间如何组合协调工作。
	- 自己总结一下日志系统的结构，自己画个相关的架构图。
 - 掌握高性能日志系统的代码结构
	- 了解项目都有那些模块。
	- 了解每个模块的作用。
- 学习、掌握 CMake 的使用。
	- 如果有多个系统，可以练习通过 CMake 构建 Windows、Linux、macOS 的 demo，并能成功运行。
- 添加日志队列
	- 项目中现在的 `压缩 & 加密 & 写入 mmap cache`，这一整个步骤会加锁保护，多线程等情况下锁会有开销，这里可以考虑使用消息队列的形式，在外部写动作和真正压缩钱，这中间加一个 `消息队列`，外部的消息同意进入消息队列中，内部使用规定线程做 `压缩 & 加密 & 写入 mmap cache` 的操作，这样可以进一步提高日志的写入速度。
 - 在日志压缩前添加一个中间队列。
- 抉择：先压缩还是先加密？
	- 先加密再压缩。
	- 先压缩再加密。
	- 比较两者性能差距。
- 总结与未来期望：完成更多优化和拓展项。
