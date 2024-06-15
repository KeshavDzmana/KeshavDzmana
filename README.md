i have created this program for my python project
import pygame
import time
import random

pygame.init()

# Window dimensions
window_width = 800
window_height = 600

# Colors
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)

# Snake block size
block_size = 20
snake_speed = 15

# Font for displaying text
font_style = pygame.font.SysFont(None, 50)

# Function to display score
def Your_score(score):
    value = font_style.render("Your Score: " + str(score), True, white)
    window.blit(value, [0, 0])

# Function to draw snake
def our_snake(block_size, snake_list):
    for x in snake_list:
        pygame.draw.rect(window, black, [x[0], x[1], block_size, block_size])

# Function to display message
def message(msg, color):
    mesg = font_style.render(msg, True, color)
    window.blit(mesg, [window_width / 6, window_height / 3])

# Main function for the game
def gameLoop():
    game_over = False
    game_close = False

    # Initial position of the snake
    x1 = window_width / 2
    y1 = window_height / 2

    # Change in position
    x1_change = 0
    y1_change = 0

    # Initial length of the snake
    snake_List = []
    Length_of_snake = 1

    # Random position for food
    foodx = round(random.randrange(0, window_width - block_size) / 10.0) * 10.0
    foody = round(random.randrange(0, window_height - block_size) / 10.0) * 10.0

    while not game_over:
        while game_close == True:
            window.fill(black)
            message("You Lost! Press C-Play Again or Q-Quit", red)
            Your_score(Length_of_snake - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.boutt:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_l:
                    x1_change = -block_size
                    y1_change = 0
                elif event.key == pygame.K_r:
                    x1_change = block_size
                    y1_change = 0
                elif event.key == pygame.K_u:
                    y1_change = -block_size
                    x1_change = 0
                elif event.key == pygame.K_d:
                    y1_change = block_size
                    x1_change = 0

        # If snake hits the wall, game over
        if x1 >= window_width or x1 < 0 or y1 >= window_height or y1 < 0:
            game_close = True
        x1 += x1_change
        y1 += y1_change
        window.fill(black)
        pygame.draw.rect(window, red, [foodx, foody, block_size, block_size])
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]

        # If snake hits itself, game over
        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True

        our_snake(block_size, snake_List)
        Your_score(Length_of_snake - 1)
        pygame.display.update()

        # If snake eats food
        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, window_width - block_size) / 10.0) * 10.0
            foody = round(random.randrange(0, window_height - block_size) / 10.0) * 10.0
            Length_of_snake += 1

        # Set snake speed
        clock.tick(snake_speed)

    pygame.quit()
    quit()

# Game window setup
window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption('Snake Game')
clock = pygame.time.Clock()

gameLoop()
