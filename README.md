<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-16" />
  <title>Гид по созданию Telegram‑ботов — Python и конструктор (robochat.io)</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --bg: #050816;
      --bg-alt: #020617;
      --accent: #00c6ff;
      --accent-soft: rgba(0, 198, 255, 0.12);
      --accent2: #7b2ff7;
      --accent2-soft: rgba(123, 47, 247, 0.1);
      --text: #f9fafb;
      --muted: #9ca3af;
      --card-bg: rgba(15, 23, 42, 0.95);
      --card-elevated: rgba(15, 23, 42, 0.98);
      --border: rgba(148, 163, 184, 0.35);
      --radius-lg: 18px;
      --radius-xl: 24px;
      --shadow-lg: 0 24px 60px rgba(15, 23, 42, 0.95);
      --shadow-soft: 0 18px 40px rgba(15, 23, 42, 0.85);
      --nav-h: 74px;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text",
        "Segoe UI", sans-serif;
      background: radial-gradient(circle at 0% 0%, #0ea5e9 0, transparent 45%),
        radial-gradient(circle at 100% 0%, #7c3aed 0, transparent 45%),
        radial-gradient(circle at 0% 100%, #22c55e 0, transparent 40%),
        linear-gradient(135deg, #020617, #020617 50%, #020617);
      color: var(--text);
      min-height: 100vh;
      -webkit-font-smoothing: antialiased;
    }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160' viewBox='0 0 160 160'%3E%3Cdefs%3E%3ClinearGradient id='g' x1='0' y1='0' x2='1' y2='1'%3E%3Cstop stop-color='%2318253a' stop-opacity='0.7' offset='0'/%3E%3Cstop stop-color='%230b1120' stop-opacity='0.95' offset='1'/%3E%3C/linearGradient%3E%3C/defs%3E%3Crect width='160' height='160' fill='url(%23g)'/%3E%3Cpath d='M0 159h160M0 140h160M0 120h160M0 100h160M0 80h160M0 60h160M0 40h160M0 20h160M0 0h160M0 0v160M20 0v160M40 0v160M60 0v160M80 0v160M100 0v160M120 0v160M140 0v160M160 0v160' stroke='%231e293b' stroke-width='0.5' opacity='0.55'/%3E%3C/svg%3E");
      mix-blend-mode: soft-light;
      opacity: 0.9;
      pointer-events: none;
      z-index: -2;
    }

    .shell {
      max-width: 1160px;
      margin: 0 auto;
      padding: 0 18px 80px;
    }

    .site-header {
      position: sticky;
      top: 0;
      z-index: 40;
      backdrop-filter: blur(20px);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.18), transparent 45%),
        radial-gradient(circle at 100% 0, rgba(129, 140, 248, 0.2), transparent 40%),
        linear-gradient(to bottom, rgba(15, 23, 42, 0.96), rgba(15, 23, 42, 0.86));
      border-bottom: 1px solid rgba(148, 163, 184, 0.45);
      box-shadow: 0 18px 35px rgba(15, 23, 42, 0.9);
    }

    .site-header-inner {
      max-width: 1160px;
      margin: 0 auto;
      height: var(--nav-h);
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 18px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .brand-logo {
      width: 30px;
      height: 30px;
      border-radius: 999px;
      background: conic-gradient(
          from 120deg,
          #0ea5e9,
          #6366f1,
          #ec4899,
          #22c55e,
          #0ea5e9
        );
      position: relative;
      box-shadow: 0 0 0 1px rgba(15, 23, 42, 0.9),
        0 0 26px rgba(34, 211, 238, 0.7);
      overflow: hidden;
    }

    .brand-logo::after {
      content: "";
      position: absolute;
      inset: 2px;
      border-radius: inherit;
      background: radial-gradient(circle at 30% 20%, #e5f3ff, #0f172a);
      mix-blend-mode: screen;
      opacity: 0.9;
    }

    .brand-title {
      font-weight: 650;
      letter-spacing: 0.04em;
      font-size: 0.97rem;
      text-transform: uppercase;
      color: #e5e7eb;
    }

    .brand-sub {
      font-size: 0.7rem;
      color: var(--muted);
      text-transform: uppercase;
      letter-spacing: 0.18em;
    }

    .nav {
      display: flex;
      gap: 18px;
      align-items: center;
      font-size: 0.9rem;
    }

    .nav a {
      color: var(--muted);
      text-decoration: none;
      padding: 6px 10px;
      border-radius: 999px;
      transition: all 0.18s ease;
      position: relative;
      border: 1px solid transparent;
    }

    .nav a span {
      font-size: 0.75rem;
      opacity: 0.7;
      margin-right: 3px;
    }

    .nav a:hover {
      color: #e5e7eb;
      border-color: rgba(148, 163, 184, 0.4);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.16), transparent);
      box-shadow: 0 10px 25px rgba(15, 23, 42, 0.9);
    }

    .nav a.is-active {
      color: #e5e7eb;
      border-color: rgba(129, 140, 248, 0.8);
      background: radial-gradient(circle at 0 0, rgba(79, 70, 229, 0.45), transparent 55%);
      box-shadow: 0 14px 32px rgba(15, 23, 42, 0.95);
    }

    .nav-cta {
      border-radius: 999px;
      padding: 7px 16px;
      border: 1px solid rgba(56, 189, 248, 0.9);
      color: #e0f2fe;
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.35), transparent 55%),
        radial-gradient(circle at 80% 0, rgba(56, 189, 248, 0.05), transparent 50%);
      box-shadow: 0 14px 40px rgba(8, 47, 73, 0.95);
      font-size: 0.82rem;
      text-decoration: none;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    .nav-cta span {
      opacity: 0.8;
      font-size: 0.78rem;
    }

    .nav-cta:hover {
      transform: translateY(-1px);
      box-shadow: 0 18px 46px rgba(8, 47, 73, 0.98);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.5), transparent 55%);
    }

    .hero {
      padding: 40px 0 50px;
    }

    .hero-grid {
      display: grid;
      grid-template-columns: minmax(0, 1.45fr) minmax(0, 1fr);
      gap: 26px;
      align-items: stretch;
    }

    .hero-main {
      padding: 24px 24px 26px;
      border-radius: var(--radius-xl);
      border: 1px solid rgba(148, 163, 184, 0.4);
      background:
        radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.28), transparent 55%),
        radial-gradient(circle at 90% 0, rgba(79, 70, 229, 0.46), transparent 60%),
        radial-gradient(circle at 20% 120%, rgba(16, 185, 129, 0.18), transparent 52%),
        linear-gradient(145deg, rgba(15, 23, 42, 0.96), rgba(15, 23, 42, 0.94));
      box-shadow: var(--shadow-lg);
      position: relative;
      overflow: hidden;
    }

    .hero-main::before {
      content: "";
      position: absolute;
      inset: -1px;
      background: conic-gradient(
        from 200deg,
        rgba(56, 189, 248, 0.12),
        rgba(59, 130, 246, 0.03),
        rgba(147, 51, 234, 0.22),
        rgba(244, 114, 182, 0.0),
        rgba(34, 197, 94, 0.14),
        rgba(56, 189, 248, 0.08)
      );
      mix-blend-mode: soft-light;
      opacity: 0.9;
      pointer-events: none;
      z-index: -1;
    }

    .eyebrow {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 4px 10px 4px 4px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.35);
      background: radial-gradient(circle at 0 0, rgba(34, 211, 238, 0.4), transparent 60%),
        linear-gradient(135deg, rgba(15, 23, 42, 0.85), rgba(15, 23, 42, 0.95));
      color: #e0f2fe;
      font-size: 0.75rem;
      margin-bottom: 16px;
      width: fit-content;
    }

    .eyebrow-badge {
      background: radial-gradient(circle at 30% 30%, #f9fafb, #38bdf8);
      color: #020617;
      font-weight: 700;
      border-radius: 999px;
      padding: 4px 8px;
      font-size: 0.7rem;
      box-shadow: 0 0 0 1px rgba(8, 47, 73, 0.65), 0 0 0 4px rgba(56, 189, 248, 0.45);
    }

    .eyebrow-label {
      opacity: 0.82;
    }

    .hero-title {
      font-size: clamp(1.9rem, 3.1vw, 2.4rem);
      font-weight: 780;
      letter-spacing: 0.02em;
      line-height: 1.15;
      margin-bottom: 14px;
    }

    .hero-title span {
      background: linear-gradient(120deg, #7dd3fc, #e5e7eb);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-shadow: 0 16px 40px rgba(15, 23, 42, 0.95);
    }

    .hero-subtitle {
      font-size: 0.96rem;
      color: #e5e7eb;
      max-width: 34rem;
      line-height: 1.55;
      margin-bottom: 18px;
    }

    .hero-sub-muted {
      font-size: 0.84rem;
      color: var(--muted);
      margin-bottom: 20px;
    }

    .hero-actions {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 20px;
    }

    .btn-primary {
      border-radius: 999px;
      padding: 9px 16px;
      font-size: 0.88rem;
      border: 1px solid rgba(249, 250, 251, 0.8);
      color: #0b1120;
      font-weight: 600;
      background: radial-gradient(circle at 0 0, #f97316, #eab308 30%, #22c55e 65%, #0ea5e9);
      box-shadow: 0 16px 38px rgba(15, 23, 42, 0.96);
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 8px;
      text-decoration: none;
    }

    .btn-primary span.icon {
      font-size: 1rem;
    }

    .btn-primary:hover {
      transform: translateY(-1px) scale(1.01);
      box-shadow: 0 22px 52px rgba(15, 23, 42, 0.98);
    }

    .btn-ghost {
      border-radius: 999px;
      padding: 8px 14px;
      font-size: 0.86rem;
      border: 1px solid rgba(148, 163, 184, 0.6);
      background: radial-gradient(circle at 0 0, rgba(148, 163, 184, 0.4), transparent 55%),
        linear-gradient(120deg, rgba(15, 23, 42, 0.85), rgba(15, 23, 42, 0.95));
      color: #e5e7eb;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      text-decoration: none;
    }

    .btn-ghost:hover {
      border-color: rgba(129, 140, 248, 0.8);
      box-shadow: 0 16px 38px rgba(15, 23, 42, 0.9);
      transform: translateY(-1px);
    }

    .hero-pills {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      font-size: 0.75rem;
      color: var(--muted);
    }

    .hero-pill {
      padding: 4px 8px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.45);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.1), transparent 60%);
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    .hero-pill strong {
      font-weight: 600;
      color: #e5e7eb;
      font-size: 0.76rem;
    }

    .hero-side {
      border-radius: var(--radius-xl);
      border: 1px solid rgba(148, 163, 184, 0.42);
      background: linear-gradient(155deg, rgba(15, 23, 42, 0.96), rgba(15, 23, 42, 0.96)),
        radial-gradient(circle at 10% 0, rgba(56, 189, 248, 0.25), transparent 50%);
      padding: 18px 18px 20px;
      box-shadow: var(--shadow-soft);
      display: flex;
      flex-direction: column;
      gap: 14px;
      position: relative;
      overflow: hidden;
    }

    .hero-side::before {
      content: "";
      position: absolute;
      inset: -1px;
      background: radial-gradient(circle at 100% 0, rgba(129, 140, 248, 0.3), transparent 55%);
      mix-blend-mode: screen;
      opacity: 0.7;
      pointer-events: none;
      z-index: -1;
    }

    .hero-side-title {
      font-size: 0.86rem;
      text-transform: uppercase;
      letter-spacing: 0.15em;
      color: var(--muted);
      margin-bottom: 2px;
    }

    .hero-side-main {
      font-size: 0.98rem;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .hero-step-list {
      list-style: none;
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin-top: 6px;
      font-size: 0.82rem;
    }

    .hero-step {
      display: grid;
      grid-template-columns: auto minmax(0, 1fr);
      gap: 8px;
      align-items: flex-start;
      padding: 7px 8px;
      border-radius: 12px;
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.12), transparent 50%);
      border: 1px solid rgba(148, 163, 184, 0.32);
    }

    .hero-step-num {
      width: 22px;
      height: 22px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      display: inline-flex;
      align-items: center;
      justify-content: center;
      font-size: 0.78rem;
      color: #e5e7eb;
      background: radial-gradient(circle at 30% 20%, rgba(248, 250, 252, 0.8), #0f172a);
      box-shadow: 0 10px 20px rgba(15, 23, 42, 0.9);
    }

    .hero-step-label {
      font-weight: 500;
      color: #e5e7eb;
    }

    .hero-step-desc {
      color: var(--muted);
      font-size: 0.78rem;
    }

    .hero-side-footer {
      margin-top: 8px;
      font-size: 0.78rem;
      color: var(--muted);
      border-top: 1px dashed rgba(148, 163, 184, 0.5);
      padding-top: 8px;
    }

    .chip-inline {
      display: inline-flex;
      align-items: center;
      gap: 5px;
      padding: 3px 7px;
      border-radius: 999px;
      border: 1px solid rgba(56, 189, 248, 0.7);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.24), transparent 60%);
      color: #e0f2fe;
      font-size: 0.72rem;
      margin-left: 3px;
    }

    .section {
      margin-top: 32px;
      padding: 22px 20px 22px;
      border-radius: var(--radius-xl);
      border: 1px solid var(--border);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.16), transparent 55%),
        radial-gradient(circle at 100% 0, rgba(79, 70, 229, 0.19), transparent 65%),
        linear-gradient(135deg, rgba(15, 23, 42, 0.98), rgba(15, 23, 42, 0.96));
      box-shadow: var(--shadow-soft);
    }

    .section-header {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: flex-end;
      gap: 8px;
      margin-bottom: 14px;
    }

    .section-kicker {
      font-size: 0.75rem;
      text-transform: uppercase;
      letter-spacing: 0.16em;
      color: var(--muted);
      margin-bottom: 4px;
    }

    .section-title {
      font-size: 1.12rem;
      font-weight: 650;
      letter-spacing: 0.02em;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .section-title span.badge {
      font-size: 0.75rem;
      font-weight: 550;
      padding: 3px 7px;
      border-radius: 999px;
      border: 1px solid rgba(56, 189, 248, 0.8);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.42), transparent 60%);
      color: #e0f2fe;
    }

    .section-sub {
      font-size: 0.86rem;
      color: var(--muted);
      max-width: 36rem;
    }

    .steps-grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 14px;
      margin-top: 10px;
    }

    .step-card {
      border-radius: var(--radius-lg);
      border: 1px solid rgba(148, 163, 184, 0.4);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.15), transparent 55%),
        linear-gradient(145deg, rgba(15, 23, 42, 0.96), rgba(15, 23, 42, 0.98));
      padding: 12px 11px 13px;
      font-size: 0.85rem;
      position: relative;
      overflow: hidden;
    }

    .step-card::before {
      content: attr(data-step);
      position: absolute;
      top: 9px;
      right: 10px;
      font-size: 0.78rem;
      color: rgba(148, 163, 184, 0.9);
      padding: 2px 7px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.6);
      background: rgba(15, 23, 42, 0.9);
    }

    .step-title {
      font-weight: 600;
      margin-bottom: 4px;
    }

    .step-desc {
      color: var(--muted);
      font-size: 0.82rem;
      margin-bottom: 4px;
    }

    .step-list {
      margin-left: 1em;
      margin-top: 4px;
      color: var(--muted);
      font-size: 0.8rem;
    }

    .step-list li + li {
      margin-top: 3px;
    }

    .badge-inline {
      display: inline-flex;
      align-items: center;
      gap: 5px;
      padding: 2px 7px;
      border-radius: 999px;
      font-size: 0.72rem;
      border: 1px solid rgba(96, 165, 250, 0.85);
      background: radial-gradient(circle at 0 0, rgba(37, 99, 235, 0.6), transparent 60%);
      color: #e5f2ff;
    }

    .code-block {
      margin-top: 10px;
      border-radius: 14px;
      background: #020617;
      border: 1px solid rgba(30, 64, 175, 0.8);
      box-shadow: 0 14px 30px rgba(15, 23, 42, 0.9);
      padding: 10px 12px;
      overflow-x: auto;
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas,
        "Liberation Mono", "Courier New", monospace;
      font-size: 0.8rem;
      position: relative;
    }

    .code-block::before {
      content: attr(data-lang);
      position: absolute;
      top: 6px;
      right: 10px;
      font-size: 0.7rem;
      color: rgba(148, 163, 184, 0.9);
      padding: 2px 6px;
      border-radius: 999px;
      border: 1px solid rgba(55, 65, 81, 0.9);
      background: rgba(15, 23, 42, 0.96);
    }

    .code-block pre {
      margin-top: 8px;
      white-space: pre;
    }

    .code-block code {
      color: #e5e7eb;
    }

    .code-comment {
      color: #6b7280;
    }

    .code-keyword {
      color: #60a5fa;
    }

    .code-string {
      color: #f97316;
    }

    .code-func {
      color: #a855f7;
    }

    .code-highlight {
      background: rgba(34, 197, 94, 0.12);
      border-radius: 6px;
    }

    .two-col {
      display: grid;
      grid-template-columns: minmax(0, 1.3fr) minmax(0, 1fr);
      gap: 16px;
      margin-top: 8px;
    }

    .callout {
      border-radius: 14px;
      border: 1px solid rgba(34, 197, 94, 0.7);
      background: radial-gradient(circle at 0 0, rgba(34, 197, 94, 0.25), transparent 60%),
        rgba(15, 23, 42, 0.98);
      padding: 9px 11px;
      font-size: 0.82rem;
      color: #bbf7d0;
    }

    .callout b {
      color: #ecfdf5;
    }

    .tabs {
      margin-top: 10px;
      border-radius: 16px;
      border: 1px solid rgba(148, 163, 184, 0.5);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.16), transparent 55%),
        linear-gradient(135deg, rgba(15, 23, 42, 0.98), rgba(15, 23, 42, 0.96));
      box-shadow: 0 14px 30px rgba(15, 23, 42, 0.95);
      overflow: hidden;
    }

    .tab-buttons {
      display: flex;
      border-bottom: 1px solid rgba(55, 65, 81, 0.9);
      background: radial-gradient(circle at 0 0, rgba(15, 23, 42, 0.5), transparent 60%);
    }

    .tab-button {
      flex: 1;
      padding: 8px 10px;
      font-size: 0.83rem;
      border: 0;
      background: transparent;
      color: var(--muted);
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 6px;
      transition: all 0.18s ease;
    }

    .tab-button span.icon {
      font-size: 0.95rem;
    }

    .tab-button.is-active {
      color: #e5e7eb;
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.28), transparent 65%);
      box-shadow: inset 0 -1px 0 rgba(56, 189, 248, 0.9);
    }

    .tab-button:not(.is-active):hover {
      color: #e5e7eb;
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.12), transparent 60%);
    }

    .tab-panels {
      padding: 10px 12px 12px;
      font-size: 0.86rem;
    }

    .tab-panel {
      display: none;
    }

    .tab-panel.is-active {
      display: block;
    }

    .tab-panel h4 {
      font-size: 0.9rem;
      margin-bottom: 6px;
    }

    .tab-panel ol,
    .tab-panel ul {
      margin-left: 1em;
      margin-top: 4px;
    }

    .tab-panel li + li {
      margin-top: 4px;
    }

    .tab-note {
      margin-top: 6px;
      font-size: 0.8rem;
      color: var(--muted);
    }

    .patterns-grid {
      display: grid;
      grid-template-columns: repeat(4, minmax(0, 1fr));
      gap: 10px;
      margin-top: 8px;
    }

    .pattern-card {
      border-radius: 14px;
      border: 1px solid rgba(148, 163, 184, 0.45);
      background: radial-gradient(circle at 0 0, rgba(56, 189, 248, 0.14), transparent 55%),
        linear-gradient(150deg, rgba(15, 23, 42, 0.98), rgba(15, 23, 42, 0.98));
      padding: 9px 9px 10px;
      font-size: 0.82rem;
      position: relative;
      overflow: hidden;
    }

    .pattern-card h4 {
      font-size: 0.88rem;
      margin-bottom: 2px;
    }

    .pattern-tag {
      font-size: 0.72rem;
      color: var(--muted);
      margin-bottom: 4px;
    }

    .pattern-body {
      color: var(--muted);
    }

    .pattern-link {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      font-size: 0.78rem;
      color: #bfdbfe;
      margin-top: 5px;
      text-decoration: none;
    }

    .pattern-link:hover {
      text-decoration: underline;
    }

    .pattern-pill {
      position: absolute;
      top: 7px;
      right: 7px;
      font-size: 0.7rem;
      padding: 2px 6px;
      border-radius: 999px;
      border: 1px solid rgba(96, 165, 250, 0.8);
      background: rgba(15, 23, 42, 0.95);
      color: #bfdbfe;
    }

    .commands-table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 8px;
      font-size: 0.8rem;
    }

    .commands-table th,
    .commands-table td {
      border: 1px solid rgba(51, 65, 85, 0.95);
      padding: 7px 8px;
    }

    .commands-table th {
      background: rgba(15, 23, 42, 0.98);
      color: #e5e7eb;
      font-weight: 550;
      text-align: left;
      font-size: 0.78rem;
    }

    .commands-table td {
      color: var(--muted);
    }

    .commands-table td:first-child {
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas,
        "Liberation Mono", "Courier New", monospace;
      color: #e5e7eb;
      white-space: nowrap;
    }

    .badge-type {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 3px 7px;
      border-radius: 999px;
      font-size: 0.72rem;
      border: 1px solid rgba(129, 140, 248, 0.9);
      background: radial-gradient(circle at 0 0, rgba(129, 140, 248, 0.4), transparent 58%);
      color: #e5e7eb;
    }

    .faq-grid {
      display: grid;
      grid-template-columns: minmax(0, 1.2fr) minmax(0, 1fr);
      gap: 14px;
      margin-top: 8px;
      font-size: 0.82rem;
    }

    .faq-item + .faq-item {
      margin-top: 6px;
    }

    .faq-q {
      font-weight: 550;
      margin-bottom: 2px;
    }

    .faq-a {
      color: var(--muted);
    }

    .footer {
      max-width: 1160px;
      margin: 26px auto 36px;
      padding: 0 18px;
      font-size: 0.78rem;
      color: var(--muted);
      display: flex;
      justify-content: space-between;
      gap: 10px;
      flex-wrap: wrap;
    }

    .footer span {
      opacity: 0.85;
    }

    .footer a {
      color: #93c5fd;
      text-decoration: none;
    }

    .footer a:hover {
      text-decoration: underline;
    }

    @media (max-width: 960px) {
      .hero-grid {
        grid-template-columns: minmax(0, 1fr);
      }

      .hero-main {
        order: 1;
      }

      .hero-side {
        order: 2;
      }

      .steps-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
      }

      .patterns-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
      }

      .two-col {
        grid-template-columns: minmax(0, 1fr);
      }

      .faq-grid {
        grid-template-columns: minmax(0, 1fr);
      }
    }

    @media (max-width: 720px) {
      .site-header-inner {
        padding: 0 14px;
      }

      .nav {
        display: none;
      }

      .shell {
        padding: 0 14px 64px;
      }

      .hero-main {
        padding: 20px 16px 20px;
      }

      .hero-side {
        padding: 14px 14px 16px;
      }

      .section {
        padding: 17px 14px 17px;
      }

      .steps-grid {
        grid-template-columns: minmax(0, 1fr);
      }

      .patterns-grid {
        grid-template-columns: minmax(0, 1fr);
      }
    }
  </style>
