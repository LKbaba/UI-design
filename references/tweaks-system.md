# Tweaks 变体探索系统

> 什么时候读这份：需要多个设计方向 / 变体探索时。它定义了如何在同一文件内暴露多个设计变体，避免文件爆炸。

---

## 核心原则

1. **同一文件内切换**——不为每个变体建新文件
2. **少即是多**——每个设计 2-3 个 Tweak，不是 10 个
3. **默认值就是成品**——Tweaks 面板关掉也是可交付的设计
4. **有意义的选项**——每个 Tweak 代表一个真实的设计决策，不是"要不要加个渐变"

---

## 架构

### React/Next.js 项目

使用 `useTweaks()` hook + `TweaksPanel` 组件：

```tsx
// hooks/useTweaks.ts
// 用 localStorage 持久化（不依赖 postMessage）
import { useState, useEffect } from 'react';

type TweakValue = string | number | boolean;

interface TweakConfig {
  label: string;                          // 显示名称
  type: 'select' | 'range' | 'toggle';   // 控件类型
  options?: string[];                     // select 的选项
  min?: number;                           // range 最小值
  max?: number;                           // range 最大值
  step?: number;                          // range 步长
  default: TweakValue;                    // 默认值
}

export function useTweaks(config: Record<string, TweakConfig>) {
  const [values, setValues] = useState<Record<string, TweakValue>>(() => {
    // 从 localStorage 恢复，或用默认值
    const stored = typeof window !== 'undefined'
      ? localStorage.getItem('tweaks')
      : null;
    if (stored) {
      try { return { ...getDefaults(config), ...JSON.parse(stored) }; }
      catch { /* 忽略无效 JSON */ }
    }
    return getDefaults(config);
  });

  useEffect(() => {
    localStorage.setItem('tweaks', JSON.stringify(values));
  }, [values]);

  const set = (key: string, value: TweakValue) => {
    setValues(prev => ({ ...prev, [key]: value }));
  };

  const reset = () => {
    const defaults = getDefaults(config);
    setValues(defaults);
    localStorage.removeItem('tweaks');
  };

  return { values, set, reset, config };
}

function getDefaults(config: Record<string, TweakConfig>) {
  return Object.fromEntries(
    Object.entries(config).map(([key, c]) => [key, c.default])
  );
}
```

### TweaksPanel 组件

```tsx
// components/TweaksPanel.tsx
// 浮动面板，右下角，可折叠
'use client';

interface TweaksPanelProps {
  tweaks: ReturnType<typeof useTweaks>;
}

export function TweaksPanel({ tweaks }: TweaksPanelProps) {
  const [open, setOpen] = useState(false);
  const { values, set, reset, config } = tweaks;

  return (
    <div style={{
      position: 'fixed',
      bottom: 16,
      right: 16,
      zIndex: 9999,
      fontFamily: 'system-ui, sans-serif',
    }}>
      {!open ? (
        <button
          onClick={() => setOpen(true)}
          style={{
            padding: '8px 16px',
            background: '#18181B',
            color: '#fff',
            border: 'none',
            borderRadius: 8,
            cursor: 'pointer',
            fontSize: 14,
          }}
        >
          Tweaks
        </button>
      ) : (
        <div style={{
          background: '#fff',
          border: '1px solid #E4E4E7',
          borderRadius: 12,
          padding: 16,
          minWidth: 280,
          boxShadow: '0 4px 24px rgba(0,0,0,0.12)',
        }}>
          <div style={{ display: 'flex', justifyContent: 'space-between', marginBottom: 12 }}>
            <strong>Tweaks</strong>
            <button onClick={() => setOpen(false)} style={{ border: 'none', background: 'none', cursor: 'pointer' }}>✕</button>
          </div>

          {Object.entries(config).map(([key, cfg]) => (
            <div key={key} style={{ marginBottom: 12 }}>
              <label style={{ fontSize: 12, color: '#71717A', display: 'block', marginBottom: 4 }}>
                {cfg.label}
              </label>
              {cfg.type === 'select' && (
                <select
                  value={String(values[key])}
                  onChange={e => set(key, e.target.value)}
                  style={{ width: '100%', padding: 6, borderRadius: 6, border: '1px solid #E4E4E7' }}
                >
                  {cfg.options?.map(opt => <option key={opt} value={opt}>{opt}</option>)}
                </select>
              )}
              {cfg.type === 'range' && (
                <input
                  type="range"
                  min={cfg.min} max={cfg.max} step={cfg.step}
                  value={Number(values[key])}
                  onChange={e => set(key, Number(e.target.value))}
                  style={{ width: '100%' }}
                />
              )}
              {cfg.type === 'toggle' && (
                <button
                  onClick={() => set(key, !values[key])}
                  style={{
                    padding: '4px 12px',
                    background: values[key] ? '#18181B' : '#F4F4F5',
                    color: values[key] ? '#fff' : '#18181B',
                    border: '1px solid #E4E4E7',
                    borderRadius: 6,
                    cursor: 'pointer',
                  }}
                >
                  {values[key] ? '开' : '关'}
                </button>
              )}
            </div>
          ))}

          <button
            onClick={reset}
            style={{
              width: '100%', padding: 8, background: '#F4F4F5',
              border: '1px solid #E4E4E7', borderRadius: 6, cursor: 'pointer',
              fontSize: 12, color: '#71717A',
            }}
          >
            重置为默认
          </button>
        </div>
      )}
    </div>
  );
}
```

