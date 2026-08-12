# Telegram order bot – setup (Railway via GitHub)

## 1. Bot aanmaken
1. Praat met **@BotFather** op Telegram → `/newbot` → volg de stappen → je krijgt een `BOT_TOKEN`.

## 2. Jouw eigen chat id vinden
1. Praat met **@userinfobot** (of @RawDataBot) → hij toont jouw numerieke `id`. Dat is je `OWNER_CHAT_ID`.

## 3. Code naar GitHub pushen
1. Maak een nieuwe (private) repo op GitHub, bv. `telegram-order-bot`.
2. Zet `bot.py`, `requirements.txt` en `Procfile` in die repo en push:
```
git init
git add bot.py requirements.txt Procfile
git commit -m "order bot"
git branch -M main
git remote add origin https://github.com/JOUWGEBRUIKER/telegram-order-bot.git
git push -u origin main
```

## 4. Railway project aanmaken
1. Ga naar railway.app → **New Project → Deploy from GitHub repo** → kies je repo.
2. Railway detecteert het `Procfile` automatisch en start de `worker` process.
3. Ga naar het project → **Variables** en voeg toe:
   - `BOT_TOKEN` = jouw token van BotFather
   - `OWNER_CHAT_ID` = jouw numerieke Telegram id
4. Railway herstart de bot automatisch zodra je variabelen opslaat.

## 5. Testen
- Kijk in Railway onder **Deployments → View Logs** of de bot gestart is ("Bot started.").
- Ga naar Telegram, stuur `/start` naar je bot.

## 6. Updates uitrollen
Elke keer je iets aanpast in `bot.py`:
```
git add bot.py
git commit -m "update"
git push
```
Railway deployt automatisch opnieuw.

## Hoe de flow werkt
1. Koper stuurt `/start` → bot vraagt naam → model naam → model link.
2. Bot vraagt "Already paid" of "Need to pay".
   - **Already paid** → direct door naar foto's.
   - **Need to pay** → de koper krijgt te horen dat hij de betaling met jou moet regelen. Jij (owner) krijgt een bericht met een knop **"Confirm payment received"**. Zodra je die indrukt gaat de koper automatisch verder naar de foto's.
3. Koper uploadt 5 foto's, dan 5 video's.
4. Bot genereert een ticket/bevestiging voor de koper, én stuurt alle info + alle foto's/video's naar jouw eigen Telegram-chat (`OWNER_CHAT_ID`).

Sessies worden in het geheugen bijgehouden — als je de bot herstart, verliezen lopende gesprekken hun voortgang (nieuwe kopers beginnen gewoon opnieuw met `/start`).