</head>
<body>
  <header class="site-header">
    <div class="site-header-inner">
      <div class="brand">
        <div class="brand-logo"></div>
        <div>
          <div class="brand-title">Telegram‑bots</div>
          <div class="brand-sub">Python · Конструктор · Готовые конструкции кода</div>
        </div>
      </div>
      <nav class="nav">
        <a href="#steps" class="nav-link" data-scrollspy>
          <span>01</span> Шаги
        </a>
        <a href="#methods" class="nav-link" data-scrollspy>
          <span>02</span> Python / Без кода
        </a>
        <a href="#patterns" class="nav-link" data-scrollspy>
          <span>03</span> Готовые боты
        </a>
        <a href="#shop" class="nav-link" data-scrollspy>
          <span>04</span> Магазин
        </a>
        <a href="#proposal" class="nav-link" data-scrollspy>
          <span>05</span> Предложка
        </a>
        <a href="#commands" class="nav-link" data-scrollspy>
          <span>06</span> Команды
        </a>
        <a href="#faq" class="nav-link" data-scrollspy>
          <span>07</span> FAQ
        </a>
        <a href="#methods" class="nav-cta">
          <span>Старт</span> с ботом
        </a>
      </nav>
    </div>
  </header>

  <main class="shell">
    <!-- HERO -->
    <section id="intro" class="hero">
      <div class="hero-grid">
        <div class="hero-main">
          <div class="eyebrow">
            <span class="eyebrow-badge">Начало</span>
            <span class="eyebrow-label">Создайте Telegram‑бота без опыта программирования</span>
          </div>
          <h1 class="hero-title">
            Пошаговый сайт‑инструкция
            <br />
            <span>как создать Telegram‑бота</span>
          </h1>
          <p class="hero-subtitle">
            Два способа: <b>через Python</b> и <b>через блочный конструктор (например, robochat.io)</b>.
            Внутри — готовые команды, типовые сценарии, примеры кода и полностью
            разобранные боты: <b>магазин</b>, <b>FAQ</b>, <b>анонимная предложка</b>.
          </p>
          <p class="hero-sub-muted">
            Все шаги расписаны по пунктам. Достаточно просто следовать инструкции — вы дойдёте
            от пустого аккаунта Telegram до работающего бота.
          </p>
          <div class="hero-actions">
            <a href="#steps" class="btn-primary">
              <span class="icon">⚡</span>
              <span>Начать с шага 1 — BotFather</span>
            </a>
            <a href="#methods" class="btn-ghost">
              <span>Сравнить: Python vs конструктор</span>
            </a>
          </div>
          <div class="hero-pills">
            <div class="hero-pill">
              <span>✅</span><strong>Безопасно:</strong> всё по правилам Telegram
            </div>
            <div class="hero-pill">
              <span>📦</span><strong>Готовые решения:</strong> магазин, предложка, FAQ
            </div>
            <div class="hero-pill">
              <span>♿️</span><strong>Для новичков:</strong> не важен ваш опыт
            </div>
          </div>
        </div>

        <aside class="hero-side">
          <div>
            <div class="hero-side-title">Карта пути</div>
            <div class="hero-side-main">Из чего состоит создание Telegram‑бота</div>
            <ul class="hero-step-list">
              <li class="hero-step">
                <div class="hero-step-num">0</div>
                <div>
                  <div class="hero-step-label">Подготовка</div>
                  <div class="hero-step-desc">Telegram‑аккаунт, выбор темы бота, установка Python (по желанию).</div>
                </div>
              </li>
              <li class="hero-step">
                <div class="hero-step-num">1</div>
                <div>
                  <div class="hero-step-label">Регистрация бота</div>
                  <div class="hero-step-desc">Через @BotFather получаете токен и настраиваете команды.</div>
                </div>
              </li>
              <li class="hero-step">
                <div class="hero-step-num">2</div>
                <div>
                  <div class="hero-step-label">Выбор способа</div>
                  <div class="hero-step-desc">Python (гибко) или конструктор robochat.io (быстро, без кода).</div>
                </div>
              </li>
              <li class="hero-step">
                <div class="hero-step-num">3</div>
                <div>
                  <div class="hero-step-label">Готовые сценарии</div>
                  <div class="hero-step-desc">Эхо‑бот, FAQ, мини‑магазин, анонимная предложка и др.</div>
                </div>
              </li>
            </ul>
          </div>
          <div class="hero-side-footer">
            Вы можете <b>начать с конструктора</b>, а когда станет мало — перейти на Python.<br />
            На этой странице разобраны <span class="chip-inline"><span>🧩</span><span>оба подхода</span></span>
            и показано, как повторить готовые решения.
          </div>
        </aside>
      </div>
    </section>

    <!-- ШАГИ -->
    <section id="steps" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Шаг 1–3</div>
          <div class="section-title">
            Общие шаги для любого Telegram‑бота
            <span class="badge">BotFather · база</span>
          </div>
        </div>
        <div class="section-sub">
          Эти шаги нужны всегда — независимо от того, делаете вы бота на Python или на конструкторе.
        </div>
      </div>

      <div class="steps-grid">
        <!-- ШАГ 1 -->
        <article class="step-card" data-step="1">
          <div class="step-title">Подготовка: что нужно</div>
          <div class="step-desc">
            Список того, что стоит подготовить до начала.
          </div>
          <ul class="step-list">
            <li>Аккаунт Telegram (на телефоне или ПК).</li>
            <li>Идея бота: <i>о чём он</i> и <i>что должен уметь</i> (FAQ, магазин, предложка и т.п.).</li>
            <li>Для Python‑варианта:
              <br />— установленный <b>Python 3.10+</b>;
              <br />— удобный редактор (например, VS Code);
              <br />— базовое понимание «запустить команду в терминале».
            </li>
            <li>Для конструктора: браузер (Chrome/Opera GX), готовность войти через Telegram.</li>
          </ul>
        </article>

        <!-- ШАГ 2 -->
        <article class="step-card" data-step="2">
          <div class="step-title">Создаём бота в BotFather</div>
          <div class="step-desc">
            Официальный бот Telegram, через которого регистрируются все другие боты.
          </div>
          <ol class="step-list">
            <li>Откройте в Telegram бота <b>@BotFather</b>.</li>
            <li>Нажмите <b>Start</b> или введите команду <code>/start</code>.</li>
            <li>Отправьте команду <code>/newbot</code>.</li>
            <li>В ответ Telegram попросит:
              <br />— <b>имя бота</b> (отображается всем, можно по‑русски);
              <br />— <b>username</b> (латиница, должен заканчиваться на <code>bot</code>, например
              <code>my_shop_helper_bot</code>).
            </li>
            <li>BotFather пришлёт <b>токен</b> формата
              <code>123456789:AA...Z</code> — это «ключ» к вашему боту.
              <br /><b>Сохраните его и никому не показывайте.</b>
            </li>
          </ol>
          <div class="callout" style="margin-top:6px;">
            <b>Важно:</b> токен даёт полный контроль над ботом. Никогда не публикуйте его открыто.
          </div>
        </article>

        <!-- ШАГ 3 -->
        <article class="step-card" data-step="3">
          <div class="step-title">Оформляем бота у BotFather</div>
          <div class="step-desc">
            Настраиваем описание, аватар и список команд.
          </div>
          <ul class="step-list">
            <li><b>Описание (bio):</b>
              <br />Команда <code>/setdescription</code> → выберите бота → введите краткое описание (1–2 предложения).
            </li>
            <li><b>О себе (about):</b>
              <br />Команда <code>/setabouttext</code> — текст, который видно в профиле бота.
            </li>
            <li><b>Аватар:</b>
              <br />Команда <code>/setuserpic</code> → отправьте картинку (квадрат, лучше 512×512).</li>
            <li><b>Список команд:</b> (то, что видно при вводе <code>/</code>)
              <br />Команда <code>/setcommands</code> → выберите бота → отправьте список:
            </li>
          </ul>
          <div class="code-block" data-lang="BotFather">
            <pre><code>start - Запустить бота
