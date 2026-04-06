---
publish: true
created: 2026-04-06T13:16:00.277+05:00
modified: 2026-04-06T13:16:00.278+05:00
---

# Как настроить раздельную маршрутизацию в Happ VPN?

## Когда нужно

Когда нужен VPN только для заблокированных сайтов (YouTube, ChatGPT, Discord и т.д.), а остальной трафик (Ozon, WB, Яндекс, банки) должен идти напрямую через реальный IP. Экономит скорость и не палит VPN на российских сервисах.

## VPN-профиль (Hysteria 2)

Сервер от Макса Рублёва (Demos Company Ltd., Амстердам):

```
hy2://tokarev:Y32QB2LAHcBYzngna5lTmpDWHCMmyTyP@194.87.216.40:1935?insecure=1&sni=github.com&obfs=salamander&obfs-password=etmKiTEA3rLemevp1WxIsKGeNdin27&pinSHA256=03:D4:2F:9D:39:A9:AB:18:B1:6F:6E:54:0C:6C:5A:0D:E7:3C:D6:C9:0A:28:73:F7:12:F4:0B:B8:6A:A1:69:76#IPv4
```

## Решение — профиль маршрутизации «RU Unblock» (v8)

### Принцип работы

- `GlobalProxy: false` — весь трафик по умолчанию идёт **напрямую** (реальный IP)
- `ProxySites` — перечисленные сайты идут **через VPN**
- `ProxyIp` — IP-диапазоны Telegram и Meta/WhatsApp принудительно через VPN
- `DirectIp` — российские IP + локальные сети принудительно напрямую
- `BlockSites` — блокируется реклама (`geosite:category-ads-all`)
- DNS: remote (для прокси) — Cloudflare DoH, domestic (для прямых) — Яндекс 77.88.8.8

### Ссылка для импорта (скопировать → вставить в Happ)

