---
layout: default
---

# Telegram IOU Bot

Github: https://github.com/sebastienlarivee/Telegram-IOU-Bot

My friends and I use [Telegram](https://telegram.org/) as our messaging app of choice. When travelling we’ll often split expenses and record them in a dedicated group chat. To eliminate the tedious process of having to calculate all the expenses manually I built a bot that uses the [Telegram API](https://core.telegram.org/bots/api), [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot), and [SQLite](https://www.sqlite.org/index.html) to automatically keep track of our expense history and calculate who owes what to who.

Features:

- Creates an SQLite database to record transaction history. Sorts transactions based on group chat ID so the bot can be used in multiple chats simultaneously.
- Accepts transactions in the format: "{name1} owes {name2} {amount} for {optional_reason}”. New names are automatically added to the database as they appear in the request.
- Can also evaluate simple arithmetic inside a transaction request. Ex: “James owes Lin 18/2” will record the owed amount as $9. Nice for splitting expenses.
- /history chat command returns a list of all transactions recorded by the bot for the group.
- /totals chat command returns the sum of what each individual name owes the other names in the group.

pls code snippet

Other considerations:

I originally built the bot using [telegram’s bot menu buttons](https://core.telegram.org/api/bots/menu) instead of requiring users to type out “{name1} owes {name2} {amount}”. But when testing with friends I found that they preferred typing it out as it felt more similar to our original manual tracking.

This is mainly aimed at small groups of friends and doesn’t have the concurrency features required to handle a larger number of users. I’m pretty sure if it received two requests at once bad things would happen.

demo