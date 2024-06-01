# Juego-del-ahorcado
Codigo

import random

palabras = ('ecuador','colombia', 'brazil', 'argentina', 'chile', 'peru', 'estados unidos', 'canada')
secreta = random.choice(palabras)
cadena = "-" * len(secreta)
print("Bienvenidos al juego del ahorcado")
intentos = 3

while True:
    print(cadena)
    letra = input("Ingresa una letra:")
    if letra in secreta:
        for i in range(len(secreta)):
            if secreta[i] == letra:
                cadena = cadena[:i] + letra + cadena [i+1:]
    else:  
        if intentos == 1:
            print("O")
        elif intentos == 2:
            print(" O")
            print("/")
        elif intentos == 3:
            print(" O")
            print("/")
        elif intentos == 4: 
            print(" O")
            print("/|\\")
        elif intentos == 5: 
            print(" O")
            print("/|\\")
            print("/")
        elif intentos == 5: 
            print(" O")
            print("/|\\")
            print("/ \\")
            print(f"Has perdido la partida, la palabra secreta era{secreta}")
            break