help  - Как пользоваться ботом
shop  - Открыть магазин
faq   - Частые вопросы
offer - Отправить анонимное предложение</code></pre>
          </div>
        </article>
      </div>

      <div class="two-col" style="margin-top:14px;">
        <div class="callout">
          <b>Дальше развилка:</b> у вас уже есть зарегистрированный бот и токен.
          Теперь выберите путь:
          <ul style="margin:4px 0 0 1em;">
            <li><b>Python:</b> максимум гибкости, можно сделать что угодно, от игр до сложных магазинов.</li>
            <li><b>Конструктор (robochat.io и аналоги):</b> минимальный вход, собираете блоки как «лего».</li>
          </ul>
        </div>
        <div style="font-size:0.82rem; color:var(--muted);">
          Вы можете <b>начать с конструктора</b>, чтобы быстро «пощупать» идею, а потом повторить логику на Python.
          Внизу есть <b>одни и те же сценарии</b> реализованные обоими способами.
        </div>
      </div>
    </section>

    <!-- PYTHON / NO-CODE -->
    <section id="methods" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Шаг 4</div>
          <div class="section-title">
            Два пути: Python и блочный конструктор
            <span class="badge">Выберите свой уровень</span>
          </div>
        </div>
        <div class="section-sub">
          Ниже — два подробных сценария. Можно прочитать оба или сразу перейти к тому, который ближе.
        </div>
      </div>

      <div class="tabs">
        <div class="tab-buttons">
          <button class="tab-button is-active" data-tab-target="#tab-python">
            <span class="icon">🐍</span>
            <span>Через Python (Pycharm/Visual Studio Code)</span>
          </button>
          <button class="tab-button" data-tab-target="#tab-nocode">
            <span class="icon">🧩</span>
            <span>Без кода (конструктор robochat.io)</span>
          </button>
        </div>
        <div class="tab-panels">
          <!-- PYTHON TAB -->
          <div class="tab-panel is-active" id="tab-python">
            <h4>Путь 1: бот на Python (Visual Studio Code)</h4>
            <p>
              Подойдёт, если вы хотите максимальную гибкость, интеграции, собственную логику.
            </p>
            <ol>
              <li>
                <b>Устанавливаем Python.</b>
                <ul>
                  <li>Скачайте с <a href="https://www.python.org/downloads/" target="_blank" rel="noopener noreferrer">python.org</a> (версии 3.10+).</li>
                  <li>При установке на Windows отметьте галочку <b>«Add Python to PATH»</b>.</li>
                </ul>
              </li>
              <li>
                <b>Установим PyCharm.</b>
                <ul>
                   <li>Скачайте с <a href="https://github.com/danilfg/pycharm" target="_blank" rel="noopener noreferrer">github.com</a> (версия Windows).</li>
                   <li>При установке на Windows отметьте все галочки.</li>
                </ul>
              </li> 
              <li>
                <b>Создаём папку проекта.</b>
                <ul>
                  <li>Создайте новую папку, например <code>telegram-bot</code>.</li>
                  <li>Откройте её в PyCharm (или в любом другом похожем редакторе).</li>
                </ul>
              </li>
              <li>
                <b>Создаём файл кода и ставим библиотеку pytelegrambotapi или python-telegram-bot последней версии.</b>
                <div class="code-block" data-lang="bash">
                  <pre><code># 1. Откройте PyCharm и откройте строку кода.

