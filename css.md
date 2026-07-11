# 原生CSS

# CSS预处理器

# Tailwind CSS

> 这是一种与传统“语义化CSS”截然不同的思路，它提供了一系列高度可组合的、功能单一的“原之类”（Atomic CSS）。
> 
> - 核心思想：不再为组件编写专门的CSS类，而是直接在HTML中组合这些原子类来构建样式。

## 使用

### 依赖安装

> 在v4版本中，`tailwindcss`包本身就是引擎，而`@tailwindcss/vite`则是与vite集成的官方插件。

```batch
# 安装Tailwind CSS v4 和 vite 插件
npm install -D tailwindcss @tailwindcss/vite
```

### 配置`vite.config.ts`

> v4版本中最大的变化之一是`零配置`优先。不再需要`tailwind.config.js`和`postcss.config.js`文件。所有配置都可以直接在Vite插件中完成。

```ts
// 在vite中的配置
import tailwindcss from "@tailwindcss/vite";

// 在vue中使用
export default defineConfig({
  plugins: [vue(), tailwindcss()],
});
```
