# Conversor de imagens para Desenhos Realistas com Python e OpenCV

**O código em questão utiliza a biblioteca tkinter para criar uma interface gráfica com o usuário (GUI) para um programa que realiza a conversão de uma imagem para uma versão semelhante a um desenho a lápis. A seguir, estão detalhadas as principais funcionalidades do código:**

1. **Importação de bibliotecas:**
O código começa com a importação das bibliotecas necessárias para a execução do programa, que incluem o tkinter (para a criação da GUI), o filedialog (para abrir janelas de diálogo para escolha de arquivos), o PIL (para manipulação de imagens) e o cv2 (OpenCV, para processamento de imagens).

**Configuração para Linux Umbutu:**

```yaml
sudo apt-get install python-pip
```
```yaml
sudo apt-get install python3-tk
```
```yaml
pip3 list -v | grep Pillow
```
```yaml
python3 -m pip install --upgrade Pillow
```
```yaml
sudo apt-get install python3-opencv
```
**É importante destacar que essa aplicação utiliza a biblioteca OpenCV para processamento de imagens e a biblioteca PIL para exibição de imagens na interface gráfica. Também é utilizada a biblioteca Tkinter para criar a interface gráfica em si.**

2. **Crie um diretório com o nome "images" na raiz do seu projeto:**

Exemplo: "/conversor/images"

Dentro do diretório imagens coloque todas as imagem que você quer converter para desenho!

3. **Insira uma imagem com o nome "logo.png" na raiz do projeto pode ser qualquer uma imagem, tem que ser no formatos "png"**

Exemplo: "/conversor/logo.png

4. **Crie um arquivo com o nome "main.py" na raiz do seu projeto: Copie e cole o código abaixo no arquivo, salve e execute o programa."

Exemplo: "/conversor/main.py

```yaml
from tkinter import*
from tkinter import Tk, ttk

from tkinter import filedialog as fd
from PIL import Image, ImageTk

import cv2

# cores ------------------------------------

co0 = "#f0f3f5"  # Preta
co1 = "#feffff"  # branca
co2 = "#4fa882"  # verde
co3 = "#38576b"  # valor
co4 = "#403d3d"   # letra
co5 = "#e06636"   # - profit

# criando janela ----------------------------

janela = Tk ()
janela.title ("")
janela.geometry('300x356')
janela.configure(background=co1)
janela.resizable(width=FALSE, height=FALSE)

style = ttk.Style(janela)
style.theme_use("clam")

# abindo imagem para logo
app_img  = Image.open('/home/paulo/Área de Trabalho/Public/python/logo.png')
app_img = app_img.resize((50, 50))
app_img = ImageTk.PhotoImage(app_img)

app_logo = Label(janela, image=app_img, text="Imagem para > Desenho a lapis", width=300, compound=LEFT, relief=RAISED, anchor=NW, font=('System 15 bold'),bg=co1, fg=co4 )
app_logo.place(x=0, y=0)


global imagem_original, l_imagem, imagem

imagem_original = ['']

# funcao para abrir imagem
def escolher_imagem():
    global imagem_original, l_imagem, imagem
    
    imagem = fd.askopenfilename()
    imagem_original.append(imagem)

    # abrindo a imagem
    imagem  = Image.open(imagem)
    imagem = imagem.resize((170, 170))
    imagem = ImageTk.PhotoImage(imagem)

    l_imagem = Label(janela, image=imagem,bg=co1, fg=co4 )
    l_imagem.place(x=60, y=60)
    
    
# funcao converter imagem

def converter_imagem():
    global imagem_original, l_imagem, imagem
    
    valor_escala = escala.get()
    
    # carregar a imagem escolhida
    imagem =cv2.imread(imagem_original[-1]) 
    
    # converter uma imagem de espaco de cores para outra
    imagem_sizenta = cv2.cvtColor(imagem, cv2.COLOR_BGR2GRAY)
    
    desfoco = cv2.GaussianBlur(imagem_sizenta, (21,21), 0,0)
    
    imagem_para_lapis = cv2.divide(imagem_sizenta, desfoco, scale=valor_escala)
    
    cv2.imwrite('/home/paulo/Área de Trabalho/Public/python/images/imagem_convertida.png',imagem_para_lapis )
    
    
    #abrindo a imagem que foi convertida
    imagem  = Image.open('/home/paulo/Área de Trabalho/Public/python/images/imagem_convertida.png')
    imagem = imagem.resize((170, 170))
    imagem = ImageTk.PhotoImage(imagem)

    l_imagem = Label(janela, image=imagem,bg=co1, fg=co4 )
    l_imagem.place(x=60, y=60)

# ----------------  opcoes ------------------------
l_opcoes = Label(janela, text="configuracoes -------------------------------------------".upper(), anchor=NW, font=('Verdana 7 bold'),bg=co1, fg=co4 )
l_opcoes.place(x=10, y=260)

escala = Scale(janela,command=converter_imagem, from_=0, to=255, length=120, bg=co1, fg='red', orient=HORIZONTAL)
escala.place(x=10, y=300)

b_escolher = Button(janela,command=escolher_imagem, text="Escolher imagem",width=15, overrelief=RIDGE,  font=('ivy 10'),bg=co1, fg=co4 )
b_escolher.place(x=147, y=287)

b_salvar = Button(janela,command=converter_imagem, text="Salvar",width=15, overrelief=RIDGE,  font=('ivy 10'),bg=co2, fg=co1 )
b_salvar.place(x=147, y=317)
```
janela.mainloop ()