# 2. Слева снизу на понели будет кнопка Python backages.

# 3. Найдите в поисковике данные библиотеки:
pytelegrambotapi или python-telegram-bot

# 4. Установите любую из них. Например:
pip install pytelegrambotapi</code></pre>
                </div>
              </li>
              <li>
                <b>Пишем простейшего эхо‑бота.</b> Создайте файл <code>bot.py</code> и вставьте код:
                <div class="code-block" data-lang="python">
                  <pre><code><span class="code-comment"># заранее установите и выберете библиотеку для кода</span>
import telebot

TOKEN = "YOUR_TOKEN"

bot = telebot.TeleBot(TOKEN)

@bot.message_handler(func=lambda message: True)
def echo(message):
    # Отправляем обратно тот же текст
    bot.reply_to(message, message.text)

if __name__ == "__main__":
    print("Бот запущен...")
    bot.infinity_polling()</code></pre>
                </div>
                <div class="tab-note">
                  Замените <code><YOUR_TOKEN></code> на токен, который вы получили от BotFather.
                </div>
              </li>
              <li>
                <b>Запускаем бота.</b>
                <div class="code-block" data-lang="bash">
                  <pre><code> python bot.py</code></pre>
                </div>
                <p class="tab-note">
                  Если ошибок нет — откройте вашего бота в Telegram (по username), нажмите <b>Start</b> и
                  попробуйте написать ему текст: он должен отвечать этим же текстом.
                </p>
              </li>
            </ol>

            <hr style="border-color: rgba(31,41,55,0.9); margin:10px 0;" />

            <h4>Добавляем меню и простейший «магазин»</h4>
            <p>Сделаем главное меню с кнопками и пример каталога товаров.</p>

            <div class="code-block" data-lang="python">
              <pre><code>import telebot
