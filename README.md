# Gerador-de-Senha
Gerador de Senhas com Interface, Exportação e Copiar para Área de Transferência
import secrets
import string
import csv
from datetime import datetime
import tkinter as tk
from tkinter import messagebox


def gerar_senha(tamanho):
    """Gera uma senha segura com o tamanho informado."""
    if tamanho < 4:
        raise ValueError("O tamanho da senha deve ser pelo menos 4.")
    caracteres = string.ascii_letters + string.digits + string.punctuation
    return ''.join(secrets.choice(caracteres) for _ in range(tamanho))


def salvar_csv(senha):
    """Salva a senha gerada com data/hora no arquivo CSV."""
    with open("senhas_geradas.csv", mode="a", newline='', encoding='utf-8') as arquivo:
        writer = csv.writer(arquivo)
        writer.writerow([datetime.now().strftime("%Y-%m-%d %H:%M:%S"), senha])


def gerar_e_exibir():
    """Executa a geração da senha, copia para a área de transferência e exibe."""
    try:
        tamanho = int(entrada_tamanho.get())
        senha = gerar_senha(tamanho)
        resultado_var.set(senha)
        janela.clipboard_clear()
        janela.clipboard_append(senha)
        salvar_csv(senha)
        messagebox.showinfo("Senha gerada", "Senha copiada para a área de transferência!")
    except ValueError as e:
        messagebox.showerror("Erro", str(e))


# Interface Tkinter
janela = tk.Tk()
janela.title("Gerador de Senhas Seguras")

tk.Label(janela, text="Tamanho da senha:").grid(row=0, column=0, padx=10, pady=10)
entrada_tamanho = tk.Entry(janela)
entrada_tamanho.insert(0, "12")
entrada_tamanho.grid(row=0, column=1, padx=10)

btn_gerar = tk.Button(janela, text="Gerar Senha", command=gerar_e_exibir)
btn_gerar.grid(row=1, column=0, columnspan=2, pady=10)

resultado_var = tk.StringVar()
tk.Label(janela, text="Senha Gerada:").grid(row=2, column=0)
resultado_label = tk.Entry(janela, textvariable=resultado_var, width=30)
resultado_label.grid(row=2, column=1)

janela.mainloop()
