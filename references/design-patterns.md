# 设计模式库（正面范例）

> 什么时候读这份：build 阶段写代码前。它提供按场景的布局策略和代码骨架，告诉你"该做什么"。
> 与 `anti-slop.md`（"不该做什么"）互补使用。

---

## 使用原则

1. **从信息目标出发**，不是从模板出发——先问"这个 section 要让用户做什么"
2. **每个 section 一个任务**——不要一个区块塞三个 CTA
3. **下面的模式是起点**，不是终点——根据 `DESIGN.md` 的实际需求调整

---

## 模式 1 · SaaS 落地页

### 信息策略：痛点 → 方案 → 证据 → 行动

**不要用 "Hero + 三栏 Features + 评价轮播 + CTA" 套路。**

```
┌─────────────────────────────────────────┐
│  S1 · 痛点共鸣                          │  ← 不是"大标题+副标题"
│  用一个具体场景描述用户的痛，让他点头      │     而是让用户觉得"你懂我"
│  CTA: 只有一个，放最后                   │
├─────────────────────────────────────────┤
│  S2 · 方案展示                          │  ← 不是三栏 feature 卡片
│  产品截图/GIF + 2-3 个关键能力            │     而是"你的问题这样被解决"
│  用非对称布局：左图右文 或 全宽截图+叠加注释│
├─────────────────────────────────────────┤
│  S3 · 社会证明                          │  ← 不是轮播评价
│  真实 logo 墙（灰度）+ 1 条深度引用       │     一条有力的比十条泛泛的强
│  无真实 logo → 用灰色占位块 + "合作伙伴"  │
├─────────────────────────────────────────┤
│  S4 · 行动                              │  ← 不是 "Ready to get started?"
│  复述核心价值主张 + 单一 CTA              │     回应 S1 的痛点，形成闭环
└─────────────────────────────────────────┘
```

### 代码骨架（HTML + Tailwind）

```html
<!-- S1 · 痛点共鸣 -->
<section class="py-24 lg:py-32">
  <div class="mx-auto max-w-3xl px-6 text-center">
    <!-- 用场景描述，不是口号 -->
    <h1 class="text-3xl font-bold tracking-tight text-zinc-900 sm:text-5xl"
        style="line-height: 1.15">
      [具体的痛点场景，一句话]
    </h1>
    <p class="mt-6 text-lg text-zinc-600 leading-relaxed max-w-2xl mx-auto">
      [展开说明：用户现在怎么痛苦地解决这个问题]
    </p>
    <a href="#" class="mt-8 inline-block rounded-lg bg-zinc-900 px-6 py-3
       text-sm font-semibold text-white hover:bg-zinc-700 transition-colors">
      [行动号召——动词开头]
    </a>
  </div>
</section>

<!-- S2 · 方案展示（左图右文，非对称） -->
<section class="py-24 lg:py-32 bg-zinc-50">
  <div class="mx-auto max-w-7xl px-6 lg:grid lg:grid-cols-5 lg:gap-16 items-center">
    <!-- 产品截图占 3 列 -->
    <div class="lg:col-span-3">
      <div class="aspect-[16/10] rounded-xl bg-zinc-200 border border-zinc-200">
        <!-- 真实截图或占位块 -->
      </div>
    </div>
    <!-- 文字占 2 列 -->
    <div class="mt-12 lg:mt-0 lg:col-span-2">
      <h2 class="text-2xl font-bold text-zinc-900">[方案标题]</h2>
      <ul class="mt-8 space-y-6">
        <li class="flex gap-4">
          <!-- 图标 + 一句话描述，不是卡片 -->
          <div class="flex-shrink-0 w-10 h-10 rounded-lg bg-zinc-100
               flex items-center justify-center">
            <!-- Lucide icon -->
          </div>
          <div>
            <p class="font-semibold text-zinc-900">[能力名称]</p>
            <p class="mt-1 text-sm text-zinc-600">[一句话解释]</p>
          </div>
        </li>
        <!-- 重复 2-3 个 -->
      </ul>
    </div>
  </div>
</section>
```

### 关键设计决策

| 决策 | 为什么 |
|------|--------|
| Hero 用 `max-w-3xl` 而不是全宽 | 控制行宽，提升可读性，避免"空旷感" |
| 非对称 3:2 栅格 | 截图比文字重要，给它更多空间 |
| 间距 `py-24 lg:py-32` | 移动端 96px、桌面 128px，节奏感强 |
| 只有一个 CTA | 减少决策负担，提升转化 |