from telebot import types

TOKEN = "YOUR_TOKEN"
bot = telebot.TeleBot(TOKEN)

# Главное меню (кнопки под полем ввода)
main_kb = types.ReplyKeyboardMarkup(resize_keyboard=True)
main_kb.add(
    types.KeyboardButton("🛒 Магазин"),
    types.KeyboardButton("❓ FAQ"),
    types.KeyboardButton("✉️ Предложка"),
)


@bot.message_handler(commands=["start"])
def cmd_start(message: types.Message):
    bot.send_message(
        message.chat.id,
        "👋 Привет! Я учебный бот.n"
        "Выберите раздел в меню ниже или напишите любое сообщение.",
        reply_markup=main_kb,
    )


# Каталог товаров — простая витрина
shop_kb = types.InlineKeyboardMarkup(row_width=1)
shop_kb.add(
    types.InlineKeyboardButton(
        text="Курс по Telegram‑ботам — 1000 ₽",
        callback_data="buy_course",
    ),
    types.InlineKeyboardButton(
        text="Консультация 30 минут — 1500 ₽",
        callback_data="buy_call",
    ),
)


@bot.message_handler(commands=["shop"])
@bot.message_handler(func=lambda m: m.text == "🛒 Магазин")
def open_shop(message: types.Message):
    bot.send_message(
        message.chat.id,
        "🛍 Вот пример каталога:n"
        "Выберите один из вариантов ниже.",
        reply_markup=shop_kb,
    )


@bot.callback_query_handler(func=lambda c: c.data and c.data.startswith("buy_"))
def process_buy(callback: types.CallbackQuery):
    # Здесь вы можете подключить оплату (Telegram Payments или ссылки на оплату)
    bot.send_message(
        callback.message.chat.id,
        "Спасибо за интерес! Это пример.n"
        "В реальном боте здесь будет ссылка на оплату или подробности заказа.",
    )
    bot.answer_callback_query(callback.id)


