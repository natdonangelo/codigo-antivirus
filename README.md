# Código Antivírus
import os
import tkinter as tk
from tkinter import filedialog, messagebox, scrolledtext


assassinaturas_maliciosas = [
    b"malware_pattern_1",  
    b"malware_pattern_2",  
]


def verificar_assinatura(arquivo):
    try:
        with open(arquivo, 'rb') as f:
            dados = f.read()
            for assinatura in assassinaturas_maliciosas:
                if assinatura in dados:
                    return True
        return False
    except Exception as e:
        print(f"Erro ao ler o arquivo {arquivo}: {e}")
        return False




def escanear_diretorio(diretorio):
    arquivos_detectados = []
    for root, dirs, files in os.walk(diretorio):
        for file in files:
            arquivo_completo = os.path.join(root, file)
            if verificar_assinatura(arquivo_completo):
                arquivos_detectados.append(arquivo_completo)
    return arquivos_detectados






def iniciar_interface():
    def selecionar_pasta():
        pasta = filedialog.askdirectory()
        if not os.path.isdir(pasta):
            resultados.insert(tk.END, "Diretório inválido ou não selecionado.\n")
            return


        resultados.delete(1.0, tk.END)
        arquivos_maliciosos = escanear_diretorio(pasta)
        if arquivos_maliciosos:
            resultados.insert(tk.END, "Arquivos suspeitos encontrados:\n\n")
            for arquivo in arquivos_maliciosos:
                resultados.insert(tk.END, f"{arquivo}\n")
        else:
            resultados.insert(tk.END, "Nenhum arquivo suspeito encontrado.\n")


    janela = tk.Tk()
    janela.title("Antivírus")
    janela.geometry("600x400")
   


    label = tk.Label(janela, text="Clique no botão abaixo para selecionar uma pasta:")
    label.pack(pady=10)


    botao = tk.Button(janela, text="Selecionar Pasta", command=selecionar_pasta)
    botao.pack()
    resultados = scrolledtext.ScrolledText(janela, width=70, height=20)
    resultados.pack(pady=10)


    janela.mainloop()




if __name__ == "__main__":
    iniciar_interface()




def main():
    diretorio_para_escanear = input("Digite o caminho do diretório para escanear: ")
    if not os.path.isdir(diretorio_para_escanear):
        print("Diretório não encontrado!")
        return
   
    arquivos_maliciosos = escanear_diretorio(diretorio_para_escanear)
   
    if arquivos_maliciosos:
        print("Arquivos suspeitos encontrados:")
        for arquivo in arquivos_maliciosos:
            print(arquivo)
    else:
        print("Nenhum arquivo suspeito encontrado.")


if __name__ == "__main__":
    main()
