# 好坏对比（代码级范例）

> 什么时候读这份：build 阶段写代码时。用真实代码展示"AI 味"和"专业级"的差异。
> 与 `anti-slop.md`（规则清单）和 `design-patterns.md`（正面范例）互补。

---

## 对比 1 · Hero 区块

### ❌ AI 味十足

```html
<section class="min-h-screen bg-gradient-to-br from-purple-600 via-pink-500 to-orange-400
       flex items-center justify-center text-white text-center">
  <div>
    <h1 class="text-7xl font-extrabold mb-6
         bg-gradient-to-r from-yellow-200 to-white bg-clip-text text-transparent">
      🚀 Revolutionize Your Workflow
    </h1>
    <p class="text-2xl mb-8 font-light">
      The AI-powered platform that transforms how teams collaborate, innovate, and succeed.
    </p>
    <div class="space-x-4">
      <button class="bg-white text-purple-600 px-8 py-4 rounded-full text-lg font-bold
            hover:scale-110 transition-transform duration-500">
        Get Started Free
      </button>
      <button class="border-2 border-white px-8 py-4 rounded-full text-lg
            hover:bg-white hover:text-purple-600 transition-all duration-500">
        Watch Demo
      </button>
    </div>
  </div>
</section>
```

**问题清单**：
- 🔴 满屏渐变背景（anti-slop 禁区）
- 🔴 emoji 做图标（🚀）
- 🔴 渐变文字（anti-slop 禁区）
- 🔴 编造口号（"Revolutionize"、"transforms how teams..."）
- 🔴 两个 CTA 按钮（增加决策负担）
- 🔴 `hover:scale-110`（过度动效）
- 🔴 `font-extrabold`（过重）
- 🔴 `text-7xl`（移动端会溢出）
- 🔴 `rounded-full`（pill 按钮是 AI 最爱）

### ✅ 克制有力

```html
<section class="py-24 lg:py-32">
  <div class="mx-auto max-w-3xl px-6">
    <h1 class="text-3xl font-bold tracking-tight text-zinc-900 sm:text-5xl"
        style="line-height: 1.15">
      [你的产品解决了什么问题——用场景描述]
    </h1>
    <p class="mt-6 text-lg leading-relaxed text-zinc-600 max-w-2xl">
      [一两句话说清楚价值主张，不要用"革命性""赋能"等套话]
    </p>
    <div class="mt-8">
      <a href="#" class="inline-block rounded-lg bg-zinc-900 px-6 py-3
         text-sm font-semibold text-white hover:bg-zinc-700
         transition-colors duration-150">
        [动词开头的行动号召]
      </a>
    </div>
  </div>
</section>
```

**为什么好**：
- ✅ 白色背景，让内容说话
- ✅ `max-w-3xl` 控制行宽（约 45-75 字符）
- ✅ 响应式字号 `text-3xl sm:text-5xl`（移动端不溢出）
- ✅ 单个 CTA，减少决策负担
- ✅ `duration-150` 微妙过渡，不炫技
- ✅ 文案用占位符，不编造
- ✅ `tracking-tight` 大标题收紧字距（光学校正）

---

## 对比 2 · Feature 展示

### ❌ 三栏卡片套路

```html
<div class="grid grid-cols-3 gap-8 text-center">
  <div class="bg-white rounded-2xl shadow-xl p-8 hover:-translate-y-2
       transition-transform duration-300">
    <div class="w-16 h-16 mx-auto mb-4 rounded-full
         bg-gradient-to-br from-blue-500 to-cyan-400 flex items-center justify-center">
      <span class="text-3xl">⚡</span>
    </div>
    <h3 class="text-xl font-bold mb-2">Lightning Fast</h3>
    <p class="text-gray-500">Experience blazing fast performance that revolutionizes
       your workflow with cutting-edge AI technology.</p>
  </div>
  <!-- × 3，完全一样的结构 -->
</div>
```

**问题清单**：
- 🔴 三栏等宽对称（anti-slop 套路布局）
- 🔴 渐变圆形 + emoji 图标
- 🔴 `shadow-xl` + hover 上浮（过度装饰）
- 🔴 编造文案（"blazing fast"、"cutting-edge AI"）
- 🔴 `grid-cols-3` 未处理响应式

### ✅ 左图右文非对称