if __name__ == "__main__":
    print("Бот запущен...")
    bot.infinity_polling()</code></pre>
            </div>

            <div class="callout" style="margin-top:8px;">
              <b>Как подключить оплату по‑настоящему:</b>
              у Telegram есть отдельный механизм <b>Telegram Payments</b> (работает через платёжных провайдеров,
              например Stripe, LiqPay и др.). Для продакшена:
              <ul style="margin:4px 0 0 1em;">
                <li>Регистрируете магазин у платёжного провайдера.</li>
                <li>Настраиваете его в боте (модуль <code>sendInvoice</code> / соответствующие методы pytelegrambotapi).</li>
                <li>Обрабатываете успешную оплату и выдаёте товар/доступ.</li>
              </ul>
            </div>
          </div>

          <!-- NO-CODE TAB -->
          <div class="tab-panel" id="tab-nocode">
            <h4>Путь 2: бот без кода (конструктор, например robochat.io)</h4>
            <p>
              Подойдёт, если не хотите писать код: сценарий собирается из блоков («приветствие», «кнопка», «переход»).
              Названия пунктов меню могут немного отличаться между разными конструкторами, но общая логика одинаковая.
            </p>
            <ol>
              <li>
                <b>Создайте аккаунт в конструкторе.</b>
                <ul>
                  <li>Откройте сайт конструктора, например <b>robochat.io</b> (или аналогичный сервис).</li>
                  <li>Зарегистрируйтесь или войдите через Telegram‑аккаунт.</li>
                </ul>
              </li>
              <li>
                <b>Подключите Telegram‑бота.</b>
                <ul>
                  <li>Найдите пункт <b>«Добавить бота» / «Подключить Telegram‑бота»</b>.</li>
                  <li>Скопируйте токен, полученный от BotFather, и вставьте в поле токена.</li>
                  <li>Сохраните — платформа должна показать, что бот подключён.</li>
                </ul>
              </li>
              <li>
                <b>Создайте сценарий приветствия.</b>
                <ul>
                  <li>Создайте <b>первый блок</b> (обычно он называется «Старт», «Приветствие» или «Первое сообщение»).</li>
                  <li>В тексте блока напишите:
                    <br />«👋 Привет! Я помогу вам с ... (опишите цель вашего бота)».</li>
                  <li>Добавьте <b>кнопки</b>:
                    <br />— «🛒 Магазин»
                    <br />— «❓ FAQ»
                    <br />— «✉️ Анонимная предложка»</li>
                  <li>Создайте для каждой кнопки отдельный блок и свяжите переходы.</li>
                </ul>
              </li>
              <li>
                <b>Пример ветки «Магазин» в конструкторе.</b>
                <ul>
                  <li>Создайте блок «Магазин» с текстом:
                    <br />«Вот наш каталог товаров» и перечислите 2–3 товара.</li>
                  <li>На каждый товар сделайте кнопку «Купить <название>».</li>
                  <li>У каждой кнопки:
                    <br />— либо открывайте ссылку на оплату (например, на платёжной странице);
                    <br />— либо переходите в блок «Оформление заказа», где попросите имя/контакт.</li>
                </ul>
              </li>
              <li>
                <b>Пример ветки «Анонимная предложка».</b>
                <ul>
                  <li>Создайте блок «Предложка — инструкция»:
                    <br />«Напишите одно сообщение — мы анонимно передадим его админам.»</li>
                  <li>Следующим блоком сделайте <b>ожидание ввода</b> (обычно «Ввод текста пользователя»).</li>
                  <li>После ввода добавьте действие «Отправить сообщение в канал/группу»:
                    <br />— укажите ID канала или группы (куда будут приходить предложения);
                    <br />— в тексте используйте переменную «сообщение пользователя».</li>
                  <li>Ответ пользователю: «Готово! Ваше сообщение отправлено анонимно.»</li>
                </ul>
              </li>
              <li>
                <b>Публикация бота.</b>
                <ul>
                  <li>Сохраните сценарий и нажмите «Опубликовать» / «Запустить бота».</li>
                  <li>Откройте вашего бота в Telegram, нажмите <b>Start</b> — вы увидите приветственное сообщение и кнопки.</li>
                </ul>
              </li>
            </ol>
            <p class="tab-note">
              На разных платформах названия (например, «Блок», «Шаг», «Сцена») могут отличаться, но логика одна:
              <b>сообщение → кнопки → переходы → действия (отправить в канал, спросить текст и т.п.).</b>
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- ГОТОВЫЕ ПАТТЕРНЫ -->
    <section id="patterns" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Готовые решения</div>
          <div class="section-title">
            Типовые боты, которые вы можете собрать прямо сейчас
            <span class="badge">FAQ · Магазин · Предложка</span>
          </div>
        </div>
        <div class="section-sub">
          Эти паттерны работают и в Python, и в конструкторе. Ниже кратко — а на отдельные сценарии
          есть развёрнутые примеры.
        </div>
      </div>

      <div class="patterns-grid">
        <article class="pattern-card">
          <div class="pattern-pill">1</div>
          <h4>Эхо‑бот</h4>
          <div class="pattern-tag">Самый простой старт</div>
          <div class="pattern-body">
            Повторяет любое сообщение пользователя. Нужен, чтобы проверить, что бот
            правильно настроен и умеет отвечать.
          </div>
          <a href="#methods" class="pattern-link">
            Как запустить эхо‑бота на Python →
          </a>
        </article>

        <article class="pattern-card">
          <div class="pattern-pill">2</div>
          <h4>FAQ‑бот</h4>
          <div class="pattern-tag">Ответы на частые вопросы</div>
          <div class="pattern-body">
            Кнопки с разделами и готовые ответы. Идеален для студий, курсов, проектов с типовыми вопросами
            («цены», «доставка», «контакты»).
          </div>
          <a href="#faq-bot" class="pattern-link">
            Пример структуры FAQ →
          </a>
        </article>

        <article class="pattern-card">
          <div class="pattern-pill">3</div>
          <h4>Мини‑магазин</h4>
          <div class="pattern-tag">Каталог и заявки / оплата</div>
          <div class="pattern-body">
            Список товаров, кнопка «Купить», сбор контакта и (по желанию) подключение платежей Telegram
            или внешнего сервиса.
          </div>
          <a href="#shop" class="pattern-link">
            Как сделать магазин →
          </a>
        </article>

        <article class="pattern-card">
          <div class="pattern-pill">4</div>
          <h4>«Предложка»</h4>
          <div class="pattern-tag">Анонимные сообщения</div>
          <div class="pattern-body">
            Пользователь пишет сообщение в личку боту, а бот пересылает его в ваш канал/чат без указания автора.
          </div>
          <a href="#proposal" class="pattern-link">
            Код и логика предложки →
          </a>
        </article>
      </div>
    </section>

    <!-- МАГАЗИН -->
    <section id="shop" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Тип бота: магазин</div>
          <div class="section-title">
            Как сделать Telegram‑бот‑магазин
            <span class="badge-type">Каталог · Заказы · Оплата</span>
          </div>
        </div>
        <div class="section-sub">
          Ниже — общий каркас: сначала логика, потом пример реализации на Python и в конструкторе.
        </div>
      </div>

      <div class="two-col">
        <div>
          <h4 id="shop-logic">1. Логика магазина в Telegram</h4>
          <ol style="margin-left:1em; font-size:0.84rem;">
            <li><b>Главное меню.</b> Кнопка «🛒 Магазин» из стартового сообщения.</li>
            <li><b>Каталог.</b> Список товаров: название, цена, краткое описание.</li>
            <li><b>Действие «Купить».</b> После нажатия:
              <br />— либо открывается <b>ссылка на оплату</b>;
              <br />— либо пользователь отправляет контакт, а вы связываетесь вручную.</li>
            <li><b>Подтверждение.</b> Бот пишет, что заказ принят / оплата прошла.</li>
          </ol>

          <h4 style="margin-top:10px;">2. Вариант на Python</h4>
          <p style="font-size:0.83rem;">
            Основные элементы: <b>инлайн‑кнопки</b>, обработка <code>callback_data</code> и отправка инструкции по оплате.
          </p>

          <div class="code-block" data-lang="python">
            <pre><code><span class="code-comment"># Фрагмент для магазина (добавьте в ваш bot.py)</span>
import telebot
from telebot import types

TOKEN = "YOUR_TOKEN"
bot = telebot.TeleBot(TOKEN)

# Будем хранить товары в словаре (для примера)
PRODUCTS = {
    "course_python": {
        "title": "Курс по Python для Telegram‑ботов",
        "price": "1000 ₽",
        "description": "4 модуля, практика и примеры.",
        "pay_link": "https://example.com/pay/python-course",  # ваша ссылка на оплату
    },
    "consult_call": {
        "title": "Индивидуальная консультация 30 минут",
        "price": "1500 ₽",
        "description": "Разбор вашего проекта, идеи, кода.",
        "pay_link": "https://example.com/pay/call-30",
    },
}


def build_catalog_keyboard() -> types.InlineKeyboardMarkup:
    kb = types.InlineKeyboardMarkup(row_width=1)
    for key, item in PRODUCTS.items():
        kb.add(
            types.InlineKeyboardButton(
                text=f"{item['title']} — {item['price']}",
                callback_data=f"product_{key}",
            )
        )
    return kb


@bot.message_handler(commands=["shop"])
@bot.message_handler(func=lambda m: m.text == "🛒 Магазин")
def shop_entry(message: types.Message):
    bot.send_message(
        message.chat.id,
        "🗄 Наш каталог:nn"
        "Выберите товар ниже, чтобы получить ссылку на оплату.",
        reply_markup=build_catalog_keyboard(),
    )


@bot.callback_query_handler(func=lambda c: c.data and c.data.startswith("product_"))
def product_details(callback: types.CallbackQuery):
    product_key = callback.data.replace("product_", "")
    item = PRODUCTS.get(product_key)
    if not item:
        bot.answer_callback_query(callback.id, "Товар не найден", show_alert=True)
        return

    text = (
        f"📦 {item['title']}n"
        f"Цена: {item['price']}nn"
        f"{item['description']}nn"
        "Для оплаты перейдите по ссылке:n"
        f"{item['pay_link']}"
    )

    bot.send_message(
        callback.message.chat.id,
        text,
        parse_mode="HTML",
    )
    bot.answer_callback_query(callback.id)


