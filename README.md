# stockbot
Stock bot for Discord

import os
import discord
import requests
from bs4 import BeautifulSoup

# Carregue as informações de autenticação do bot
TOKEN = os.getenv("TOKEN")
# URL para verificar o stock de GPU's
GPU_URL = "https://www.example.com/gpus"

# Função para verificar o stock de GPU's
def check_gpu_stock():
    # Faça uma solicitação GET para a URL da GPU
    response = requests.get(GPU_URL)

    # Crie um objeto BeautifulSoup para analisar o HTML da resposta
    soup = BeautifulSoup(response.text, "html.parser")
    # Encontre o elemento que contém o stock de GPU's
    gpu_stock_element = soup.find("div", {"class": "gpu-stock"})

    # Extrai o texto do elemento de stock de GPU's
    gpu_stock_text = gpu_stock_element.text

    # Verifique se o stock de GPU's está disponível
    if "Disponível" in gpu_stock_text:
        return True
    else:
        return False

# Crie um cliente do Discord
client = discord.Client()

# Função para enviar uma mensagem de alerta no canal desejado
def send_alert_message(channel, message):
    # Envie a mensagem de alerta no canal especificado
    channel.send(message)

# Função principal do bot
@client.event
async def on_ready():
    # Imprima uma mensagem no console indicando que o bot está pronto
    print(f"Estou pronto! {client.user}")
# Função para verificar o stock de GPU's e enviar uma mensagem de alerta
async def check_gpu_stock_and_send_alert():
    # Verifique se o stock de GPU's está disponível
    if check_gpu_stock():
        # Obtenha o canal onde a mensagem de alerta será enviada
        channel = client.get_channel(<ID_DO_CANAL>)

        # Envie a mensagem de alerta
        await send_alert_message(channel, "Stock de GPU's disponível!")
# Inicie o bot e verifique o stock de GPU's a cada 5 minutos
@client.event
async def on_ready():
    while True:
        await check_gpu_stock_and_send_alert()
        await asyncio.sleep(300)
