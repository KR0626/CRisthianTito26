import tkinter as tk
from tkinter import ttk
from PIL import ImageTk, Image
from tkinter import messagebox

#Widget: elemento gráfico interactivo que forma parte de la interfaz de usuario
#Los widgets son componentes básicos que se utilizan para construir interfaces de usuario 
#y permiten a los usuarios interactuar con la aplicación.

# Lista que representa la matriz del juego
matriz_juego = []
# Lista temporal para almacenar una fila de la matriz
submatriz = []
# Contadores para el número de barcos de cada tipo
cont_nave1 = 6
cont_nave2 = 4
cont_nave3 = 2

# Variables globales para el tipo de barco seleccionado y su orientación
selected_ship = None
selected_orientation = None

# Función para crear la pantalla de inicio del juego
def pantalla_jug():
    global Namejug1, Nickjug1, Namejug2, Nickjug2, Filas, Columnas, root
    DicJugadores = {} 
    root = tk.Tk()
    root.title("Registro de Jugadores")
    root.geometry("600x600""+0+0")
    root.resizable(False, False)

    # Cargar y mostrar una imagen de fondo
    imagen1 = Image.open("Portada.jpg")
    imagen1 = imagen1.resize((600, 600)) #resize dimensiones de una imagen
    imagen1 = ImageTk.PhotoImage(imagen1) #PhotoImage es una Clase que permite cargar imagenes
    labelimagen = ttk.Label(image=imagen1) # crea un widget para mostrar la imagen, con el parámetro de Image y image1 tienen la imagen
    labelimagen.place(x=0, y=1) #Espacio de la imagen completa en la pantalla

    # Etiquetas y campos de entrada para los nombres y apodos de los jugadores
    JugLabel1 = ttk.Label(text="Jugador 1", padding=5, background="green", font="Arial")
    JugLabel1.place(x=10, y=280)
    NickLabel1 = ttk.Label(text="Nickname 1", padding=5, background="green", font="Arial")
    NickLabel1.place(x=10, y=330)

    JugLabel2 = ttk.Label(text="Jugador 2", padding=5, background="green", font="Arial") 
    JugLabel2.place(x=300, y=280)
    NickLabel2 = ttk.Label(text="Nickname 2", padding=5, background="green", font=("Arial"))
    NickLabel2.place(x=300, y=330)
    
    NFilas = ttk.Label(text="Filas", padding=5, background="blue", font="Arial") 
    NFilas.place(x=10, y=380)
    NColumnas = ttk.Label(text="Columas", padding=5, background="blue", font=("Arial"))
    NColumnas.place(x=300, y=380)

    #padding es un espacio adicional
    
    Namejug1 = ttk.Entry(background="green", justify="center", font=("Arial", 14))
    Nickjug1 = ttk.Entry(background="green", justify="center", font=("Arial", 14))
    Namejug2 = ttk.Entry(background="green", justify="center", font=("Arial", 14))
    Nickjug2 = ttk.Entry(background="green", justify="center", font=("Arial", 14))
    Filas = ttk.Entry(background="green", justify="center", font=("Arial", 14))
    Columnas = ttk.Entry(background="green", justify="center", font=("Arial", 14))
    
    #Entry es una clase que crea un widget de entrada de texto
    
    Namejug1.place(x=130, y=280, width=160, height=30)
    Nickjug1.place(x=130, y=330, width=160, height=30)
    Namejug2.place(x=420, y=280, width=160, height=30)
    Nickjug2.place(x=420, y=330, width=160, height=30)
    Filas.place(x=130, y=380, width=160, height=30)
    Columnas.place(x=420, y=380, width=160, height=30)

    # Botón para iniciar el juego
    Aceptar = ttk.Button(text="INICIO", command=validar, padding=5)
    Aceptar.place(x=500, y=420)
    root.mainloop()
    return Namejug1, Nickjug1, Namejug2, Nickjug2

