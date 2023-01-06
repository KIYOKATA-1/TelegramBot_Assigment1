# TelegramBot_Assigment1
My Assignment Python:


import telebot
import asyncio
import emoji
#import Created by Pavel
from emoji import emojize
from aiogram import types, Dispatcher, Bot
from aiogram.contrib.fsm_storage.memory import MemoryStorage
from aiogram.utils import executor 
from aiogram.types import ReplyKeyboardRemove, \
    ReplyKeyboardMarkup, KeyboardButton, \
    InlineKeyboardMarkup, InlineKeyboardButton

db = []
TOKEN = '5969254931:AAF8Ixff17xQCdf9f8tvuptIhkyVhvlx4Bs'
bot = Bot(token = TOKEN)
storage= MemoryStorage()
dp = Dispatcher(bot, storage=storage)
counter = 0
studentID = 1
smth = 0
student = []

btn1 = InlineKeyboardButton('Sort by name:🧬', callback_data='sort')
btn2 = InlineKeyboardButton('Find Name by using AGE:🔍', callback_data='age')
btn3 = InlineKeyboardButton(' Information:📁', callback_data='inf')
btn4 = InlineKeyboardButton('Exit:⛔', callback_data='exit')

keyboard = ReplyKeyboardMarkup(resize_keyboard=True)
keyboard.row(btn1, btn2, btn3, btn4)

@dp.message_handler(commands=['start'])
async def start(message: types.Message):
    await message.reply((f'<b>Hello My Friend</b> 👋'), parse_mode='html', reply=False)
    await message.reply("<b>How many student record you want to store?</b>", parse_mode='html', reply=False)

@dp.message_handler()
async def echo_message(msg: types.Message):
    global counter, studentID, smth, student, db
    if counter == 100:
        return
    if msg.text == 'Sort by name:🧬':
        db.sort()
        mess = ''
        for student in db:
            mess += f'{student[0]} {student[1]}\n{student[2]} y.o, {student[3]}\n\n'
        await msg.reply(f'<b>{mess}</b>', parse_mode='html', reply=False, reply_markup=keyboard)
    elif msg.text == 'Find Name by using AGE:🔍':
        await msg.reply("<b>Input age</b>", parse_mode='html', reply=False)
        counter = 20
    elif msg.text == 'Information:📁':
        mess = ''
        for student in db:
            mess += f'{student[0]} {student[1]}\n{student[2]} y.o., {student[3]}\n\n'
        await msg.reply(f'<b>{mess}</b>', parse_mode='html', reply=False, reply_markup=keyboard)
    elif msg.text == 'Exit:⛔':
        await msg.reply('<b>STOPPED</b>🔴\n\n<b>GOOD BYE</b>❤️❤️❤️', parse_mode='html', reply=False, reply_markup=keyboard)
        counter = 100
    elif counter == 20:
        for student in db:
            if student[2] == msg.text:
                await msg.reply(f'<b>{student[0]} {student[1]}, {student[3]}</b>', parse_mode='html', reply=False, reply_markup=keyboard)
                return
        else:
            await msg.reply('<b>There are no student with this age</b> 🚷', parse_mode='html', reply=False, reply_markup=keyboard)
        await msg.reply("<b>Input age</b>", parse_mode='html', reply=False)
        counter = 20
    elif counter == 0:
        smth = int(msg.text)
        student = []
        counter += 1
        await msg.reply("1️⃣<b>What's your firstname?</b>", parse_mode='html', reply=False)
    elif counter == 1:
        student.append(msg.text)
        counter += 1
        await msg.reply("2️⃣<b>What's your lastname?</b>", parse_mode='html', reply=False)
    elif counter == 2:
        student.append(msg.text)
        counter += 1
        await msg.reply("3️⃣<b>How old are you?</b>", parse_mode='html', reply=False)
    elif counter == 3:
        student.append(msg.text)
        counter += 1
        await msg.reply("4️⃣<b>Input residence</b>", parse_mode='html', reply=False)
    elif counter == 4:
        student.append(msg.text)
        db.append(student)
        studentID += 1
        student = []
        counter = 1
        smth -= 1
        await msg.reply("<b>Success✨✨✨</b>", parse_mode='html', reply=False)
        if smth != 0:
            await msg.reply("1️⃣<b>What's your firstname?</b>", parse_mode='html', reply=False)
    if smth == 0:
        counter = -1
        smth = -1
        await msg.reply("<b>Done</b>✅", parse_mode='html', reply=False, reply_markup=keyboard)


if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
    
    #CREATED BY PAVEL 
