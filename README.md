import random
import tkinter as tk
from tkinter import messagebox, simpledialog

def choose_word():
    with open("palabras.txt", "r") as file:
        words = file.read().splitlines()
    return random.choice(words)

def display_word(word, guessed_letters):
    displayed_word = ""
    for letter in word:
        if letter in guessed_letters:
            displayed_word += letter + " "
        else:
            displayed_word += "_ "
    return displayed_word.strip()

def display_hangman(attempts):
    hangman_stages = [
        """
        -----
        |   |
            |
            |
            |
            |
        ----------
        """,
        """
        -----
        |   |
        O   |
            |
            |
            |
        ----------
        """,
        """
        -----
        |   |
        O   |
        |   |
            |
            |
        ----------
        """,
        """
        -----
        |   |
        O   |
       /|   |
            |
            |
        ----------
        """,
        """
        -----
        |   |
        O   |
       /|\\  |
            |
            |
        ----------
        """,
        """
        -----
        |   |
        O   |
       /|\\  |
       /    |
            |
        ----------
        """,
        """
        -----
        |   |
        O   |
       /|\\  |
       / \\  |
            |
        ----------
        """
    ]
    return hangman_stages[attempts]

class HangmanGame(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Juego del Ahorcado")
        self.geometry("500x600")
        self.mode = tk.StringVar()
        self.word = ""
        self.guessed_letters = []
        self.attempts = 0

        self.create_widgets()

    def create_widgets(self):
        mode_frame = tk.Frame(self)
        mode_frame.pack(pady=10)
        tk.Label(mode_frame, text="Modo de Juego: ", font=("Courier", 16)).pack(side=tk.LEFT)
        tk.Radiobutton(mode_frame, text="Individual", variable=self.mode, value="individual", font=("Courier", 14)).pack(side=tk.LEFT)
        tk.Radiobutton(mode_frame, text="VS", variable=self.mode, value="vs", font=("Courier", 14)).pack(side=tk.LEFT)
        self.mode.set("individual")

        self.hangman_label = tk.Label(self, text=display_hangman(self.attempts), font=("Courier", 16))
        self.hangman_label.pack(pady=10)

        self.word_label = tk.Label(self, text="", font=("Courier", 24))
        self.word_label.pack(pady=10)

        self.attempts_label = tk.Label(self, text=f"Intentos restantes: {6 - self.attempts}", font=("Courier", 16))
        self.attempts_label.pack(pady=10)

        self.guess_entry = tk.Entry(self, font=("Courier", 16))
        self.guess_entry.pack(pady=10)

        self.guess_button = tk.Button(self, text="Adivinar", command=self.guess_letter, font=("Courier", 16))
        self.guess_button.pack(pady=10)

        self.start_button = tk.Button(self, text="Iniciar Juego", command=self.start_game, font=("Courier", 16))
        self.start_button.pack(pady=10)

    def start_game(self):
        if self.mode.get() == "vs":
            player1_word = simpledialog.askstring("Jugador 1", "Jugador 1, por favor ingrese su palabra:", show='*')
            if not player1_word:
                messagebox.showerror("Error", "Por favor ingrese una palabra válida.")
                return
            self.word = player1_word.lower()
        else:
            self.word = choose_word()

        self.guessed_letters = []
        self.attempts = 0
        self.update_display()

    def guess_letter(self):
        guess = self.guess_entry.get().lower()
        self.guess_entry.delete(0, tk.END)

        if not guess:
            messagebox.showwarning("Advertencia", "Por favor, introduce una letra o una palabra.")
            return

        if guess in self.guessed_letters:
            messagebox.showwarning("Advertencia", "Ya has intentado esta letra. Prueba con otra.")
            return

        self.guessed_letters.append(guess)

        if guess == self.word:
            messagebox.showinfo("Felicidades", "¡Has ganado!")
            self.reset_game()
        elif len(guess) == 1 and guess in self.word:
            self.update_display()
            if all(letter in self.guessed_letters for letter in self.word):
                messagebox.showinfo("Felicidades", "¡Has ganado!")
                self.reset_game()
        else:
            self.attempts += 1
            self.update_display()
            if self.attempts == 6:
                messagebox.showinfo("Game Over", f"¡No! ¡Te has quedado sin intentos!\nLa palabra era: {self.word}")
                self.reset_game()

    def update_display(self):
        self.hangman_label.config(text=display_hangman(self.attempts))
        self.word_label.config(text=display_word(self.word, self.guessed_letters))
        self.attempts_label.config(text=f"Intentos restantes: {6 - self.attempts}")

    def reset_game(self):
        self.word = ""
        self.guessed_letters = []
        self.attempts = 0
        self.word_label.config(text="")
        self.hangman_label.config(text=display_hangman(self.attempts))
        self.attempts_label.config(text=f"Intentos restantes: {6 - self.attempts}")

if __name__ == "__main__":
    game = HangmanGame()
    game.mainloop()