# Variables para el tamaño de la matriz del juego
global fila, columna
while True:
    try: #maneja excepciones
        fila = int(input("Filas: "))
        columna = int(input("Columna: "))
    except ValueError:
        print("DIguite un número valido")
    if fila < 10 or columna < 20:
         print("La matriz dada es muy pequeña, por favor hágala más grande.")
    elif fila > 18 or columna > 26:
        print("La matriz dada es demasiado grande, por favor hágala más pequeña.")
    elif columna % 2 != 0:
        print("Por favor, ingrese un número par de columnas.")
    else:
        break

# Función para validar la entrada del usuario y comenzar el juego
def validar():
    global Namejug1, Nickjug1, Namejug2, Nickjug2, DicJugadores
    DicJugadores = {} 
    dicmatriz = {}

    # Guardar información de jugadores y tamaño de la matriz en archivos de texto
    DicJugadores.update({"Jug1": Namejug1.get(), "NN1": Nickjug1.get(), "Jug2": Namejug2.get(), "NN2": Nickjug2.get()})
    dicmatriz.update({"filas": fila,"columnas":columna})
    archivojud = open("jugadores.txt", "w") #Modo de escritura
    archivomatriz= open("matriz.txt", "w") #Modo de escritura
    archivomatriz.write(str(dicmatriz))
    archivojud.write(str(DicJugadores))
    archivojud.close()
    archivomatriz.close()

    # Cerrar la ventana de inicio
    root.destroy()
    # Leer la información guardada desde los archivos y mostrarla en la consola
    archivojud = open("jugadores.txt", "r")
    archivomatriz= open("matriz.txt","r")
    DicJugadores = {}
    dicmatriz = {}
    listmatriz = archivomatriz.readline()[1:-1]
    listmatriz = listmatriz.split(",") #Divide una cadena en una lista de subcadenas basadas en un delimitador específico.
    for x in range(len(listmatriz)):
        tempmatriz = listmatriz[x].split(":")
        dicmatriz.update({tempmatriz[0].strip("' "): tempmatriz[1].strip("' ")})
    print(dicmatriz) #update actualiza la información 
    ListJugadores = archivojud.readline()[1:-1] 
    ListJugadores = ListJugadores.split(",")
    for x in range(len(ListJugadores)):
        TempJugadores = ListJugadores[x].split(":")
        DicJugadores.update({TempJugadores[0].strip("' "): TempJugadores[1].strip("' ")})

    print(DicJugadores)

# Función para crear el tablero de juego
def crear_tablero(fila, columna):
    global submatriz, matriz_juego
    root = tk.Tk()
    root.title("Juego de Batalla Naval")
    root.geometry(str(fila * 80) + "x" + str(columna * 20) + "+210+0")
    root.resizable(False, False)
    
    posy = 0
    for f in range(fila):
        posx = 0
        for c in range(columna):
            submatriz.append(tk.Button(root, background="dark blue", command=lambda a=f, b=c: accion_boton(a, b)))
            celda = submatriz[c]
            celda.place(x=posx, y=posy)
            celda.configure(width=40, height=40)
            posx += 40
        matriz_juego.append(submatriz)
        posy += 40
        submatriz = []

    return root