---

## 模式 2 · 仪表盘 / 后台

### 信息策略：导航 → 概览 → 详情

```
┌──────┬──────────────────────────────────┐
│      │  顶栏（面包屑 + 搜索 + 用户头像）  │
│ 侧   ├──────────────────────────────────┤
│ 栏   │  概览卡片（2-4 个关键指标）        │
│      ├──────────────────────────────────┤
│ 导   │  主内容区                         │
│ 航   │  表格 / 图表 / 列表               │
│      ├──────────────────────────────────┤
│      │  次要信息 / 操作日志               │
└──────┴──────────────────────────────────┘
```

### 代码骨架

```html
<div class="flex h-screen bg-zinc-50">
  <!-- 侧栏 -->
  <nav class="w-64 bg-white border-r border-zinc-200 p-4 flex flex-col">
    <div class="text-lg font-bold text-zinc-900 mb-8">[产品名]</div>
    <ul class="space-y-1 flex-1">
      <li>
        <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-lg
           text-sm font-medium text-zinc-900 bg-zinc-100">
          <!-- 当前页高亮用 bg-zinc-100，不用品牌色 -->
          <!-- icon --> [当前页面]
        </a>
      </li>
      <li>
        <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-lg
           text-sm text-zinc-600 hover:bg-zinc-50 transition-colors">
          <!-- icon --> [其他页面]
        </a>
      </li>
    </ul>
  </nav>

  <!-- 主区域 -->
  <main class="flex-1 overflow-y-auto">
    <!-- 概览卡片 -->
    <div class="p-8 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
      <div class="bg-white rounded-xl border border-zinc-200 p-6">
        <p class="text-sm text-zinc-500">[指标名]</p>
        <p class="mt-2 text-2xl font-bold text-zinc-900">[数值]</p>
        <p class="mt-1 text-xs text-zinc-400">[趋势/对比]</p>
      </div>
      <!-- 重复 2-4 个 -->
    </div>

    <!-- 主内容 -->
    <div class="px-8 pb-8">
      <div class="bg-white rounded-xl border border-zinc-200 overflow-hidden">
        <!-- 表格 / 图表 -->
      </div>
    </div>
  </main>
</div>
```

### 关键设计决策

| 决策 | 为什么 |
|------|--------|
| 侧栏固定宽度 `w-64`(256px) | 不要折叠/展开动效，保持信息可见 |
| 卡片用 `border` 不用 `shadow` | 仪表盘信息密度高，阴影太"重" |
| 指标卡只有 3 行信息 | 标题 + 数值 + 趋势，不堆叠 |
| 主内容区 `overflow-y-auto` | 侧栏固定，内容区可滚动 |

---

## 模式 3 · 文档 / 博客

### 信息策略：导航 → 正文 → 辅助

```
┌──────┬───────────────────────┬────────┐
│ 侧栏  │      正文             │ 目录   │
│ 章节  │  max-w-[65ch]        │ 锚点   │
│ 导航  │  居中                 │ 跟随   │
└──────┴───────────────────────┴────────┘
```

### 代码骨架

```html
<article class="mx-auto max-w-[65ch] px-6 py-16">
  <!-- 正文限制行宽，用 ch 单位 -->
  <h1 class="text-3xl font-bold text-zinc-900 tracking-tight">[标题]</h1>
  <p class="mt-4 text-sm text-zinc-500">[日期 · 作者 · 阅读时长]</p>

  <div class="mt-12 prose prose-zinc prose-lg">
    <!-- 用 prose 类处理富文本排版 -->
    <!-- 如果不用 Tailwind Typography，手动设置： -->
    <!--   段落间距: mb-6 -->
    <!--   标题间距: mt-12 mb-4 -->
    <!--   代码块: bg-zinc-50 rounded-lg p-4 -->
  </div>
</article>
```

### 关键设计决策

| 决策 | 为什么 |
|------|--------|
| `max-w-[65ch]` | 45-75 字符行宽是可读性最优区间 |
| 段落 `text-lg`(18px) | 长文阅读需要比 16px 更大的正文 |
| 标题间距 `mt-12 mb-4` | 标题和上文拉开，和下文亲近 |

