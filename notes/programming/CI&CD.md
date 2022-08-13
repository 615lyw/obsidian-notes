CI/CD 是一种软件开发实践，旨在实现频繁向客户交付可靠软件。

核心概念：持续集成、持续交付、持续部署。

- CI：仅指持续集成
- CD：持续交付或持续部署

# 持续集成

代码提交至版本控制系统后自动构建、自动测试、自动合并的过程。

目的在于尽快暴露错误，用于判断新代码和旧代码能否正确集成。

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/6ROz97.png)

# 持续交付

在持续集成基础上，实现自动化部署至类生产环境，由运维团队手动部署至生产环境。

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/GW6ISv.png)

# 持续部署

在持续交付的基础上，实现自动化部署至生产环境。

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/yYFgP3.png)

# 持续交付和持续部署的区别

部署至生产环境是否实现自动化。

# CI/CD 实现方式

[[Jenkins]]