# Función para cargar las imágenes de los barcos
def cargar_imagenes():
    global nave1, nave21, nave22, nave31, nave32, nave33
    global arnave1, arnave21, arnave22, arnave31, arnave32, arnave33
    global dnave1, dnave21, dnave22, dnave31, dnave32, dnave33
    global abnave1, abnave21, abnave22, abnave31, abnave32, abnave33

    # Cargar y redimensionar las imágenes de los barcos en diferentes orientaciones
    nave1 = Image.open("b1.png")
    nave1 = nave1.resize((40, 40))
    nave1 = nave1.rotate(180)
    nave1 = ImageTk.PhotoImage(nave1)
    nave21 = Image.open("b21.png")
    nave21 = nave21.resize((40, 40))
    nave21 = nave21.rotate(180)
    nave21 = ImageTk.PhotoImage(nave21)
    nave22 = Image.open("b22.png")
    nave22 = nave22.resize((40, 40))
    nave22 = nave22.rotate(180)
    nave22 = ImageTk.PhotoImage(nave22)
    nave31 = Image.open("b31.png")
    nave31 = nave31.resize((40, 40))
    nave31 = nave31.rotate(180)
    nave31 = ImageTk.PhotoImage(nave31)
    nave32 = Image.open("b32.png")
    nave32 = nave32.resize((40, 40))
    nave32 = nave32.rotate(180)
    nave32 = ImageTk.PhotoImage(nave32)
    nave33 = Image.open("b33.png")
    nave33 = nave33.resize((40, 40))
    nave33 = nave33.rotate(180)
    nave33 = ImageTk.PhotoImage(nave33)
    
    # Repetir el proceso para otras orientaciones y tipos de barcos
    arnave1 = Image.open("b1.png")
    arnave1 = arnave1.resize((40, 40))
    arnave1 = arnave1.rotate(90)
    arnave1 = ImageTk.PhotoImage(arnave1)
    arnave21 = Image.open("b21.png")
    arnave21 = arnave21.resize((40, 40))
    arnave21 = arnave21.rotate(90)
    arnave21 = ImageTk.PhotoImage(arnave21)
    arnave22 = Image.open("b22.png")
    arnave22 = arnave22.resize((40, 40))
    arnave22 = arnave22.rotate(90)
    arnave22 = ImageTk.PhotoImage(arnave22)
    arnave31 = Image.open("b31.png")
    arnave31 = arnave31.resize((40, 40))
    arnave31 = arnave31.rotate(90)
    arnave31 = ImageTk.PhotoImage(arnave31)
    arnave32 = Image.open("b32.png")
    arnave32 = arnave32.resize((40, 40))
    arnave32 = arnave32.rotate(90)
    arnave32 = ImageTk.PhotoImage(arnave32)
    arnave33 = Image.open("b33.png")
    arnave33 = arnave33.resize((40, 40))
    arnave33 = arnave33.rotate(90)
    arnave33 = ImageTk.PhotoImage(arnave33)
    dnave1 = Image.open("b1.png")
    dnave1 = dnave1.resize((40, 40))
    dnave1 = dnave1.rotate(0)
    dnave1 = ImageTk.PhotoImage(dnave1)
    dnave21 = Image.open("b21.png")
    dnave21 = dnave21.resize((40, 40))
    dnave21 = dnave21.rotate(0)
    dnave21 = ImageTk.PhotoImage(dnave21)
    dnave22 = Image.open("b22.png")
    dnave22 = dnave22.resize((40, 40))
    dnave22 = dnave22.rotate(0)
    dnave22 = ImageTk.PhotoImage(dnave22) 
    dnave31 = Image.open("b31.png")
    dnave31 = dnave31.resize((40, 40))
    dnave31 = dnave31.rotate(0)
    dnave31 = ImageTk.PhotoImage(dnave31)
    dnave32 = Image.open("b32.png")
    dnave32 = dnave32.resize((40, 40))
    dnave32 = dnave32.rotate(0)
    dnave32 = ImageTk.PhotoImage(dnave32)
    dnave33 = Image.open("b33.png")
    dnave33 = dnave33.resize((40, 40))
    dnave33 = dnave33.rotate(0)
    dnave33 = ImageTk.PhotoImage(dnave33)
    abnave1 = Image.open("b1.png")
    abnave1 = abnave1.resize((40, 40))
    abnave1 = abnave1.rotate(270)
    abnave1 = ImageTk.PhotoImage(abnave1)
    abnave21 = Image.open("b21.png")
    abnave21 = abnave21.resize((40, 40))
    abnave21 = abnave21.rotate(270)
    abnave21 = ImageTk.PhotoImage(abnave21)
    abnave22 = Image.open("b22.png")
    abnave22 = abnave22.resize((40, 40))
    abnave22 = abnave22.rotate(270)
    abnave22 = ImageTk.PhotoImage(abnave22) 
    abnave31 = Image.open("b31.png")
    abnave31 = abnave31.resize((40, 40))
    abnave31 = abnave31.rotate(270)
    abnave31 = ImageTk.PhotoImage(abnave31)
    abnave32 = Image.open("b32.png")
    abnave32 = abnave32.resize((40, 40))
    abnave32 = abnave32.rotate(270)
    abnave32 = ImageTk.PhotoImage(abnave32)
    abnave33 = Image.open("b33.png")
    abnave33 = abnave33.resize((40, 40))
    abnave33 = abnave33.rotate(270)
    abnave33 = ImageTk.PhotoImage(abnave33)

