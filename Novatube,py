import telebot
from pytube import YouTube
import os
bot = telebot.TeleBot('Telegram bot Token') # getting it from https://t.me/BotFather after creating a new bot

def markup_(message):
    markup = telebot.types.InlineKeyboardMarkup(row_width=1)
    res144 = telebot.types.InlineKeyboardButton("144p", callback_data="vid144")
    res360 = telebot.types.InlineKeyboardButton("360p", callback_data="vid360")
    res480 = telebot.types.InlineKeyboardButton("480p", callback_data="vid480")
    res720 = telebot.types.InlineKeyboardButton("720p", callback_data="vid720")
    res1080 = telebot.types.InlineKeyboardButton("1080p", callback_data="vid1080")
    markup.add(res144, res360, res480, res720, res1080)
    bot.send_message(message.chat.id, "Please select the resolution:", reply_markup=markup)
    return markup


@bot.message_handler(commands=['youtube'])
def you(message):
    bot.send_message(message.chat.id, 'Please paste the YouTube video URL:')
    bot.register_next_step_handler(message, process_url)
def process_url(message):
    global url
    url = message.text
    markup_(message)


@bot.callback_query_handler(func=lambda call: True)
def callback_data(call):
    if call.message:
        if call.data == "vid144":
            download_vid(call.message, '144p')
        elif call.data == "vid360":
            download_vid(call.message, '360p')
        elif call.data == "vid480":
            download_vid(call.message, '480p')
        elif call.data == "vid720":
            download_vid(call.message, '720p')
        elif call.data == "vid1080":
            download_vid(call.message, '1080p')
def download_vid(message, resolution):
    yt = YouTube(url)
    video_title = yt.title
    bot.send_message(message.chat.id, f"Downloading {video_title} in {resolution} resolution...")
    stream = yt.streams.filter(res=resolution).first()
    if stream:
        stream.download()
        bot.send_message(message.chat.id, f"Downloading .....")
        bot.send_message(message.chat.id, f"{video_title} downloaded successfully")
        with open(stream.default_filename, 'rb') as video:
            bot.send_video(message.chat.id, video, caption=f"Download completed: {video_title} in {resolution}")
        os.remove(stream.default_filename)
    else:
        bot.send_message(message.chat.id, f"No video found in {resolution} resolution.")

bot.polling()
