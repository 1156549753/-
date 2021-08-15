# vue 源码

## 区别介绍

- 源码采用monrepo方式进行管理，将模块拆分到packge目录中
- vue3采用ts开发，增强类型检测。vue2则采用flow
- vue3的性能优化，支持tree-shaking，不使用就不会被打包
- vue2后期引入RFC,使每个版本可控rfcs

> 内部代码优化

- vue3劫持数据采用proxy vue2劫持数据采用Object.defineProperty。Object.defineProperty有性能问题和缺陷
- vue3中对模板编译进行了优化，编译时生成了Block tree，可以对子节点的动态节点进行收集，可以减少比较，并且采用了patchFlag标记动态节点
- vue3采用compositionApi进行组织功能，解决反复横跳，优化复用逻辑（mixin带来的数据来源不清晰，命名冲突等），相比optionApi类型推断更加方便
- 增加了Fragment,Teleport,Suspense组件

## vue架构分析

### 1.Monorepo介绍

`Monorepo`是管理项目代码的一个方式，指在一个项目仓库(`repo`)中管理多个模块/包(package)

- 一个仓库可维护多个模块，不用到处找仓库
- 方便版本管理和依赖管理，模块之间的引用，调用都非常方便

> 缺点：仓库体积会变大。

### 2.Vue3项目结构

- reactivity:响应式系统
- runtime-core:与平台无关的运行是核心(可以创剑针对特定平台的运行时-自定义渲染器)
- runtime-dom:针对浏览器运行时。包括`DOM API`，属性，事件处理等
- runtime-test:用于测试
- server-renderer:用于服务器渲染
- compiler-core:与平台无关的编译器核心
- compiler-dom:针对浏览器的编译模块
- compiler-ssr:针对服务端渲染的编译模块
- compiler-sfc:针对单文件解析
- size-check:用来测试代码体积
- template-explorer:用于调试编译器输出的开发工具
- shared:多个包之间共享的内容
- vue:完整版本，包括运行时和编译器

### 3.安装依赖

| 依赖                        |                        |
| --------------------------- | ---------------------- |
| typscript                   | 支持typescirpt         |
| rollup                      | 打包工具               |
| rollup-plugin-typescript2   | rollup和ts的桥梁       |
| @rollup/plugin-node-resolve | 解析node第三方模块     |
| @rollup/plugin-json         | 支持·引入json          |
| execa                       | 开启子进程方便执行命令 |