### HTML 静态项目

不用 React 时，用原生 JS 实现相同逻辑：

```html
<!-- 放在页面底部 -->
<script>
  // Tweaks 配置
  const TWEAKS_CONFIG = {
    heroLayout: { label: '首屏布局', type: 'select', options: ['居中', '左对齐', '分栏'], default: '居中' },
    showParticles: { label: '粒子背景', type: 'toggle', default: false },
  };

  // 读取/存储
  function getTweaks() {
    try { return { ...getDefaults(), ...JSON.parse(localStorage.getItem('tweaks') || '{}') }; }
    catch { return getDefaults(); }
  }
  function getDefaults() {
    return Object.fromEntries(Object.entries(TWEAKS_CONFIG).map(([k, v]) => [k, v.default]));
  }
  function setTweak(key, value) {
    const tweaks = getTweaks();
    tweaks[key] = value;
    localStorage.setItem('tweaks', JSON.stringify(tweaks));
    location.reload(); // 简单粗暴但对静态页面够用
  }

  // 在代码中使用：const tweaks = getTweaks();
</script>
```

---

## 推荐 Tweaks 项

### 落地页

| Tweak | 类型 | 选项 | 为什么有意义 |
|-------|------|------|------------|
| Hero 布局 | select | 居中 / 左对齐 / 分栏 | 影响信息层级和视觉重心 |
| Section 间距 | select | 紧凑 / 标准 / 宽松 | 影响页面节奏和信息密度 |
| CTA 样式 | select | 实心 / 描边 / 文字链接 | 影响转化率和视觉权重 |

### 仪表盘

| Tweak | 类型 | 选项 | 为什么有意义 |
|-------|------|------|------------|
| 信息密度 | select | 紧凑 / 标准 / 宽松 | 影响单屏展示的数据量 |
| 侧栏状态 | select | 展开 / 折叠 / 隐藏 | 影响内容区宽度 |
| 颜色模式 | toggle | 亮 / 暗 | 仪表盘常见需求 |

### 组件

| Tweak | 类型 | 选项 | 为什么有意义 |
|-------|------|------|------------|
| 尺寸 | select | sm / md / lg | 组件在不同上下文的适配 |
| 变体 | select | primary / secondary / ghost | 视觉权重层级 |
| 带图标 | toggle | 是 / 否 | 信息密度权衡 |

---

## 反模式

### ❌ 每个变体一个文件

```
Hero-v1.tsx    ← 反模式！
Hero-v2.tsx
Hero-v3.tsx
```

### ✅ 同文件 Tweaks 切换

```tsx
// Hero.tsx — 一个文件，通过 Tweaks 切换变体
const tweaks = useTweaks({
  layout: { label: '布局', type: 'select', options: ['居中', '左对齐', '分栏'], default: '居中' },
});

// 根据 tweaks.values.layout 渲染不同布局
```

### ❌ 无意义的 Tweaks

```
字体大小: 14px / 15px / 16px    ← 差异太小，用户看不出
要不要渐变: 是 / 否              ← 违反 anti-slop
动画速度: 100ms / 200ms / 300ms ← 用户不关心具体毫秒
```

### ✅ 有意义的 Tweaks

```
首屏布局: 居中 / 左对齐 / 分栏   ← 真实的设计决策
CTA 强度: 温和 / 标准 / 激进     ← 影响转化策略
信息密度: 紧凑 / 标准 / 宽松     ← 影响用户体验
```

---

## EDITMODE 标记

如果宿主环境支持编辑模式（如 Claude.ai Artifacts），可以用标记块包裹 Tweaks 相关代码：

```tsx
{/* EDITMODE-BEGIN */}
<TweaksPanel tweaks={tweaks} />
{/* EDITMODE-END */}
```

这样宿主环境可以在发布时自动移除 Tweaks 面板。非必须，但向前兼容。