if __name__ == "__main__":
    print("Бот запущен...")
    bot.infinity_polling()</code></pre>
          </div>

          <p style="font-size:0.82rem; margin-top:6px;">
            Дальше вы можете перейти к <b>Telegram Payments</b>, когда будете готовы, чтобы оплата
            происходила прямо внутри Telegram (потребуется зарегистрировать магазин у платёжного провайдера).
          </p>
        </div>

        <div>
          <h4>3. Вариант в конструкторе (robochat.io и аналоги)</h4>
          <ol style="margin-left:1em; font-size:0.83rem;">
            <li><b>Блок «Магазин».</b>
              <ul>
                <li>Текст: «Вот наш каталог товаров».</li>
                <li>Добавьте кнопки по товарам: «Курс по Python — 1000 ₽», «Консультация — 1500 ₽».</li>
              </ul>
            </li>
            <li><b>Блок «Детали товара».</b>
              <ul>
                <li>На каждую кнопку — переход в отдельный блок с описанием товара.</li>
                <li>В блоке: название, цена, описание и кнопка «Перейти к оплате».</li>
              </ul>
            </li>
            <li><b>Оплата.</b>
              <ul>
                <li>Простейший вариант: кнопка открывает <b>внешнюю ссылку</b> (страница оплаты, форма, ЮKassa и т.п.).</li>
                <li>Более продвинутый вариант: если конструктор поддерживает платежи Telegram — используйте встроенные блоки оплаты.</li>
              </ul>
            </li>
            <li><b>Подтверждение заказа.</b>
              <ul>
                <li>После оплаты пользователь возвращается к боту (по кнопке «Вернуться в бот»).</li>
                <li>Сделайте блок «Спасибо за оплату», который выдаёт доступ / ссылку на материал.</li>
              </ul>
            </li>
          </ol>

          <div class="callout" style="margin-top:10px;">
            <b>Мини‑магазин без сложной интеграции:</b>
            для первых продаж достаточно связки:
            <br />бот → кнопка товара → страница оплаты → блок «Спасибо, скоро свяжемся».
            Позже вы можете добавить полноценную проверку платежей.
          </div>
        </div>
      </div>
    </section>

    <!-- ПРЕДЛОЖКА -->
    <section id="proposal" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Тип бота: предложка</div>
          <div class="section-title">
            «Предложка» — анонимные сообщения через бота
            <span class="badge-type">Анонимные идеи · Обратная связь</span>
          </div>
        </div>
        <div class="section-sub">
          Пользователи пишут в личку боту, а он пересылает сообщения в ваш канал/чат без указания автора.
        </div>
      </div>

      <div class="two-col">
        <div>
          <h4>1. Логика анонимной «предложки»</h4>
          <ol style="margin-left:1em; font-size:0.84rem;">
            <li>Пользователь заходит к боту и нажимает <code>/offer</code> или кнопку «✉️ Предложка».</li>
            <li>Бот объясняет правила: «Напишите одно сообщение, оно будет анонимно отправлено админам».</li>
            <li>Пользователь пишет текст.</li>
            <li>Бот пересылает этот текст в <b>закрытый канал или чат админов</b> без имени автора.</li>
            <li>Бот отвечает пользователю: «Ваше сообщение отправлено анонимно».</li>
          </ol>

          <h4 style="margin-top:10px;">2. Реализация на Python (отдельный файл предложки)</h4>
          <p style="font-size:0.82rem;">
            Сделаем отдельного бота для предложки.
          </p>

          <div class="code-block" data-lang="python">
            <pre><code>import logging
import telebot
from telebot import types

API_TOKEN = "YOUR_TOKEN"
ADMIN_CHAT_ID = -1001234567890  # ID канала или чата, куда будут приходить предложения

logging.basicConfig(level=logging.INFO)

bot = telebot.TeleBot(API_TOKEN, parse_mode="HTML")


@bot.message_handler(commands=["start"])
def cmd_start(message: types.Message):
    bot.send_message(
        message.chat.id,
        "👋 Это анонимная предложка.n"
        "Отправьте команду /offer, чтобы поделиться идеей или отзывом."
    )


@bot.message_handler(commands=["offer"])
def cmd_offer(message: types.Message):
    bot.send_message(
        message.chat.id,
        "✉️ Напишите одно сообщение с вашим предложением.n"
        "Мы отправим его админам анонимно (без указания вашего имени)."
    )


# Обрабатываем все сообщения, кроме команд
@bot.message_handler(
    content_types=[
        "text", "audio", "photo", "sticker", "video", "video_note",
        "voice", "document", "location", "contact"
    ]
)
def handle_offer(message: types.Message):
    # Чтобы анонимность не ломалась — работаем только в личке с ботом
    if message.chat.type != "private":
        return

    # Не реагируем на команды (/start, /offer и т.п.)
    if message.text and message.text.startswith("/"):
        return

    text = message.text

    if not text:
        bot.send_message(message.chat.id, "Пока я принимаю только текстовые сообщения 🙂")
        return

    # Формируем анонимное сообщение для админов
    admin_text = (
        "💌 Новое анонимное предложениеnn"
        f"{text}"
    )

    bot.send_message(ADMIN_CHAT_ID, admin_text)
    bot.send_message(
        message.chat.id,
        "✅ Готово! Ваше сообщение отправлено анонимно. Спасибо 🙌"
    )


if __name__ == "__main__":
    logging.info("Бот запущен...")
    bot.infinity_polling(skip_pending=True)</code></pre>
          </div>

          <p style="font-size:0.82rem; margin-top:6px;">
            <b>Как узнать ADMIN_CHAT_ID:</b>
            добавьте бота в приватный канал/чат админов, сделайте там любое сообщение и посмотрите ID через
            отдельный сервис/бот (например, боты‑инструменты «Chat ID»). ID канала обычно выглядит как отрицательное число.
          </p>
        </div>

        <div>
          <h4>3. Реализация в конструкторе</h4>
          <ol style="margin-left:1em; font-size:0.83rem;">
            <li><b>Блок «Старт» (или «Предложка»).</b>
              <ul>
                <li>Текст: «Это анонимная предложка. Напишите одно сообщение — и мы увидим его без имени автора».</li>
                <li>Добавьте кнопку «Написать предложение».</li>
              </ul>
            </li>
            <li><b>Блок «Ожидание сообщения».</b>
              <ul>
                <li>Задача блока: принять произвольный текст от пользователя.</li>
                <li>Во многих конструкторах это блок вида «Ожидать сообщение» / «Ввод текста».</li>
              </ul>
            </li>
            <li><b>Действие «Отправить в канал/чат».</b>
              <ul>
                <li>После получения текста добавьте действие «Отправить сообщение в Telegram‑чат/канал».</li>
                <li>Укажите ID канала или группы админов, сформируйте сообщение:
                  <br />«Новое анонимное предложение:\n\n<текст пользователя>».</li>
              </ul>
            </li>
            <li><b>Ответ пользователю.</b>
              <ul>
                <li>Сделайте блок: «Спасибо! Ваше анонимное сообщение отправлено админам».</li>
                <li>При желании дайте ссылку на канал, где публикуются принятые предложения.</li>
              </ul>
            </li>
          </ol>

          <div class="callout" style="margin-top:10px;">
            <b>Главное правило анонимности:</b> не используйте в сообщении переменные, связанные с
            именем/username пользователя. Передавайте в канал только текст сообщения.
          </div>
        </div>
      </div>
    </section>

    <!-- FAQ-БОТ (структура) -->
    <section id="faq-bot" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Тип бота: FAQ</div>
          <div class="section-title">
            Структура бота с частыми вопросами
            <span class="badge-type">Ответы · Навигация по разделам</span>
          </div>
        </div>
        <div class="section-sub">
          Здесь — логика и структура. Реализовать можно аналогично примерам выше (кнопки + ответы).
        </div>
      </div>

      <div class="two-col">
        <div>
          <h4>1. Набор разделов FAQ</h4>
          <ul style="margin-left:1em; font-size:0.83rem;">
            <li>«О нас» — кто вы, чем занимаетесь.</li>
            <li>«Цены» или «Тарифы».</li>
            <li>«Как заказать / как записаться».</li>
            <li>«Гарантии / возврат» (если актуально).</li>
            <li>«Контакты» — ссылки на соцсети, почту, сайт.</li>
          </ul>

          <h4 style="margin-top:10px;">2. Кнопки в боте</h4>
          <p style="font-size:0.83rem;">
            Вариант на Python: используйте <b>ReplyKeyboardMarkup</b> или <b>InlineKeyboardMarkup</b>.
          </p>

          <div class="code-block" data-lang="python">
            <pre><code>import telebot
from telebot import types

API_TOKEN = "YOUR_TOKEN"
bot = telebot.TeleBot(API_TOKEN)

# Фрагмент FAQ — простое меню
faq_kb = types.ReplyKeyboardMarkup(resize_keyboard=True)
faq_kb.add(
    types.KeyboardButton("ℹ️ О нас"),
    types.KeyboardButton("💰 Цены"),
)
faq_kb.add(
    types.KeyboardButton("📦 Как заказать"),
    types.KeyboardButton("📞 Контакты"),
)

# Открыть FAQ по команде /faq
@bot.message_handler(commands=["faq"])
def faq_menu_cmd(message: types.Message):
    bot.send_message(
        message.chat.id,
        "Выберите раздел, чтобы получить подробный ответ:",
        reply_markup=faq_kb,
    )

# Открыть FAQ по нажатию кнопки "❓ FAQ"
@bot.message_handler(func=lambda m: m.text == "❓ FAQ")
def faq_menu_btn(message: types.Message):
    bot.send_message(
        message.chat.id,
        "Выберите раздел, чтобы получить подробный ответ:",
        reply_markup=faq_kb,
    )

@bot.message_handler(func=lambda m: m.text == "ℹ️ О нас")
def faq_about(message: types.Message):
    bot.send_message(
        message.chat.id,
        "Мы команда ... (ваш текст про компанию / проект)."
    )

