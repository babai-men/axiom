<p align="center">
  <img src="axiom-logo.svg" alt="AXIOM Protocol Logo" width="120" height="120">
</p>

<h1 align="center">AXIOM Protocol ($AXIOM)</h1>

<p align="center">
  <b>Autonomous DeFi Liquidity & Tokenomics Flywheel on Solana</b>
</p>

<p align="center">
  <img src="axiom-banner.svg" alt="AXIOM Protocol Banner" width="100%">
</p>

<p align="center">
  <a href="https://solana.com"><img src="https://img.shields.io/badge/Solana-Token--2022-3772FF?style=for-the-badge&logo=solana" alt="Solana Token-2022"></a>
  <a href="#-токеномика-и-механика"><img src="https://img.shields.io/badge/Transfer_Fee-0.05%25_Fixed-FF7A59?style=for-the-badge" alt="Transfer Fee"></a>
  <a href="https://www.anchor-lang.com"><img src="https://img.shields.io/badge/Anchor-v0.30+-C9A227?style=for-the-badge" alt="Anchor"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-82E6AB?style=for-the-badge" alt="License"></a>
</p>

---

## 📌 О проекте

**AXIOM Protocol** — это полностью автономный DeFi-протокол нового поколения на блокчейне Solana, использующий расширение **Token-2022 Transfer Hook**. Протокол взимает фиксированную нативную комиссию 0.05% с каждой транзакции, создавая непрерывный дефляционный маховик сжигания и стимуляции публичных исполнителей (Crank Executors).

---

## 🎨 Графические ресурсы бренда (Brand Assets)

В репозитории используются следующие варианты визуализации для поддержки браузеров, социалок и криптовалютных кошельков:

| Файл | Формат | Превью | Назначение |
| :--- | :--- | :---: | :--- |
| `axiom-logo.svg` | Вектор (SVG) | <img src="axiom-logo.svg" width="48" height="48" alt="SVG Logo"> | Шапка сайта (Navbar), адаптивный векторный интерфейс, масштабируемые dApp-компоненты |
| `axiom-logo.png` | Растр (PNG) | <img src="axiom-logo.png" width="48" height="48" alt="PNG Logo"> | Фавиконка браузера, иконка токена в Phantom / Solflare, CoinGecko, Telegram-боты |
| `axiom-banner.svg` | Вектор (SVG) | *(см. баннер вверху)* | Карточка репозитория GitHub, OpenGraph preview, презентационные соцсети |

---

## 💡 Токеномика и механика

Каждый перевод токенов $AXIOM удерживает фиксированную комиссию **0.05%**, которая собирается в нативном хранилище контракта. Автоматический вызов функции `harvest` (Crank) распределяет собранные средства по фиксированной пропорции:

```mermaid
graph TD
    A[Транзакция перевода $AXIOM] -->|Нативная комиссия 0.05%| B(Token-2022 Fee Vault)
    B -->|Публичный вызов Crank| C{AXIOM Distribution Hook}
    C -->|70%| D[🔥 Burn Account / Окончательное сжигание]
    C -->|20%| E[⚡ Public Bounty / Награда вызвавшему Crank]
    C -->|10%| F[🛡️ Dev Treasury / Развитие и инфраструктура]