```html
<section class="py-24 bg-zinc-50">
  <div class="mx-auto max-w-6xl px-6 space-y-24">
    <!-- 功能 1：左图右文 -->
    <div class="lg:grid lg:grid-cols-5 lg:gap-16 items-center">
      <div class="lg:col-span-3">
        <div class="aspect-[16/10] rounded-xl bg-zinc-200 border border-zinc-200
             overflow-hidden">
          <!-- 真实产品截图，或灰色占位块 -->
        </div>
      </div>
      <div class="mt-8 lg:mt-0 lg:col-span-2">
        <div class="w-10 h-10 rounded-lg bg-zinc-100 flex items-center justify-center">
          <!-- Lucide 图标，不用 emoji -->
        </div>
        <h3 class="mt-4 text-xl font-bold text-zinc-900">[功能名称]</h3>
        <p class="mt-3 text-zinc-600 leading-relaxed">[解释这个功能解决了什么问题]</p>
      </div>
    </div>

    <!-- 功能 2：右图左文（交替） -->
    <div class="lg:grid lg:grid-cols-5 lg:gap-16 items-center">
      <div class="mt-8 lg:mt-0 lg:col-span-2 lg:order-1">
        <!-- 文字 -->
      </div>
      <div class="lg:col-span-3 lg:order-2">
        <!-- 图片 -->
      </div>
    </div>
  </div>
</section>
```

**为什么好**：
- ✅ 非对称 3:2 布局（截图比文字重要）
- ✅ 左右交替增加阅读节奏
- ✅ 专业图标（Lucide），不是 emoji
- ✅ 每个功能有产品截图，不是抽象描述
- ✅ 响应式：移动端自然坍缩为单列

---

## 对比 3 · 配色

### ❌ 彩虹系

```css
:root {
  --primary: #8B5CF6;       /* 紫色 */
  --secondary: #EC4899;      /* 粉色 */
  --accent: #F59E0B;         /* 橙色 */
  --background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  --card-bg: rgba(255, 255, 255, 0.1);  /* 玻璃态 */
}
```

**问题**：紫+粉+橙=视觉噪音，渐变背景+玻璃态=AI 味

### ✅ 克制中性

```css
:root {
  --brand: #18181B;          /* zinc-900，品牌主色用深色 */
  --bg: #FFFFFF;
  --bg-secondary: #F4F4F5;  /* zinc-100 */
  --text: #18181B;           /* zinc-900 */
  --text-secondary: #71717A; /* zinc-500 */
  --text-muted: #52525B;     /* zinc-600，白底对比度 7.2:1 */
  --border: #E4E4E7;         /* zinc-200 */
  --ring: #18181B;           /* focus ring */
}
```

**为什么好**：
- ✅ 中性色系，让内容说话
- ✅ 对比度达标（zinc-900 / white = 18.1:1）
- ✅ 色阶层次清晰（900 → 500 → 400 → 200）
- ✅ 无渐变，无半透明，无玻璃态
- ✅ 如用户有品牌色，只替换 `--brand` 一个变量

---

## 对比 4 · 按钮

### ❌ 过度设计

```html
<button class="bg-gradient-to-r from-purple-500 to-pink-500 text-white
       px-8 py-4 rounded-full text-lg font-bold shadow-lg shadow-purple-500/50
       hover:scale-110 hover:shadow-2xl transition-all duration-500
       animate-pulse">
  ✨ Get Started Now ✨
</button>
```

### ✅ 简洁有力

```html
<!-- 主要操作 -->
<button class="rounded-lg bg-zinc-900 px-4 py-2.5 text-sm font-semibold
       text-white hover:bg-zinc-700 transition-colors duration-150
       focus-visible:outline-2 focus-visible:outline-offset-2
       focus-visible:outline-zinc-900">
  [动词开头]
</button>

<!-- 次要操作 -->
<button class="rounded-lg border border-zinc-300 px-4 py-2.5 text-sm
       font-semibold text-zinc-700 hover:bg-zinc-50 transition-colors
       duration-150">
  [动词开头]
</button>

<!-- 弱操作 -->
<button class="text-sm font-semibold text-zinc-600 hover:text-zinc-900
       transition-colors duration-150 underline-offset-4 hover:underline">
  [动词开头]
</button>
```

**为什么好**：
- ✅ 三级视觉权重（实心 → 描边 → 文字）
- ✅ `rounded-lg` 而非 `rounded-full`（更专业）
- ✅ `duration-150` 快速过渡（不拖泥带水）
- ✅ `focus-visible` 无障碍焦点样式
- ✅ 没有 `animate-pulse`、`hover:scale`、渐变、emoji

---

## 对比 5 · 间距

### ❌ 随意间距

```html
<div class="p-5">           <!-- 20px，不在刻度上 -->
  <h2 class="mb-3">...</h2>  <!-- 12px，和上文关系不明 -->
  <p class="mb-7">...</p>    <!-- 28px，不在刻度上 -->
  <div class="mt-10">...</div><!-- 40px，不在刻度上 -->
</div>
```

### ✅ 8px 网格纪律

```html
<div class="p-6">             <!-- 24px ✓ -->
  <h2 class="mb-4">...</h2>   <!-- 16px ✓ 标题和正文亲近 -->
  <p class="mb-8">...</p>     <!-- 32px ✓ 段落间明确分隔 -->
  <div class="mt-12">...</div> <!-- 48px ✓ section 内分隔 -->
</div>
```