```
happ://routing/onadd/ewogICAgIk5hbWUiOiAiUlUgVW5ibG9jayIsCiAgICAiR2xvYmFsUHJveHkiOiAiZmFsc2UiLAogICAgIlJlbW90ZUROU1R5cGUiOiAiRG9IIiwKICAgICJSZW1vdGVETlNEb21haW4iOiAiaHR0cHM6Ly9jbG91ZGZsYXJlLWRucy5jb20vZG5zLXF1ZXJ5IiwKICAgICJSZW1vdGVETlNJUCI6ICIxLjEuMS4xIiwKICAgICJEb21lc3RpY0ROU1R5cGUiOiAiRG9VIiwKICAgICJEb21lc3RpY0ROU0RvbWFpbiI6ICIiLAogICAgIkRvbWVzdGljRE5TSVAiOiAiNzcuODguOC44IiwKICAgICJHZW9pcHVybCI6ICJodHRwczovL2dpdGh1Yi5jb20vTG95YWxzb2xkaWVyL3YycmF5LXJ1bGVzLWRhdC9yZWxlYXNlcy9sYXRlc3QvZG93bmxvYWQvZ2VvaXAuZGF0IiwKICAgICJHZW9zaXRldXJsIjogImh0dHBzOi8vZ2l0aHViLmNvbS9Mb3lhbHNvbGRpZXIvdjJyYXktcnVsZXMtZGF0L3JlbGVhc2VzL2xhdGVzdC9kb3dubG9hZC9nZW9zaXRlLmRhdCIsCiAgICAiRG5zSG9zdHMiOiB7CiAgICAgICAgImNsb3VkZmxhcmUtZG5zLmNvbSI6ICIxLjEuMS4xIiwKICAgICAgICAiZG5zLmdvb2dsZSI6ICI4LjguOC44IgogICAgfSwKICAgICJEaXJlY3RTaXRlcyI6IFtdLAogICAgIkRpcmVjdElwIjogWwogICAgICAgICJnZW9pcDpydSIsCiAgICAgICAgIjEwLjAuMC4wLzgiLAogICAgICAgICIxNzIuMTYuMC4wLzEyIiwKICAgICAgICAiMTkyLjE2OC4wLjAvMTYiLAogICAgICAgICIxNjkuMjU0LjAuMC8xNiIsCiAgICAgICAgIjIyNC4wLjAuMC80IiwKICAgICAgICAiMjU1LjI1NS4yNTUuMjU1IgogICAgXSwKICAgICJQcm94eVNpdGVzIjogWwogICAgICAgICJnZW9zaXRlOnlvdXR1YmUiLAogICAgICAgICJnZW9zaXRlOmdvb2dsZSIsCiAgICAgICAgImdlb3NpdGU6ZmFjZWJvb2siLAogICAgICAgICJnZW9zaXRlOnR3aXR0ZXIiLAogICAgICAgICJnZW9zaXRlOmluc3RhZ3JhbSIsCiAgICAgICAgImdlb3NpdGU6bGlua2VkaW4iLAogICAgICAgICJnZW9zaXRlOnRlbGVncmFtIiwKICAgICAgICAiZG9tYWluOmRpc2NvcmQuY29tIiwKICAgICAgICAiZG9tYWluOmRpc2NvcmQuZ2ciLAogICAgICAgICJkb21haW46ZGlzY29yZGFwcC5jb20iLAogICAgICAgICJkb21haW46ZGlzY29yZC5tZWRpYSIsCiAgICAgICAgImRvbWFpbjpkaXNjb3JkYXBwLm5ldCIsCiAgICAgICAgImRvbWFpbjp4LmNvbSIsCiAgICAgICAgImRvbWFpbjp0LmNvIiwKICAgICAgICAiZG9tYWluOnRocmVhZHMubmV0IiwKICAgICAgICAiZG9tYWluOm9wZW5haS5jb20iLAogICAgICAgICJkb21haW46Y2hhdGdwdC5jb20iLAogICAgICAgICJkb21haW46Y2hhdC5vcGVuYWkuY29tIiwKICAgICAgICAiZG9tYWluOm9haXVzZXJjb250ZW50LmNvbSIsCiAgICAgICAgImRvbWFpbjphbnRocm9waWMuY29tIiwKICAgICAgICAiZG9tYWluOmNsYXVkZS5haSIsCiAgICAgICAgImRvbWFpbjpjbGVyay5jb20iLAogICAgICAgICJkb21haW46Y2xlcmsuZGV2IiwKICAgICAgICAiZG9tYWluOmNsZXJrLmFjY291bnRzLmRldiIsCiAgICAgICAgImRvbWFpbjphY2NvdW50cy5kZXYiLAogICAgICAgICJkb21haW46Y2xlcmtqcy5jb20iLAogICAgICAgICJkb21haW46bWlkam91cm5leS5jb20iLAogICAgICAgICJkb21haW46cGVycGxleGl0eS5haSIsCiAgICAgICAgImRvbWFpbjpodWdnaW5nZmFjZS5jbyIsCiAgICAgICAgImRvbWFpbjpzdGFiaWxpdHkuYWkiLAogICAgICAgICJkb21haW46cmVwbGljYXRlLmNvbSIsCiAgICAgICAgImRvbWFpbjpjdXJzb3IuY29tIiwKICAgICAgICAiZG9tYWluOmN1cnNvci5zaCIsCiAgICAgICAgImRvbWFpbjpzdW5vLmNvbSIsCiAgICAgICAgImRvbWFpbjp1ZGlvLmNvbSIsCiAgICAgICAgImRvbWFpbjpjb3BpbG90Lm1pY3Jvc29mdC5jb20iLAogICAgICAgICJkb21haW46bm90aW9uLnNvIiwKICAgICAgICAiZG9tYWluOm5vdGlvbi5jb20iLAogICAgICAgICJkb21haW46bWVkaXVtLmNvbSIsCiAgICAgICAgImRvbWFpbjpxdW9yYS5jb20iLAogICAgICAgICJkb21haW46cGF0cmVvbi5jb20iLAogICAgICAgICJkb21haW46c291bmRjbG91ZC5jb20iLAogICAgICAgICJkb21haW46c3BvdGlmeS5jb20iLAogICAgICAgICJkb21haW46bmV0ZmxpeC5jb20iLAogICAgICAgICJkb21haW46dHdpdGNoLnR2IiwKICAgICAgICAiZG9tYWluOmJiYy5jb20iLAogICAgICAgICJkb21haW46YmJjLmNvLnVrIiwKICAgICAgICAiZG9tYWluOmJiY2kuY28udWsiLAogICAgICAgICJkb21haW46ZHcuY29tIiwKICAgICAgICAiZG9tYWluOnJldXRlcnMuY29tIiwKICAgICAgICAiZG9tYWluOnNpZ25hbC5vcmciLAogICAgICAgICJkb21haW46d2hpc3BlcnN5c3RlbXMub3JnIiwKICAgICAgICAiZG9tYWluOnByb3Rvbi5tZSIsCiAgICAgICAgImRvbWFpbjpwcm90b25tYWlsLmNvbSIsCiAgICAgICAgImRvbWFpbjpwcm90b252cG4uY29tIiwKICAgICAgICAiZG9tYWluOmFyY2hpdmUub3JnIiwKICAgICAgICAiZG9tYWluOnJ1dHJhY2tlci5vcmciLAogICAgICAgICJkb21haW46bm9yZHZwbi5jb20iLAogICAgICAgICJkb21haW46ZXhwcmVzc3Zwbi5jb20iLAogICAgICAgICJkb21haW46d2luZHNjcmliZS5jb20iLAogICAgICAgICJkb21haW46bXVsbHZhZC5uZXQiLAogICAgICAgICJkb21haW46dG9ycHJvamVjdC5vcmciLAogICAgICAgICJkb21haW46bnBtanMuY29tIiwKICAgICAgICAiZG9tYWluOmRvY2tlci5jb20iLAogICAgICAgICJkb21haW46ZG9ja2VyLmlvIiwKICAgICAgICAiZG9tYWluOmh1Yi5kb2NrZXIuY29tIiwKICAgICAgICAiZG9tYWluOmdyYW1tYXJseS5jb20iLAogICAgICAgICJkb21haW46Y2FudmEuY29tIiwKICAgICAgICAiZG9tYWluOmZpZ21hLmNvbSIsCiAgICAgICAgImRvbWFpbjp6b29tLnVzIiwKICAgICAgICAiZG9tYWluOnNsaWRlc2hhcmUubmV0IiwKICAgICAgICAiZG9tYWluOmRhaWx5bW90aW9uLmNvbSIsCiAgICAgICAgImRvbWFpbjp2aW1lby5jb20iLAogICAgICAgICJkb21haW46aW1ndXIuY29tIiwKICAgICAgICAiZG9tYWluOmdpdGh1Yi5jb20iLAogICAgICAgICJkb21haW46Z2l0aHViLmlvIiwKICAgICAgICAiZG9tYWluOmdpdGh1YnVzZXJjb250ZW50LmNvbSIsCiAgICAgICAgImRvbWFpbjpnaXRodWJhc3NldHMuY29tIiwKICAgICAgICAiZG9tYWluOmdoY3IuaW8iLAogICAgICAgICJkb21haW46d2hhdHNhcHAuY29tIiwKICAgICAgICAiZG9tYWluOndoYXRzYXBwLm5ldCIsCiAgICAgICAgImRvbWFpbjp3ZWIud2hhdHNhcHAuY29tIiwKICAgICAgICAiZG9tYWluOndhLm1lIiwKICAgICAgICAiZG9tYWluOndhYmEubWUiLAogICAgICAgICJkb21haW46d2lzcHIuYWkiLAogICAgICAgICJkb21haW46d2lzcHJmbG93LmFpIiwKICAgICAgICAiZG9tYWluOmFwaS53aXNwci5haSIKICAgIF0sCiAgICAiUHJveHlJcCI6IFsKICAgICAgICAiMTQ5LjE1NC4xNjAuMC8yMCIsCiAgICAgICAgIjkxLjEwOC40LjAvMjIiLAogICAgICAgICI5MS4xMDguOC4wLzIyIiwKICAgICAgICAiOTEuMTA4LjEyLjAvMjIiLAogICAgICAgICI5MS4xMDguMTYuMC8yMiIsCiAgICAgICAgIjkxLjEwOC4yMC4wLzIyIiwKICAgICAgICAiOTEuMTA4LjU2LjAvMjIiLAogICAgICAgICI5NS4xNjEuNjQuMC8yMCIsCiAgICAgICAgIjE1Ny4yNDAuMC4wLzE2IiwKICAgICAgICAiMzEuMTMuMjQuMC8yMSIsCiAgICAgICAgIjMxLjEzLjY0LjAvMTgiLAogICAgICAgICIxNzkuNjAuMTkyLjAvMjIiLAogICAgICAgICIxODUuNjAuMjE2LjAvMjIiCiAgICBdLAogICAgIkJsb2NrU2l0ZXMiOiBbCiAgICAgICAgImdlb3NpdGU6Y2F0ZWdvcnktYWRzLWFsbCIKICAgIF0sCiAgICAiQmxvY2tJcCI6IFtdLAogICAgIkRvbWFpblN0cmF0ZWd5IjogIklQSWZOb25NYXRjaCIsCiAgICAiRmFrZUROUyI6ICJmYWxzZSIKfQ==
```

