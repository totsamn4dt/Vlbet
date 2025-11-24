from aiogram import Bot, Dispatcher, executor, types

API_TOKEN = 'YOUR_BOT_TOKEN'  # ← сю8563190809:AAE6_DqU46OErhc15p_65UEg61kQIDydcs4да вставь свой токен

bot = Bot(token=API_TOKEN)
dp = Dispatcher(bot)

# Временное хранилище данных
users = {}

@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    user_id = message.from_user.id
    if user_id not in users:
        users[user_id] = {'balance': 1000, 'bets': []}
    await message.answer("Добро пожаловать в VelBet!\nВыберите действие:",
                         reply_markup=main_menu())

def main_menu():
    buttons = [
        [types.KeyboardButton("Посмотреть матчи")],
        [types.KeyboardButton("Сделать ставку")],
        [types.KeyboardButton("Мой баланс")]
    ]
    return types.ReplyKeyboardMarkup(keyboard=buttons, resize_keyboard=True)

@dp.message_handler(lambda message: message.text == "Мой баланс")
async def check_balance(message: types.Message):
    user_id = message.from_user.id
    balance = users.get(user_id, {}).get('balance', 0)
    await message.answer(f"Ваш баланс: {balance}₽")

@dp.message_handler(lambda message: message.text == "Посмотреть матчи")
async def show_matches(message: types.Message):
    matches = "1. Манчестер Юнайтед vs Челси — 2.1 / 3.2 / 2.9\n2. Реал Мадрид vs Барселона — 2.5 / 3.0 / 2.4"
    await message.answer(f"Доступные матчи:\n{matches}")

@dp.message_handler(lambda message: message.text == "Сделать ставку")
async def place_bet(message: types.Message):
    await message.answer("Пока что функция в разработке. Скоро добавим возможность делать ставки!")

if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
