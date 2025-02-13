在 **Spring Cloud Alibaba** 的框架下，使用 Nacos 作为配置中心时，配置源的优先级遵循一定的规则。当 Nacos 配置和本地配置（如 application. Properties 或 application. Yml）中出现相同的属性时，通常遵循以下优先级顺序：
- **命令行参数** - 这些是最优先的，可以通过启动应用时直接传递参数来覆盖任何其他配置源中的值。
- **Spring Boot 的默认属性** - 这些是在没有其他来源时使用的默认值。
- **随机值** - 如果使用了 RandomValuePropertySource，它会在这个位置被考虑。
- **Java 系统属性 (System.GetProperties ())** - 可以通过-Dproperty=value 的方式在命令行中设置。
- **操作系统环境变量**
- **JNDI 属性 (java: comp/env)**
- **配置文件 (application. Properties 或 application. Yml)** - 这些是本地配置文件，位于类路径中。
- **Nacos 配置中心的配置** - 这些是从 Nacos 服务器拉取的配置。
- **Spring Boot 的 bootstrap. Properties 或 bootstrap. Yml** - 这些配置文件用于设置 Spring Boot 的初始化属性，如激活特定的特性或配置外部化配置源。
当涉及到 Nacos 配置与本地配置的优先级时，本地配置文件通常具有更高的优先级。这意味着如果在本地配置文件和 Nacos 中存在相同的属性，那么本地配置文件中的值将被使用，除非你通过某种方式显式地更改了这一行为。
如果你想要改变这个优先级，例如让 Nacos 的配置优先于本地配置，你可能需要调整 Spring Boot 的配置逻辑或者使用更复杂的配置策略，比如在 Spring Boot 的 application. Properties 或 application. Yml 中使用 spring. Cloud. Config. Server. Bootstrap=true 来确保 Nacos 的配置在启动时被优先加载。但是，这可能会根据你使用的 Spring Boot 和 Spring Cloud 的版本有所不同，因此建议查阅相关文档或源码以确认具体的行为。