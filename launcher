import tkinter as tk
from tkinter import scrolledtext
import subprocess
import random
import os

# Obtener el PID del propio script
script_pid = os.getpid()

print(f"El PID del script es: {script_pid}")

class TerminalApp:
    def _init_(self, master):
        self.master = master
        master.title("Terminal")

        # Cuadro de texto desplazable para la salida del terminal
        self.output_text = scrolledtext.ScrolledText(master, wrap=tk.WORD, height=20, width=60, font=("Consolas", 12), bg="black", fg="lime green", insertbackground="lime green")
        self.output_text.pack(expand=True, fill="both")

        # Entrada del usuario
        self.input_entry = tk.Entry(master, width=60, font=("Consolas", 12), bg="black", fg="lime green")
        self.input_entry.pack(expand=True, fill="x")
        self.input_entry.bind("<Return>", self.process_input)

        # Enfocar la entrada de usuario al inicio
        self.input_entry.focus()

        # Lista para almacenar los números roleados
        self.rolled_numbers = []

        # Configurar el tema "hacker"
        self.master.configure(bg="black")

    def process_input(self, event):
        # Obtener el texto de la entrada del usuario
        user_input = self.input_entry.get()

        # Ejecutar comandos
        if user_input.lower() == "rolls":
            self.roll_dice()
        elif user_input.lower() == "flip_coin":
            self.flip_coin()
        elif user_input.lower().startswith("choose"):
            self.choose_option(user_input)
        elif user_input.lower() == "random_color":
            self.random_color()
        elif user_input.lower() == "table":
            self.show_leaderboard()
        else:
            result = self.execute_command(user_input)

            # Mostrar el resultado en el cuadro de texto de salida
            self.output_text.insert(tk.END, f">> {user_input}\n{result}\n\n")

        # Limpiar la entrada del usuario
        self.input_entry.delete(0, tk.END)

    def execute_command(self, command):
        try:
            # Ejecutar el comando y capturar la salida estándar y el código de salida
            output = subprocess.check_output(command, shell=True, stderr=subprocess.STDOUT, text=True)
            return output
        except subprocess.CalledProcessError as e:
            # Capturar errores de ejecución de comandos
            return f"Error: {e.output}"

    def roll_dice(self):
        # Generar un número aleatorio según las nuevas probabilidades
        lucky_number = self.generate_lucky_number()

        # Almacenar el número en la lista de clasificación
        self.rolled_numbers.append(lucky_number)

        # Ordenar la lista de clasificación en orden descendente
        self.rolled_numbers.sort(reverse=True)

        # Mostrar el resultado en el cuadro de texto de salida
        result = f"Número obtenido: {lucky_number}"
        self.output_text.insert(tk.END, f">> Rolls\n{result}\n\n")

    def generate_lucky_number(self):
        # Generar un número aleatorio con las nuevas probabilidades
        probability = random.random()  # Número aleatorio entre 0 y 1

        if probability < 0.8:
            return random.randint(1, 100)
        elif probability < 0.9:
            return random.randint(101, 2000)
        elif probability < 0.97:
            return random.randint(2001, 40000)
        else:
            return random.randint(40001, 100000)

    def flip_coin(self):
        # Simular lanzamiento de una moneda
        result = "Cara" if random.choice([True, False]) else "Cruz"

        # Mostrar el resultado en el cuadro de texto de salida
        self.output_text.insert(tk.END, f">> Flip Coin\nResultado: {result}\n\n")

    def choose_option(self, user_input):
        # Obtener opciones de la entrada del usuario
        options = user_input.split()[1:]

        # Elegir una opción aleatoria
        selected_option = random.choice(options)

        # Mostrar la opción seleccionada en el cuadro de texto de salida
        self.output_text.insert(tk.END, f">> Choose\nOpción seleccionada: {selected_option}\n\n")

    def random_color(self):
        # Generar un código hexadecimal de color aleatorio
        random_color = "#{:06x}".format(random.randint(0, 0xFFFFFF))

        # Cambiar el esquema de colores de la terminal
        self.master.configure(bg=random_color)
        self.output_text.configure(bg=random_color)
        self.input_entry.configure(bg=random_color, fg="black")

        # Mostrar el color generado en el cuadro de texto de salida
        self.output_text.insert(tk.END, f">> Random Color\nCódigo de color: {random_color}\n\n")

    def show_leaderboard(self):
        # Mostrar la tabla de clasificación en el cuadro de texto de salida
        if self.rolled_numbers:
            self.output_text.insert(tk.END, "Tabla de Clasificación:\n")
            for i, number in enumerate(self.rolled_numbers, start=1):
                self.output_text.insert(tk.END, f"{i}. {number}\n")
            self.output_text.insert(tk.END, "\n")
        else:
            self.output_text.insert(tk.END, "Aún no se han obtenido números. ¡Juega 'rolls' primero!\n\n")

def main():
    root = tk.Tk()
    app = TerminalApp(root)
    root.mainloop()

if _name_ == "_main_":
    main()
