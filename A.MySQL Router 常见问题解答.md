# 附录 A MySQL Router 常见问题解答

A.1. 我应该在哪里安装 MySQL Router？

A.2. 我可以运行多个 Router 应用程序实例吗？

A.3. 如何使 Router 应用程序具有高可用性？

A.4. Router 是否检查数据包？

A.5. Router 是否影响性能？

A.6. 请解释不同的 MySQL Router 版本，尤其是 Router 为什么从 2.1.4 跳到 8.0.3。

A.7. 我可以将 Router 绑定到多个 IP 地址吗？

A.8. 不同的调度模式和策略有什么区别？

A.9. 每个 MySQL Router 实例支持多少并发连接？

A.10. 如何在使用 AppArmor 的系统上配置 MySQL Router 使用非默认目录？

| 问题  | 答案                                                         |
| ----- | ------------------------------------------------------------ |
| A.1.  | 我应该在哪里安装 MySQL Router？                              |
|       | 为了获得最佳性能，通常在使用它的应用程序所在的同一主机上安装 MySQL Router。这样做可以减少网络延迟，允许应用程序通过本地 unix 域套接字连接而不是 TCP/IP，并且通常应用服务器最容易扩展。但这不是一个要求，因为 Router 可以安装在任何主机上，甚至是它自己的主机上。注意：Unix 域套接字可以与连接到 MySQL Router 的应用程序一起工作，但不适用于 MySQL Router 连接到 MySQL Server。 |
| A.2.  | 我可以运行多个 Router 应用程序实例吗？                       |
|       | 是的，也请参见 `--directory` 引导选项。                      |
| A.3.  | 如何使 Router 应用程序具有高可用性？                         |
|       | 将 MySQL Router 作为 InnoDB Cluster 的一部分使用。更多细节，请参见 MySQL AdminAPI。 |
| A.4.  | Router 是否检查数据包？                                      |
|       | 不。                                                         |
| A.5.  | Router 是否影响性能？                                        |
|       | 在通信流中引入组件会带来一定量的开销；这在很大程度上受工作负载影响。幸运的是，当前版本的性能测试显示，对于简单重定向连接路由，性能大约在与直接连接速度相同的 1% 范围内。 |
| A.6.  | 请解释不同的 MySQL Router 版本，尤其是 Router 为什么从 2.1.4 跳到 8.0.3。 |
|       | MySQL Router 2.0 是初始版本，面向 MySQL Fabric 用户。它已经被弃用，不再支持。MySQL Router 2.1 引入了对 MySQL InnoDB cluster 的支持，并添加了新功能，如引导程序。MySQL Router 8.0 在 MySQL Router 2.1 的基础上扩展，但版本号与 MySQL Server 对齐。换句话说，Router 2.1.5 作为 Router 8.0.3（与 MySQL Server 8.0.3 一起）发布，2.1.x 分支被 8.0.x 替代。两个分支完全兼容。 |
| A.7.  | 我可以将 Router 绑定到多个 IP 地址吗？                       |
|       | 不，配置文件中的 `bind_address` 选项只接受一个地址。然而，可以使用 `bind_address = 0.0.0.0` 绑定到 localhost 上的所有端口。 |
| A.8.  | 不同的调度模式和策略有什么区别？                             |
|       | Router 8.0 引入了 `routing_strategy` 选项。它提供了 first-available、next-available、round-robin 和 round-robin-with-fallback 策略。更多细节，请参见 `routing_strategy` 文档。 |
| A.9.  | 每个 MySQL Router 实例支持多少并发连接？                     |
|       | 截至 MySQL Router 8.0.22，超过 50,000，这取决于系统的 poll（poll 或 linux_epoll）限制，也取决于可用的 CPU 核心/线程数量。早期版本的 MySQL Router 有接近 5000 的限制，这取决于操作系统的 poll() 限制。 |
| A.10. | 如何在使用 AppArmor 的系统上配置 MySQL Router 使用非默认目录？ |
|       | 如果您在使用 AppArmor 的系统上使用 `--directory` 选项，例如 Ubuntu，您可能会遇到由于 MySQL Router 访问非默认目录而引起的权限错误。在这种情况下，将您传递给 `--directory` 的路径添加到 AppArmor 文件中，如建议的那样，并重启 AppArmor。 |