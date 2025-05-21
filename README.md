# Sistema-Filmes
Sistema para controle de dados ( FILMES) Rafael Martins e Gustavo Prates 


#adicionando as bibliotecas 

import customtkinter as ctk
from tkinter import messagebox
from collections import Counter

# mudando a aparência da interface visual

ctk.set_appearance_mode('dark')

# matriz onde salvamos os dados 
memoria = []

# Janela principal, a primeira que ira aparecer
principal = ctk.CTk()
principal.title('Projeto Filmes')
principal.geometry('300x300')

# função para definir janela do cadastro

def janela_cadastro():
    janelacad = ctk.CTkInputDialog(text='Janela de Cadastro', title='Janela de Cadastro')
    janelacad.geometry('300x500')

# função para salvar dados dentro da lista, nome, ano, genero, nota. Logo em seguida essa lista será salva dentro da matriz "memoria".

    def salvar():
        nome = campo_nome.get().upper()
        ano = campo_ano.get()
        if not ano.isdigit() or int(ano) < 1900:
            messagebox.showerror("Erro", "Ano inválido! Use um valor a partir de 1900")
            return

        gener = campo_gener.get().upper()
        nota = float(campo_nota.get())
        if nota <0 or nota >10:
            messagebox.showerror("Erro", "Nota inválida! Use uma nota entre 0 a 10")
            return
        dados = (nome, ano, gener, nota)
        if dados    :
            memoria.append(dados)
            messagebox.showinfo("Sucesso", "Dados salvos com sucesso!")

# comando para limpar dados assim que eu me cadastrar.

        campo_ano.delete(0, 'end')
        campo_nome.delete(0, 'end')
        campo_nota.delete(0, 'end')
        campo_gener.delete(0, 'end')

# LABEL=defenir nome que aparece acima do campo digitavel, ENTRY= Definr o campo digitavel

    label_nome = ctk.CTkLabel(janelacad, text='Nome do Filme:')
    label_nome.pack(pady=10)

    campo_nome = ctk.CTkEntry(janelacad, placeholder_text='Digite o nome do filme')
    campo_nome.pack(pady=10)

    label_ano = ctk.CTkLabel(janelacad, text='Ano de lançamento do Filme:')
    label_ano.pack(pady=10)


    campo_ano = ctk.CTkEntry(janelacad, placeholder_text='Digite o ano de lançamento do filme')
    campo_ano.pack(pady=10)

    label_gener = ctk.CTkLabel(janelacad, text='Gênero do Filme:')
    label_gener.pack(pady=10)

    campo_gener = ctk.CTkEntry(janelacad, placeholder_text='Digite o gênero do filme')
    campo_gener.pack(pady=10)

    label_nota = ctk.CTkLabel(janelacad, text='Nota do Filme:')
    label_nota.pack(pady=10)

    campo_nota = ctk.CTkEntry(janelacad, placeholder_text='Digite a nota do filme')
    campo_nota.pack(pady=10)

    # Botão feito para salvar as informações inseridas nos campos

    botao_salvar = ctk.CTkButton(janelacad, text='Salvar', text_color='white', command=salvar)
    botao_salvar.pack(pady=10)

#Criação de outra janela para listar os filmes 

def janela_listar():
    janelalist = ctk.CTkInputDialog(text='Listar Filmes', title='Listar Filmes')
    janelalist.geometry('300x400')

    label_exibicao = ctk.CTkLabel(janelalist, text=f'Lista dos Filmes: ')
    label_exibicao.pack(pady=10)

#FRAME_SCROLL= usado para ter o sistema deslizante/rolagem da pagina

    frame_scroll = ctk.CTkScrollableFrame(janelalist, width=320, height=300)
    frame_scroll.pack(padx=10, pady=10, fill='both', expand=True)

 #função para classificar o filme pela nota

    def classificar_filme(nota):

        if 0 <= nota <= 3.9:
            return "Ruim"
        elif 4 <= nota <= 6.9:
            return "Regular"
        elif 7 <= nota <= 8.9:
            return "Bom"
        elif 9 <= nota <= 10:
            return "Ótimo"
        else:
            return " "
#  Função para exibir os filmes salvos na janela anterior 
    if memoria:
        for dados in memoria:
            classificacao=classificar_filme(float(dados[3]))
            label_filme=ctk.CTkLabel(frame_scroll, text=f'Nome: {dados[0]} \nAno: {dados[1]} \nGênero: {dados[2]} \nNota: {dados[3]} \nClssificação: {classificacao}')
            label_filme.pack(padx=10, pady=10)
    else:
        label_vazio=ctk.CTkLabel(frame_scroll, text='Lista de filmes vazia.')
        label_vazio.pack(padx=10, pady=10)

#Criando outra janela de busca    

