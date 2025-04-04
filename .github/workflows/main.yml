import asyncio
import logging
import re
from telegram import InlineKeyboardButton, InlineKeyboardMarkup, Update
from telegram.ext import Application, CallbackQueryHandler, CommandHandler, ContextTypes

TOKEN = "7299777546:AAE5nwALrJWYQIshV79S6F6cGzPYcqm0Y2M"
logging.basicConfig(level=logging.INFO)

def calculate(expression):
    try:
        expression = re.sub(r'[^0-9+\-*/().]', '', expression)  # Sanitize input
        return str(eval(expression))
    except Exception as e:
        return "Error in calculation"

def generate_keyboard():
    keyboard = [
        [InlineKeyboardButton("7", callback_data="7"), InlineKeyboardButton("8", callback_data="8"), InlineKeyboardButton("9", callback_data="9"), InlineKeyboardButton("/", callback_data="/")],
        [InlineKeyboardButton("4", callback_data="4"), InlineKeyboardButton("5", callback_data="5"), InlineKeyboardButton("6", callback_data="6"), InlineKeyboardButton("*", callback_data="*")],
        [InlineKeyboardButton("1", callback_data="1"), InlineKeyboardButton("2", callback_data="2"), InlineKeyboardButton("3", callback_data="3"), InlineKeyboardButton("-", callback_data="-")],
        [InlineKeyboardButton(".", callback_data="."), InlineKeyboardButton("0", callback_data="0"), InlineKeyboardButton("DEL", callback_data="DEL"), InlineKeyboardButton("+", callback_data="+")],
        [InlineKeyboardButton("C", callback_data="C"), InlineKeyboardButton("=", callback_data="=")]
    ]
    return InlineKeyboardMarkup(keyboard)

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Welcome to Calculator Bot!\nUse the keyboard below to enter numbers.", reply_markup=generate_keyboard())

async def button_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    data = query.data

    current_text = query.message.text or ""
    expression = current_text.split("\n")[-1] if "Result" not in current_text else ""

    if data == "C":
        await query.edit_message_text("Calculator cleared.", reply_markup=generate_keyboard())
    elif data == "=":
        result = calculate(expression)
        await query.edit_message_text(f"Result: {expression} = {result}", reply_markup=generate_keyboard())
    elif data == "DEL":
        expression = expression[:-1]
        await query.edit_message_text(expression or "0", reply_markup=generate_keyboard())
    else:
        expression += data
        await query.edit_message_text(expression, reply_markup=generate_keyboard())

async def main():
    app = Application.builder().token(TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(CallbackQueryHandler(button_handler))
    await app.run_polling()

if __name__ == "__main__":
    import nest_asyncio
    nest_asyncio.apply()
    asyncio.run(main())