@bot.message_handler(func=lambda m: m.text == "💰 Цены")
def faq_prices(message: types.Message):
    bot.send_message(
        message.chat.id,
        "Актуальные тарифы:n"
        "— Тариф А — ...n"
        "— Тариф B — ...n"
        "Подробности можно уточнить у менеджера."
    )

# Пример доп. обработчиков для остальных пунктов меню
@bot.message_handler(func=lambda m: m.text == "📦 Как заказать")
def faq_order(message: types.Message):
    bot.send_message(
        message.chat.id,
        "Инструкция по оформлению заказа ..."
    )

@bot.message_handler(func=lambda m: m.text == "📞 Контакты")
def faq_contacts(message: types.Message):
    bot.send_message(
        message.chat.id,
        "Наши контакты: ..."
    )

if __name__ == "__main__":
    bot.infinity_polling()</code></pre>
          </div>
        </div>

        <div>
          <h4>3. FAQ в конструкторе</h4>
          <ol style="margin-left:1em; font-size:0.83rem;">
            <li>Создайте блок «FAQ — меню» с текстом: «Выберите раздел FAQ».</li>
            <li>Добавьте кнопки: «О нас», «Цены», «Как заказать», «Контакты».</li>
            <li>Для каждой кнопки создайте отдельный блок с готовым ответом.</li>
            <li>В конце каждого ответа добавьте кнопку «Вернуться в главное меню» или «Назад к FAQ».</li>
          </ol>

          <div class="callout" style="margin-top:10px;">
            <b>Совет:</b> начните с нескольких ключевых вопросов и постепенно дополняйте FAQ на основе
            реальных запросов пользователей в бот.
          </div>
        </div>
      </div>
    </section>

    <!-- КОМАНДЫ -->
    <section id="commands" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Справочник</div>
          <div class="section-title">
            Готовый набор команд для ваших ботов
            <span class="badge">Подходит для BotFather</span>
          </div>
        </div>
        <div class="section-sub">
          Этот список можно напрямую скопировать в BotFather через <code>/setcommands</code>,
          а также использовать в коде и в конструкторе.
        </div>
      </div>

      <table class="commands-table">
        <thead>
          <tr>
            <th>Команда</th>
            <th>Описание</th>
            <th>Где использовать</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>/start</td>
            <td>Запустить бота, показать приветствие и главное меню.</td>
            <td>Во всех ботах, всегда.</td>
          </tr>
          <tr>
            <td>/help</td>
            <td>Кратко объяснить, что умеет бот и как им пользоваться.</td>
            <td>Во всех ботах.</td>
          </tr>
          <tr>
            <td>/faq</td>
            <td>Открыть раздел с частыми вопросами и ответами.</td>
            <td>Инфо‑боты, поддержка, проекты, магазины.</td>
          </tr>
          <tr>
            <td>/shop</td>
            <td>Открыть каталог товаров / услуг.</td>
            <td>Магазины, продающие боты.</td>
          </tr>
          <tr>
            <td>/offer</td>
            <td>Отправить анонимное предложение / отзыв.</td>
            <td>«Предложки», боты обратной связи.</td>
          </tr>
          <tr>
            <td>/admin</td>
            <td>Вход в административное меню (только для вас / команды).</td>
            <td>Боты, которыми вы управляете (добавление товаров, массовые рассылки и т.п.).</td>
          </tr>
        </tbody>
      </table>

      <div style="margin-top:10px; font-size:0.82rem; color:var(--muted);">
        После настройки команд в BotFather не забудьте, что <b>бот всё равно должен уметь их обрабатывать</b>:
        <br />на Python — через <code>@dp.message_handler(commands=["..."])</code>,
        в конструкторе — через отдельные блоки, привязанные к этим командам.
      </div>
    </section>

    <!-- FAQ -->
    <section id="faq" class="section">
      <div class="section-header">
        <div>
          <div class="section-kicker">Ответы на частые вопросы</div>
          <div class="section-title">
            FAQ по созданию Telegram‑ботов
            <span class="badge">Практические нюансы</span>
          </div>
        </div>
        <div class="section-sub">
          Краткие ответы на вопросы, которые обычно возникают при первом запуске бота.
        </div>
      </div>

      <div class="faq-grid">
        <div>
          <div class="faq-item">
            <div class="faq-q">Нужны ли мне знания программирования?</div>
            <div class="faq-a">
              Нет, если вы идёте через <b>конструктор</b> (robochat.io и аналоги).
              Да, хоть минимальные — если хотите использовать <b>Python</b>. Но на этой странице даётся базовый код,
              который можно просто скопировать и постепенно разбирать.
            </div>
          </div>

          <div class="faq-item">
            <div class="faq-q">Можно ли сделать одного и того же бота и на Python, и в конструкторе?</div>
            <div class="faq-a">
              Логически — да: те же сценарии (FAQ, магазин, предложка) доступны в обоих вариантах. Разница в гибкости:
              Python позволяет делать сложную бизнес‑логику и интеграции; конструктор — быстрее собирать простые цепочки.
            </div>
          </div>

          <div class="faq-item">
            <div class="faq-q">Насколько безопасно использовать токен бота?</div>
            <div class="faq-a">
              Токен — это «пароль» от бота. Храните его в секрете: не публикуйте в открытых репозиториях, не пересылайте
              незнакомым. Если токен утёк, в BotFather есть команда <b>/revoke</b> / <b>/token</b>, чтобы выпустить новый.
            </div>
          </div>
        </div>

        <div>
          <div class="faq-item">
            <div class="faq-q">Что выбрать: конструктор или Python?</div>
            <div class="faq-a">
              <b>Если хотите «просто работающий бот» без углубления в код</b> — начните с конструктора.
              <br /><b>Если планируете расти, автоматизировать и интегрироваться</b> — Python (aiogram) даёт больше свободы.
            </div>
          </div>

          <div class="faq-item">
            <div class="faq-q">Можно ли разместить этот код где‑то (например, на сервере)?</div>
            <div class="faq-a">
              Да. Для постоянной работы бота его скрипт должен быть запущен 24/7 (на сервере, VPS или хостинге).
              На этапе обучения достаточно запускать бот на своём компьютере.
            </div>
          </div>

          <div class="faq-item">
            <div class="faq-q">Что делать дальше?</div>
            <div class="faq-a">
              1) Запустите <b>эхо‑бота</b> на Python или в конструкторе.
              <br />2) Добавьте <b>FAQ</b> и кнопку «Предложка».
              <br />3) Расширяйте функционал: магазин, рассылки, админ‑панель, интеграции.
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer class="footer">
    <span>Если вы сломали бота, НЕ ПИШИТЕ МНЕ, это не новость. У вас кривые руки. Смиритесь. Ломается он за частую просто так потому что хочет, так всегда.</span>
    <span>Всё спрогано через: Telegram Bot API · Python · блочные конструкторы (robochat.io и др.)</span>
  </footer>

  <script>
    // Переключение табов (Python / конструктор)
    (function () {
      var tabButtons = document.querySelectorAll(".tab-button");
      var tabPanels = document.querySelectorAll(".tab-panel");

      tabButtons.forEach(function (btn) {
        btn.addEventListener("click", function () {
          var targetSelector = btn.getAttribute("data-tab-target");
          var target = document.querySelector(targetSelector);
          if (!target) return;

          tabButtons.forEach(function (b) {
            b.classList.remove("is-active");
          });
          tabPanels.forEach(function (p) {
            p.classList.remove("is-active");
          });

          btn.classList.add("is-active");
          target.classList.add("is-active");
        });
      });
    })();

    // Подсветка пунктов навигации при скролле
    (function () {
      var navLinks = document.querySelectorAll(".nav-link[data-scrollspy]");
      var sections = [];

      navLinks.forEach(function (link) {
        var href = link.getAttribute("href");
        if (href && href.charAt(0) === "#") {
          var sec = document.querySelector(href);
          if (sec) sections.push({ id: href, el: sec });
        }
      });

      function onScroll() {
        var scrollPos = window.scrollY;
        var currentId = null;

        sections.forEach(function (section) {
          var rect = section.el.getBoundingClientRect();
          var offsetTop = rect.top + window.scrollY - 90; // небольшое смещение
          if (scrollPos >= offsetTop) {
            currentId = section.id;
          }
        });

        navLinks.forEach(function (link) {
          var href = link.getAttribute("href");
          if (href === currentId) {
            link.classList.add("is-active");
          } else {
            link.classList.remove("is-active");
          }
        });
      }

 window.addEventListener("scroll", onScroll);
      onScroll();
    })();
  </script>
</body>
</html>
