import discord
from discord.ext import commands

# هنا تضيف التوكن الخاص بك
TOKEN = 'MTI5NjA0OTk3NTMzMDg2NTE5NA.GUtQ3f.gEVUBKK0uhVrBgFWHwuu0ybAKsBoYaFcQ4-U9M'

# حدد البوت واستخدم بريفكس (مثلاً !)
intents = discord.Intents.default()
intents.messages = True
bot = commands.Bot(command_prefix='!', intents=intents)

# ضع معرف القناة المخصصة للاقتراحات هنا
SUGGESTION_CHANNEL_ID = 1295453137766973566  # استبدل هذا بمعرف القناة الفعلي

# عندما يقوم أي شخص بإرسال رسالة في القناة المحددة
@bot.event
async def on_message(message):
    # التأكد أن الرسالة ليست من البوت نفسه
    if message.author == bot.user:
        return

    # تحقق أن الرسالة أرسلت في القناة المحددة بالمعرف
    if message.channel.id == SUGGESTION_CHANNEL_ID:
        # الحصول على الاقتراح من محتوى الرسالة
        suggestion = message.content

        # حذف رسالة المستخدم الأصلية
        await message.delete()

        # إنشاء رسالة جديدة تحتوي على الاقتراح
        embed = discord.Embed(title="اقتراح جديد", description=suggestion, color=0x00ff00)
        embed.set_footer(text=f"الاقتراح من: {message.author}")

        # إرسال الرسالة الجديدة
        suggestion_message = await message.channel.send(embed=embed)

        # إضافة ردود الفعل (رياكشن) للقبول والرفض
        await suggestion_message.add_reaction("✅")  # رمز القبول
        await suggestion_message.add_reaction("❌")  # رمز الرفض

# تشغيل البوت
bot.run(TOKEN)