def janela_buscar():
    janelabusca = ctk.CTkInputDialog(text='Buscar Filmes', title='Buscar Filmes')
    janelabusca.geometry('300x400')

    def buscar():
        buscar_nome=campo_bucar.get().upper()
        resultado = None
        for dados in memoria:
            if dados[0] == buscar_nome:
                resultado=dados
                break
        if resultado:
            messagebox.showinfo("Resultado", f"Filme encontrado: \nNome: {resultado[0]} \nAno: {resultado[1]} \nGenêro: {resultado[2]} \nNota: {resultado[3]}")
        else:
            messagebox.showerror("Erro", "Filme não encontrado!")

# LABEL=defenir nome que aparece acima do campo digitavel, ENTRY= Definr o campo digitavel

    label_bucar=ctk.CTkLabel(janelabusca, text='Buscar filme pelo nome')
    label_bucar.pack(padx=10, pady=10)

    campo_bucar=ctk.CTkEntry(janelabusca, placeholder_text='Digite aqui o nome do filme.')
    campo_bucar.pack(padx=10, pady=10)

# criação do butão para buscar os dados cadastrados 

    botaobusca=ctk.CTkButton(janelabusca, text='Buscar', command=buscar)
    botaobusca.pack(padx=10, pady=10)

# criação de mais uma janela "remover"

def janela_remover():
    janelaremover = ctk.CTkInputDialog(text='Remover Filmes', title='Remover Filmes')
    janelaremover.geometry('300x400')

    def remover():
        remover_filme=campo_remover.get().upper()
        for dados in memoria:
            if dados[0]==remover_filme:
                memoria.remove(dados)
                messagebox.showinfo("Sucesso", f"O filme '{remover_filme}' foi removido!")
                campo_remover.delete(0, 'end')
                return
        messagebox.showerror("Erro.", "Filme não encontrado!")

    label_remover  = ctk.CTkLabel(janelaremover, text='Remover filme pelo nome')
    label_remover.pack(padx=10, pady=10)

    campo_remover = ctk.CTkEntry(janelaremover, placeholder_text='Digite aqui o nome do filme.')
    campo_remover.pack(padx=10, pady=10)

    botaoremover = ctk.CTkButton(janelaremover, text='Remover', command=remover)
    botaoremover.pack(padx=10, pady=10)

# criação de mais uma janela "estastisticas"

def janela_estisticas():
    janelaestatistica = ctk.CTkInputDialog(text='Estatisticas', title='Estatisticas')
    janelaestatistica.geometry('300x400')



    qtdd_filmes=len(memoria)

#aqui está sendo feito a media dos filme, SUM=somar os filmes, LEN= dividir pela quantidade. 

    notas=[float(dados[3]) for dados in memoria] if memoria else []
    media_notas=sum(notas)/len(notas) if notas else 0

    generos=[dados[2] for dados in memoria] if memoria else []
    genero_maior=Counter(generos).most_common(1)[0][0] if generos else "Nenhum"

# definir filmes maior que nota 8 será motrado nas estastisticas.

    filmes_maior8=[dados[0] for dados in memoria if float(dados[3])>8]

    label_qtdd = ctk.CTkLabel(janelaestatistica, text=f'Quantidade de filmes salvos: {qtdd_filmes}')
    label_qtdd.pack(padx=10, pady=10)

    label_media = ctk.CTkLabel(janelaestatistica, text=f'Média das notas: {media_notas:.2f}')
    label_media.pack(padx=10, pady=10)

    label_genero = ctk.CTkLabel(janelaestatistica, text=f'Gênero mais cadastrado: {genero_maior}')
    label_genero.pack(padx=10, pady=10)

    label_filmes_maior8 = ctk.CTkLabel(janelaestatistica, text=f'Filmes com nota maior que 8: ')
    label_filmes_maior8.pack(padx=10, pady=10)
    if filmes_maior8:
        for filme in filmes_maior8:
            label_filme=ctk.CTkLabel(janelaestatistica, text=f'- {filme}')
            label_filme.pack(padx=10, pady=10)
    else:
            label_filmes_vazio=ctk.CTkLabel(janelaestatistica, text='Nenhum filme com nota maior que 8.')
            label_filmes_vazio.pack(padx=10, pady=10)



#Criação dos botões da tela inicial

botao_cadastro = ctk.CTkButton(principal, text='Cadastrar Filmes', text_color='white', command=janela_cadastro)
botao_cadastro.pack(pady=10)

botao_listar = ctk.CTkButton(principal, text='Listar Filmes', text_color='white', command=janela_listar)
botao_listar.pack(pady=10)

botao_buscar=ctk.CTkButton(principal, text='Buscar Filmes', text_color='white', command=janela_buscar)
botao_buscar.pack(pady=10)

botao_remover=ctk.CTkButton(principal, text='Remover Filmes', text_color='white', command=janela_remover)
botao_remover.pack(pady=10)

botao_exibir=ctk.CTkButton(principal, text='Exibir Filmes', text_color='white', command=janela_estisticas)
botao_exibir.pack(pady=10)

#LOOP=Rodar o sistemna.

principal.mainloop()
