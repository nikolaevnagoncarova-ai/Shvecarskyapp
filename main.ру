import os
import asyncio
import logging
from aiohttp import web, ClientSession
from aiogram import Bot, Dispatcher, types
from aiogram.filters import Command
from aiogram.types import BotCommand, InlineKeyboardMarkup, InlineKeyboardButton, WebAppInfo

BOT_TOKEN = os.getenv("BOT_TOKEN")
# Твоя ссылка на GitHub Pages:
MINI_APP_URL = "https://nikolaevnagoncarova-ai.github.io/Shvecarskyapp/"

bot = Bot(token=BOT_TOKEN)
dp = Dispatcher()

async def set_bot_commands(bot: Bot):
    commands = [
        BotCommand(command="start", description="Запустить бота"),
        BotCommand(command="app", description="Открыть портфолио"),
    ]
    await bot.set_my_commands(commands)

def start_kb():
    return InlineKeyboardMarkup(inline_keyboard=[
        [
            InlineKeyboardButton(
                text="🚀 Открыть портфолио shvecarsky", 
                web_app=WebAppInfo(url=MINI_APP_URL)
            )
        ]
    ])

@dp.message(Command("start"))
async def start_handler(message: types.Message):
    await message.answer(
        "👋 **Привет! Я бот-визитка shvecarsky.**\n\n"
        "Нажми на кнопку ниже, чтобы запустить моё космическое портфолио прямо внутри Telegram!",
        reply_markup=start_kb(),
        parse_mode="Markdown"
    )

@dp.message(Command("app"))
async def app_handler(message: types.Message):
    await message.answer(
        "✨ Нажми для открытия Mini App:",
        reply_markup=start_kb()
    )

async def handle_ping(request):
    return web.Response(text="Bot-Portfolio is active.")

async def self_ping():
    await asyncio.sleep(10)
    port = os.getenv("PORT", "8080")
    render_url = os.getenv("RENDER_EXTERNAL_URL", f"http://127.0.0.1:{port}")
    async with ClientSession() as session:
        while True:
            try:
                async with session.get(render_url) as resp: pass
            except: pass
            await asyncio.sleep(600)

async def main():
    logging.basicConfig(level=logging.INFO)
    await set_bot_commands(bot)
    
    app = web.Application()
    app.router.add_get("/", handle_ping)
    runner = web.AppRunner(app)
    await runner.setup()
    
    port = int(os.getenv("PORT", 8080))
    site = web.TCPSite(runner, "0.0.0.0", port)
    await site.start()

    asyncio.create_task(self_ping())
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
