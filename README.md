from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
import os
import random

# Replace this if you're running locally without env variables
TOKEN = os.getenv("BOT_TOKEN") or "8106915232:AAE9h1C0gVOgmjfFm-RaHRNLewyfuhbTwi4" 

guides = {
    "aviator": "✈️ Aviator Guide:\n1. Watch the plane.\n2. Cash out before it flies too high.\n3. Strategy: low risk = early cashout.",
    "mines": "💣 Mines Guide:\n1. Choose grid and number of mines.\n2. Click safe tiles.\n3. More tiles = more reward.",
    "thimbles": "🥄 Thimbles Guide:\n1. A ball hides under 1 of 3 cups.\n2. Track the movement.\n3. Choose the right cup.",
    "chicken": "🐔 Chicken Road Guide:\n1. Pick safe tiles.\n2. Avoid hidden bombs.\n3. Cash out anytime!"
}

promo_codes = [
    {"code": "FREE100", "link": "https://mycasino.com", "desc": "100% bonus on first deposit"},
    {"code": "WELCOME50", "link": "https://mycasino.com", "desc": "50 Free Spins for new users"},
    {"code": "LUCKYSPIN", "link": "https://mycasino.com", "desc": "Spin & Win up to $500 bonus"},
]

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("🎰 Welcome to Casino Guide Bot!\nTry /aviator, /mines, /thimbles, /chicken or /promocode")

async def guide_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
    command = update.message.text[1:]  # removes "/"
    guide = guides.get(command, "❌ Guide not found.")
    await update.message.reply_text(guide)

async def promocode(update: Update, context: ContextTypes.DEFAULT_TYPE):
    promo = random.choice(promo_codes)
    msg = f"💰 Here’s your promo code: `{promo['Simo21']}`\n" \
          f"📝 {promo['desc']}\n" \
          f"🔗 Use it here: {promo['link']}"
    await update.message.reply_text(msg, parse_mode="Markdown")

def main():
    app = ApplicationBuilder().token(TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("promocode", promocode))

    for game in guides:
        app.add_handler(CommandHandler(game, guide_handler))

    print("Bot running...")
    app.run_polling()

if __name__ == '__main__':
    main()