# Función que se ejecuta al hacer clic en un botón del tablero
def accion_boton(x, y):
    global cont_nave1, cont_nave2, cont_nave3
    if selected_ship is None or selected_orientation is None:
        messagebox.showerror("Por favor, seleccione el tipo de barco y su orientación.")
        return
    if selected_ship == "nave3":
        # Colocar el barco de 3 celdas en la orientación seleccionada
        # Reducir el contador de barcos de 3 celdas
        if cont_nave3 > 0:
            if selected_orientation == "izquierda":
                if y-1 >= 0 and y+1 < len(matriz_juego[x]): #cget: valor actual de una opción específica de un widget
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x][y-1].cget("image") == "" and matriz_juego[x][y+1].cget("image") == "":
                        matriz_juego[x][y].config(image=nave32) # config: configura las propiedades de un widget
                        matriz_juego[x][y+1].config(image=nave33)
                        matriz_juego[x][y-1].config(image=nave31)
                        cont_nave3 -= 1
            elif selected_orientation == "Arriba":
                if x-1 >= 0 and x+1 < len(matriz_juego) and y < len(matriz_juego[x]):
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x-1][y].cget("image") == "" and matriz_juego[x+1][y].cget("image") == "":
                        matriz_juego[x][y].config(image=arnave32)
                        matriz_juego[x+1][y].config(image=arnave33)
                        matriz_juego[x-1][y].config(image=arnave31)
                        cont_nave3 -= 1
            elif selected_orientation == "Abajo":
                if x-1 >= 0 and x+1 < len(matriz_juego) and y < len(matriz_juego[x]):
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x-1][y].cget("image") == "" and matriz_juego[x+1][y].cget("image") == "":
                        matriz_juego[x][y].config(image=abnave32)
                        matriz_juego[x+1][y].config(image=abnave31) 
                        matriz_juego[x-1][y].config(image=abnave33)   
                        cont_nave3-=1
            elif selected_orientation == "Derecha":
                if y-1 >= 0 and y+1 < len(matriz_juego[x]):
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x][y-1].cget("image") == "" and matriz_juego[x][y+1].cget("image") == "":
                        matriz_juego[x][y].config(image=dnave32)
                        matriz_juego[x][y-1].config(image=dnave33)
                        matriz_juego[x][y+1].config(image=dnave31)
                        cont_nave3 -= 1
        else:
            messagebox.showerror("Ya no quedan más barcos de tipo Acorazado ")
    if selected_ship == "nave2":
        # Colocar el barco de 2 celdas en la orientación seleccionada
        # Reducir el contador de barcos de 2 celdas
        if cont_nave2 > 0:
            if selected_orientation == "izquierda":
                if y+1 < len(matriz_juego[x]):
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x][y+1].cget("image") == "":
                        matriz_juego[x][y].config(image=nave21)
                        matriz_juego[x][y+1].config(image=nave22)
                        cont_nave2 -= 1
            elif selected_orientation == "Arriba":
                if x+1 < len(matriz_juego):
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x+1][y].cget("image") == "":
                        matriz_juego[x][y].config(image=arnave21)
                        matriz_juego[x+1][y].config(image=arnave22)
                        cont_nave2 -= 1
            elif selected_orientation == "Abajo":
                if x-1 >= 0 < len(matriz_juego):
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x-1][y].cget("image") == "":
                        matriz_juego[x][y].config(image=abnave21)
                        matriz_juego[x-1][y].config(image=abnave22)    
                        cont_nave2-=1
            elif selected_orientation == "Derecha":
                if y-1 >= 0 < len(matriz_juego[x]):
                    if matriz_juego[x][y].cget("image") == "" and matriz_juego[x][y-1].cget("image") == "":
                        matriz_juego[x][y].config(image=dnave21)
                        matriz_juego[x][y-1].config(image=dnave22)
                        cont_nave2 -= 1
        else:
            messagebox.showerror("Ya no quedan más barcos de tipo Crucero")
    if selected_ship == "nave1":
        # Colocar el barco de 1 celda en la orientación seleccionada
        # Reducir el contador de barcos de 1 celda
        if cont_nave1 > 0:
            if selected_orientation == "izquierda":
                if matriz_juego[x][y].cget("image") == "":
                    matriz_juego[x][y].config(image=nave1)
                    cont_nave1 -= 1
            elif selected_orientation == "Arriba":
                if matriz_juego[x][y].cget("image") == "":
                    matriz_juego[x][y].config(image=arnave1)
                    cont_nave1 -= 1
            elif selected_orientation == "Abajo":
                if matriz_juego[x][y].cget("image") == "":
                    matriz_juego[x][y].config(image=abnave1)
                    cont_nave1 -= 1
            elif selected_orientation == "Derecha":
                if matriz_juego[x][y].cget("image") == "":
                    matriz_juego[x][y].config(image=dnave1)
                    cont_nave1 -= 1
        else:
            messagebox.showerror("Ya no quedan más barcos de tipo Destructor")
