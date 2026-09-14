<p align="center">
  <img src="axiom-logo.svg" alt="AXIOM Protocol Logo" width="128" height="128">
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
  <a href="#-механика-протокола-и-архитектура"><img src="https://img.shields.io/badge/Transfer_Fee-0.05%25_Fixed-FF7A59?style=for-the-badge" alt="Transfer Fee"></a>
  <a href="https://www.anchor-lang.com"><img src="https://img.shields.io/badge/Anchor-v0.30+-C9A227?style=for-the-badge" alt="Anchor"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-82E6AB?style=for-the-badge" alt="License"></a>
</p>

---

## 📌 Обзоp

**AXIOM Protocol** — это децентрализованный, полностью автономный DeFi-протокол на блокчейне Solana, построенный на стандарте **Token-2022**. Протокол использует встроенное расширение **Transfer Fee Extension** (0.05%) и передает права на изъятие комиссий под управление автономного смарт-контракта через **Program Derived Address (PDA)**.

Архитектура исключает «человеческий фактор» (Human Risk): команда проекта не имеет доступа к сбору или выводу средств. Любой участник сети (MEV-бот, трейдер или скрипт) может инициировать процедуру расщепления налога и получить мгновенное вознаграждение.

---

## ⚡ Механика протокола и архитектура

### 1. Начисление комиссии (0.05%)
При совершении любой транзакции перевода $AXIOM программа Solana Token-2022 автоматически удерживает **0.05%** от суммы перевода. Комиссия задерживается непосредственно на токен-аккаунтах участников сети в виде удерживаемого баланса (`withheld_amount`).

### 2. Децентрализованный авто-сбор (PDA Fee Authority)
Права `withdraw_withheld_authority` навсегда закреплены за PDA смарт-контракта (`seeds = [b"fee_authority", mint_key]`). Это позволяет контракту подписывать транзакции изъятия налога без участия приватных ключей администратора.

### 3. Автоматический распределительный маховик (Volume Flywheel)
Любой пользователь или бот запускает публичную инструкцию `harvest_and_distribute`. В рамках **одной атомарной транзакции** происходят следующие шаги:

1. **Изъятие (Withdraw):** Контракт извлекает накопленные комиссии со всех удержавших их аккаунтов на свой сейф (`contract_vault`).
2. **🔥 Burn (70%):** Контракт выполняет Cross-Program Invocation (CPI) в программу Token-2022 и сжигает 70% собранных токенов, уменьшая общее предложение $AXIOM.
3. **⚡ Public Bounty (20%):** Контракт автоматически переводит 20% от сбора на кошелек аккаунта, вызвавшего функцию (стимул для Crank-ботов).
4. **🛡️ Dev Treasury (10%):** Контракт направляет 10% в резерв для покрытия расходов на инфраструктуру, RPC и Liquidity-бустинг.

---

## 🔄 Схема работы (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    actor Crank as Public Crank Bot / Пользователь
    participant Contract as AXIOM Program (Anchor PDA)
    participant Token2022 as Solana Token-2022 Program
    participant Accounts as User Token Accounts
    participant Vault as Contract Vault
    participant Treasury as Dev Treasury Account

    Note over Accounts, Token2022: Удержание 0.05% при переводах
    Crank->>Contract: Вызов инструкции harvest_and_distribute()
    
    rect rgb(25, 25, 35)
        Note over Contract, Token2022: CPI с подписью PDA (fee_authority)
        Contract->>Token2022: withdraw_withheld_tokens_from_accounts()
        Token2022->>Vault: Перевод удержанных комиссий на сейф
    end

    rect rgb(35, 25, 25)
        Note over Contract, Token2022: 🔥 CPI Burn (70%)
        Contract->>Token2022: burn(70% from Vault)
        Token2022-->>Vault: Токены уничтожены
    end

    rect rgb(25, 35, 25)
        Note over Contract, Crank: ⚡ CPI Transfer (20%)
        Contract->>Token2022: transfer(20% from Vault to Crank)
        Token2022-->>Crank: Мгновенная выплата Bounty
    end

    rect rgb(25, 25, 45)
        Note over Contract, Treasury: 🛡️ CPI Transfer (10%)
        Contract->>Token2022: transfer(10% from Vault to Treasury)
        Token2022-->>Treasury: Пополнение Dev-резерва
    end
