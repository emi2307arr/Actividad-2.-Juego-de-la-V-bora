# Actividad-2.-Juego-de-la-V-bora
from turtle import *
from random import choice, randrange
from freegames import square, vector

# Escogemos que colores pueden ser para la comida y la serpiente

COLORS = ['blue', 'green', 'pink', 'purple', 'orange']

food_color = snake_color = choice([c for c in COLORS if c != 'red'])

## Especificamos los vectores de la comida, la serpiente y el movimiento
 
food = vector(0, 0)
snake = [vector(10, 0)]
aim = vector(0, -10)

def change(x, y):
    "Change snake direction."
    aim.x = x
    aim.y = y

## Definimos la cabeza de nuestra serpiente
    
def inside(head):
    "Return True if head inside boundaries."
    return -200 < head.x < 190 and -200 < head.y < 190

## Definimos como se mueve la serpiente

def move():
    "Move snake forward one segment."
    head = snake[-1].copy()
    head.move(aim)

    if not inside(head) or head in snake:
        square(head.x, head.y, 9, 'red')
        update()
        return

    snake.append(head)

    if head == food:
        print('Snake:', len(snake))
        food.x = randrange(-15, 15) * 10
        food.y = randrange(-15, 15) * 10

   # Definimos el color de la comida y hacemos que sea diferente al de la serpiente
        global food_color
        while food_color == snake_color:
            food_color = choice(COLORS)

    # movemos la comida al azar un paso a la vez sin salirse de la ventana
    else:
        movement = vector(randrange(-10, 11, 10), randrange(-10, 11, 10))
        new_food_pos = food + movement
        if inside(new_food_pos):
            food.move(movement)

    clear()

    for body in snake:
        square(body.x, body.y, 9, snake_color)

    square(food.x, food.y, 9, food_color)
    update()
    ontimer(move, 100)

## Ajustamos cómo será nuestro tablero y movimientos.
    
setup(420, 420, 370, 0)
hideturtle()
tracer(False)
listen()
onkey(lambda: change(10, 0), 'Right')
onkey(lambda: change(-10, 0), 'Left')
onkey(lambda: change(0, 10), 'Up')
onkey(lambda: change(0, -10), 'Down')
move()
done()