### Что идёт через VPN

| Категория      | Сервисы                                                                                            |
| -------------- | -------------------------------------------------------------------------------------------------- |
| Соцсети        | Instagram, Facebook, Twitter/X, LinkedIn, Threads, Discord                                         |
| Видео          | YouTube, Twitch, Vimeo, Dailymotion                                                                |
| AI-сервисы     | ChatGPT, Claude, Midjourney, Perplexity, Copilot, HuggingFace, Cursor, Suno, Udio                  |
| Авторизация AI | Clerk (clerk.com, clerk.dev, accounts.dev, clerkjs.com) — нужен для Chrome-расширения Claude       |
| Голосовые AI   | Wispr Flow (wispr.ai, wisprflow.ai) — облачное распознавание речи                                  |
| Мессенджеры    | Telegram (домены + IP 149.154.x.x, 91.108.x.x), WhatsApp (домены + IP Meta 157.240.x.x, 31.13.x.x) |
| Стриминг       | Spotify, Netflix, SoundCloud                                                                       |
| Инструменты    | Notion, Medium, Figma, Canva, Grammarly, Zoom, Docker Hub, npm                                     |
| Разработка     | GitHub (github.com, githubusercontent.com, githubassets.com, ghcr.io)                              |
| СМИ            | BBC, DW, Reuters                                                                                   |
| Прочее         | Archive.org, Rutracker, Patreon, Signal, Proton Mail, VPN-сайты                                    |