**规则**：
- 间距只用 `1(4px)·2(8px)·3(12px)·4(16px)·6(24px)·8(32px)·12(48px)·16(64px)·24(96px)·32(128px)`
- 相关元素间距小（4-16px），不相关元素间距大（32-128px）
- 同级元素间距必须一致

---

## 对比 6 · 卡片

### ❌ 过度装饰

```html
<div class="bg-white rounded-3xl shadow-2xl p-8 border border-purple-100
     hover:shadow-purple-200 hover:-translate-y-4 transition-all duration-700
     backdrop-blur-xl bg-opacity-80">
  <!-- 玻璃态 + 巨大阴影 + 悬浮动效 + 紫色边框 -->
</div>
```

### ✅ 干净利落

```html
<div class="bg-white rounded-xl border border-zinc-200 p-6
     hover:border-zinc-300 transition-colors duration-150">
  <!-- 细边框 + 微妙 hover 反馈，够了 -->
</div>
```

**为什么好**：
- ✅ `border` 而非 `shadow`（更轻、更现代——Linear/Vercel 风格）
- ✅ `rounded-xl`(12px) 而非 `rounded-3xl`(24px)（更专业）
- ✅ hover 只改边框色，不移动卡片
- ✅ `duration-150` 快反馈

---

## 对比 7 · 去掉 slop 后如何加质感

去掉了渐变、emoji、glass 之后页面可能显得"平"。以下是专业设计师加质感的方式——**不触碰 anti-slop 禁区**。

### 字距微调（Letter-spacing）

```css
/* 大标题收紧——视觉更紧凑有力 */
h1 { letter-spacing: -0.025em; }  /* 48px 以上可以到 -0.05em */

/* 全大写标签加宽——否则字母挤成一团 */
.tag { text-transform: uppercase; letter-spacing: 0.05em; font-size: 0.75rem; }
```

**规则**：字号越大，字距越紧；全大写必须加宽。正文保持 0。

### 按钮微交互（:active 反馈）

```css
/* 点击时轻微缩小——给用户"按下去了"的触感 */
button:active {
  transform: scale(0.98);
}

/* 不要做成 hover 效果（hover:scale-110 是 slop），只用在 :active */
/* 不要超过 0.98——太夸张就成了 slop */
```

### 边框 hover 过渡

```css
/* 卡片 hover 边框变深——最克制的交互反馈 */
.card {
  border: 1px solid var(--border-default, #E4E4E7);
  transition: border-color 150ms ease-out;
}
.card:hover {
  border-color: var(--border-hover, #A1A1AA);  /* zinc-200 → zinc-400 */
}

/* 不要 shadow、不要 translate、不要 scale——只改颜色 */
```

### 文字层次精修

```css
/* 用透明度区分同色系文字层级——比用不同灰度更统一 */
.text-primary   { color: #18181B; }                    /* 100% */
.text-secondary { color: #71717A; }                    /* 实色次要 */
.text-muted     { color: #52525B; }                    /* 实色弱化 */

/* 标题用 text-wrap: balance 避免丑陋换行 */
h1, h2 { text-wrap: balance; }

/* 正文用 text-wrap: pretty 避免孤字 */
p { text-wrap: pretty; }
```

### Section 交替背景

```css
/* 白 → 浅灰交替——不用渐变就能划分区域 */
.section-alt { background: var(--bg-secondary, #F4F4F5); }

/* 深色区块（一个页面最多一处）——用于 CTA 或 Footer */
.section-dark {
  background: var(--bg-dark, #18181B);
  color: #FFFFFF;
}
```

### 图标用 SVG，不用 emoji

```html
<!-- ❌ -->
<span>🚀</span>

<!-- ✅ 用 Lucide / Heroicons / Phosphor 等 SVG 图标库 -->
<svg class="w-5 h-5 text-zinc-400" ...>...</svg>

<!-- 纯 HTML 环境下可以用内联 SVG，不算外部依赖 -->
```

### 质感加分清单

| 技巧 | 效果 | 注意 |
|------|------|------|
| `letter-spacing: -0.025em` 大标题 | 紧凑有力 | 不要用在正文和 CJK |
| `button:active { scale(0.98) }` | 按钮触感 | 只用 :active，不用 hover |
| `border-color` hover 过渡 | 微妙反馈 | 不要 shadow/translate |
| `text-wrap: balance` 标题 | 避免丑换行 | 浏览器支持已广泛 |
| `text-wrap: pretty` 正文 | 避免孤字行 | 同上 |
| 白/灰交替背景 | 区块划分 | 不要渐变 |
| SVG 图标 | 专业感 | 风格统一，线宽一致 |
| `:focus-visible` 焦点环 | 无障碍 + 专业 | 用 accent 色 |

---

## 总结：一句话记住

> **好设计做减法，AI 设计做加法。**
> 每多一个渐变、一个阴影、一个动效、一个字重——先问：删掉它会不会更好？
> 答案几乎总是：会。
>
> **但减法不等于无聊。** 字距、微交互、边框过渡、排版细节——这些是不喧哗的质感。
