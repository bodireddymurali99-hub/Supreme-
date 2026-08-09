# Supreme-import discord
from discord.ext import commands

TOKEN = "YOUR_BOT_TOKEN"

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(
    command_prefix="$",
    intents=intents
)


@bot.event
async def on_ready():
    print(f"✅ {bot.user} is ONLINE!")


@bot.command()
async def ping(ctx):
    await ctx.send("🏓 Pong!")


@bot.command()
async def join(ctx):
    if not ctx.author.voice:
        await ctx.send("❌ Join a voice channel first!")
        return

    channel = ctx.author.voice.channel

    if ctx.voice_client:
        await ctx.voice_client.move_to(channel)
    else:
        await channel.connect()

    await ctx.send(f"🎧 Joined **{channel.name}**!")


@bot.command()
async def leave(ctx):
    if not ctx.voice_client:
        await ctx.send("❌ I'm not in a voice channel!")
        return

    await ctx.voice_client.disconnect()
    await ctx.send("👋 Left the voice channel!")


bot.run(TOKEN)