# Funciones para seleccionar el tipo de barco y su orientación
def seleccionar_barco(tipo):
    global selected_ship
    selected_ship = tipo

def seleccionar_orientacion(orientacion):
    global selected_orientation
    selected_orientation = orientacion

def salir(salir):
    if cont_nave1 ==0 and cont_nave2 ==0 and cont_nave3 == 0:
        tablero.destroy()
    else:
        messagebox.showerror("Aun quedan barcos")

# Función principal del juego
def main():
    global fila,columna,tablero

    # Crear el tablero de juego y mostrar ventanas emergentes para seleccionar barco y orientación
    tablero = crear_tablero(int(fila), int(columna))
    cargar_imagenes()

    ventana_barco = tk.Toplevel(tablero)
    ventana_barco.title("Seleccionar Barco")
    ventana_barco.geometry("200x200"+"+0+0")
    ttk.Label(ventana_barco, text="Seleccione el tipo de barco:").pack(pady=10)
    ttk.Button(ventana_barco, text="Destructor", command=lambda: seleccionar_barco("nave1")).pack(pady=5)
    ttk.Button(ventana_barco, text="Crucero", command=lambda: seleccionar_barco("nave2")).pack(pady=5)
    ttk.Button(ventana_barco, text="Acorazado", command=lambda: seleccionar_barco("nave3")).pack(pady=5)
    ttk.Button(ventana_barco, text="Guardar", command=lambda: salir("Guardar")).pack(pady=5)

    ventana_orientacion = tk.Toplevel(tablero) # Con toplevel creamos ventanas individuales
    ventana_orientacion.title("Seleccionar Orientación")
    ventana_orientacion.geometry("200x200" + "+0+250")
    ttk.Label(ventana_orientacion, text="Seleccione la orientación del barco:").pack(pady=10)
    ttk.Button(ventana_orientacion, text="izquierda", command=lambda: seleccionar_orientacion("izquierda")).pack(pady=5)
    ttk.Button(ventana_orientacion, text="Arriba", command=lambda: seleccionar_orientacion("Arriba")).pack(pady=5)
    ttk.Button(ventana_orientacion, text="Derecha", command=lambda: seleccionar_orientacion("Derecha")).pack(pady=5)
    ttk.Button(ventana_orientacion, text="Abajo", command=lambda: seleccionar_orientacion("Abajo")).pack(pady=5)

    tablero.mainloop()

if __name__ == "__main__":
    pantalla_jug()
    # Contadores para el número de barcos de cada tipo
    cont_nave1 = 6
    cont_nave2 = 4
    cont_nave3 = 2    
    main()
