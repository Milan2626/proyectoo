import tkinter as tk
from tkinter import messagebox
import random

class AhorcadoGUI:
    def __init__(self, master):
        self.master = master
        master.title("Juego del Ahorcado")

        self.palabras = ["erick", "mayky", "codigo", "juego", "logica"]
        self.palabra_secreta = random.choice(self.palabras)
        self.numero_intentos = 8
        self.letras_adivinadas = ""
        self.representacion_visual_palabra = "_" * len(self.palabra_secreta)

        # Labels
        self.label_palabra = tk.Label(master, text="Palabra: " + self.representacion_visual_palabra, font=("Arial", 24))
        self.label_palabra.pack(pady=20)

        self.label_intentos = tk.Label(master, text="Intentos restantes: " + str(self.numero_intentos), font=("Arial", 14))
        self.label_intentos.pack()

        self.label_letras_adivinadas = tk.Label(master, text="Letras adivinadas: ", font=("Arial", 14))
        self.label_letras_adivinadas.pack()

        # Entry
        self.entry_letra = tk.Entry(master, width=3, font=("Arial", 16))
        self.entry_letra.pack(pady=10)

        # Button
        self.boton_adivinar = tk.Button(master, text="Adivinar", command=self.adivinar_letra, font=("Arial", 14))
        self.boton_adivinar.pack()

        # Message Label
        self.label_mensaje = tk.Label(master, text="", font=("Arial", 14, "italic"))
        self.label_mensaje.pack(pady=10)

    def adivinar_letra(self):
        letra = self.entry_letra.get().lower()
        self.entry_letra.delete(0, tk.END)

        if not letra.isalpha() or len(letra) != 1:
            self.label_mensaje.config(text="Ingresa una sola letra válida.")
            return

        if letra in self.letras_adivinadas:
            self.label_mensaje.config(text="Ya usaste esa letra. Intenta con otra.")
            return

        self.letras_adivinadas += letra
        self.label_letras_adivinadas.config(text="Letras adivinadas: " + self.letras_adivinadas)

        acierto = False
        nueva_representacion = ""
        for i in range(len(self.palabra_secreta)):
            if self.palabra_secreta[i] == letra:
                nueva_representacion += letra
                acierto = True
            else:
                nueva_representacion += self.representacion_visual_palabra[i]

        self.representacion_visual_palabra = nueva_representacion
        self.label_palabra.config(text="Palabra: " + self.representacion_visual_palabra)

        if acierto:
            self.label_mensaje.config(text="¡Correcto!")
        else:
            self.numero_intentos -= 1
            self.label_intentos.config(text="Intentos restantes: " + str(self.numero_intentos))
            self.label_mensaje.config(text="Incorrecto. Esa letra no está.")

        self.verificar_estado_juego()

    def verificar_estado_juego(self):
        if "_" not in self.representacion_visual_palabra:
            self.label_mensaje.config(text="¡Felicidades! Adivinaste la palabra: " + self.palabra_secreta)
            self.deshabilitar_juego()
        elif self.numero_intentos <= 0:
            self.label_mensaje.config(text="¡Oh no! Te quedaste sin intentos. La palabra era: " + self.palabra_secreta)
            self.label_palabra.config(text="Palabra: " + self.palabra_secreta)
            self.deshabilitar_juego()

    def deshabilitar_juego(self):
        self.entry_letra.config(state="disabled")
        self.boton_adivinar.config(state="disabled")

root = tk.Tk()
gui = AhorcadoGUI(root)
root.mainloop()