---

## 模式 4 · 移动端优先界面

### 关键约束

```
宽度: 375px（iPhone SE）~ 428px（iPhone Pro Max）
触控目标: ≥ 44 × 44px
正文: ≥ 16px（否则 iOS Safari 会自动缩放）
底部安全区: env(safe-area-inset-bottom)
```

### 代码骨架

```html
<!-- 移动端底部导航栏 -->
<nav class="fixed bottom-0 inset-x-0 bg-white border-t border-zinc-200
     pb-[env(safe-area-inset-bottom)]">
  <div class="flex justify-around py-2">
    <a href="#" class="flex flex-col items-center gap-1 min-w-[44px] min-h-[44px]
       justify-center text-zinc-900">
      <!-- icon 24px -->
      <span class="text-xs font-medium">[标签]</span>
    </a>
    <!-- 重复 3-5 个 -->
  </div>
</nav>

<!-- 主内容需要底部留白，避免被导航栏遮挡 -->
<main class="pb-24">
  <!-- ... -->
</main>
```

---

## 模式 5 · 定价页

### 信息策略：对比 → 推荐 → 行动

**不要做三列等宽对比表。**

```html
<!-- 推荐方案视觉突出，非推荐方案弱化 -->
<div class="mx-auto max-w-5xl px-6 py-24">
  <div class="text-center mb-16">
    <h2 class="text-3xl font-bold text-zinc-900">[定价标题]</h2>
    <p class="mt-4 text-lg text-zinc-600">[一句话说明定价逻辑]</p>
  </div>

  <div class="grid md:grid-cols-3 gap-8 items-start">
    <!-- 普通方案 -->
    <div class="rounded-2xl border border-zinc-200 bg-white p-8">
      <h3 class="text-lg font-semibold text-zinc-900">[方案名]</h3>
      <p class="mt-2 text-sm text-zinc-500">[一句话描述]</p>
      <p class="mt-6">
        <span class="text-4xl font-bold text-zinc-900">[价格]</span>
        <span class="text-sm text-zinc-500">/月</span>
      </p>
      <ul class="mt-8 space-y-3 text-sm text-zinc-600">
        <li class="flex gap-3">✓ [功能]</li>
      </ul>
      <a href="#" class="mt-8 block text-center rounded-lg border border-zinc-300
         py-2.5 text-sm font-semibold text-zinc-700 hover:bg-zinc-50
         transition-colors">
        [选择]
      </a>
    </div>

    <!-- 推荐方案：ring + scale -->
    <div class="rounded-2xl border-2 border-zinc-900 bg-white p-8
         relative md:-mt-4 md:mb-[-16px]">
      <span class="absolute -top-3 left-1/2 -translate-x-1/2 rounded-full
           bg-zinc-900 px-4 py-1 text-xs font-medium text-white">
        推荐
      </span>
      <!-- 同样的内容结构，但 CTA 用实心按钮 -->
      <a href="#" class="mt-8 block text-center rounded-lg bg-zinc-900
         py-2.5 text-sm font-semibold text-white hover:bg-zinc-700
         transition-colors">
        [选择]
      </a>
    </div>

    <!-- 普通方案（同第一个） -->
  </div>
</div>
```

### 关键设计决策

| 决策 | 为什么 |
|------|--------|
| 推荐方案 `border-2 border-zinc-900` | 用边框而非背景色突出，更克制 |
| 推荐方案 `-mt-4` 微微上移 | 视觉层级提升但不夸张 |
| 非推荐方案 CTA 用描边按钮 | 降低视觉权重，引导用户选推荐 |

---

## 通用布局决策树

当你不确定用什么布局时，问这几个问题：

```
内容是线性叙事吗？（落地页、文章）
  → 是 → 单列，max-w 限制行宽
  → 否 → 继续

需要同时看多个维度吗？（仪表盘、对比）
  → 是 → 多列栅格，注意移动端坍缩顺序
  → 否 → 继续

内容是同类项集合吗？（产品列表、团队成员）
  → 是 → 卡片网格（auto-fill + minmax），不用 bento
  → 否 → 继续

有主次关系吗？（正文+侧栏、邮件列表+详情）
  → 是 → 非对称两栏（如 3:2 或 2:1）
  → 否 → 问用户，不要猜
```