### Что идёт напрямую (реальный IP)

Всё остальное: Ozon, Wildberries, Яндекс, ВК, банки, госуслуги и т.д.

## Решённые проблемы

### Telegram не подключался («Соединение...»)

**Причина:** Google DNS 8.8.8.8 плохо резолвился через Дом.ru при split-tunnel. Позже выяснилось, что Дом.ru также дросселит MTProto-протокол через DPI на IP-диапазонах Telegram.
**Решение:** Сменили domestic DNS на Яндекс 77.88.8.8. Telegram добавлен через `geosite:telegram` + IP-диапазоны серверов в `ProxyIp`.

### WhatsApp не подключался

**Причина:** Дом.ru дросселит IP-подсети Meta (157.240.0.0/16, 31.13.x.x). WhatsApp использует эти IP для соединения.
**Решение:** Добавили домены WhatsApp (whatsapp.com, whatsapp.net, wa.me) в `ProxySites` и IP-диапазоны Meta в `ProxyIp`.

### Wispr Flow — медленное распознавание речи

**Причина:** Wispr Flow обрабатывает аудио в облаке (wispr.ai). Без VPN Дом.ru замедляет соединение до серверов Wispr.
**Решение:** Добавили `wispr.ai`, `wisprflow.ai`, `api.wispr.ai` в `ProxySites`.

### Chrome-расширение Claude не авторизуется

**Причина:** Claude использует Clerk для OAuth. Домен claude.ai шёл через VPN, а домены clerk.com/accounts.dev — напрямую. Сессия разваливалась из-за разных IP.
**Решение:** Добавили домены Clerk в ProxySites: `clerk.com`, `clerk.dev`, `accounts.dev`, `clerkjs.com`.

### Ошибка загрузки гео-данных на мобильном (Happ Plus)

**Причина:** Гео-файлы (geoip.dat, geosite.dat) скачиваются с GitHub, который замедлён/заблокирован РКН. Загрузка происходит до применения маршрутизации → идёт напрямую → таймаут.
**Решение:**

1. Отключить маршрутизацию → подключить VPN без роутинга → импортировать профиль → гео-файлы скачаются через VPN
2. GitHub добавлен в ProxySites, поэтому при следующих обновлениях гео-баз проблема не повторится

## Нюансы

- После импорта профиля нужно **переподключить VPN** — роутинг применяется при подключении
- Domestic DNS — Яндекс `77.88.8.8` (быстрый из Екатеринбурга). Google `8.8.8.8` вызывал проблемы
- Если профиль с тем же именем уже есть — он **перезаписывается** при импорте
- Проверка: зайти на 2ip.ru — должен показать Екатеринбург / Дом.ru
- Чтобы добавить новый сайт: дописать `"domain:example.com"` в `ProxySites`, пересобрать base64, импортировать
- На мобильном первый импорт лучше делать с выключенной маршрутизацией и включённым VPN
- Документация Happ по маршрутизации: https://www.happ.su/main/ru/dev-docs/routing
- Конструктор профилей: https://routing.happ.su/

## История версий

- **v1** — базовый профиль, DNS Google 8.8.8.8
- **v2** — добавлен Telegram в прокси + DNS Яндекс 77.88.8.8
- **v3** — убран Telegram из прокси (тормозил файлы), DNS Яндекс остался
- **v4** — добавлены домены Clerk для авторизации Chrome-расширения Claude
- **v5** — добавлен GitHub для загрузки гео-баз и работы с репозиториями
- **v6** — Telegram вернули в прокси (Дом.ru дросселит MTProto через DPI)
- **v7** — добавлен WhatsApp (домены + IP-подсети Meta) — Дом.ru дросселит Meta IP
- **v8** — добавлен Wispr Flow (wispr.ai, wisprflow.ai) — облачное распознавание речи тормозило

## Источник

Настроено с Claude, из практики 2026-03

---

Теги: #vpn #happ #маршрутизация #инфраструктура
