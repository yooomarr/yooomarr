import pygame
import random

# Initialize Pygame
pygame.init()

# Constants
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
BACKGROUND_COLOR = (0, 0, 0)
PLAYER_COLOR = (0, 128, 255)
OBJECT_COLOR = (255, 0, 0)
OBJECT_SIZE = 30
PLAYER_SIZE = 50
PLAYER_SPEED = 5
OBJECT_COUNT = 10

# Create the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Scratch Game")

# Create the player
player_x = SCREEN_WIDTH // 2
player_y = SCREEN_HEIGHT // 2
player_rect = pygame.Rect(player_x, player_y, PLAYER_SIZE, PLAYER_SIZE)

# Create objects
objects = [pygame.Rect(random.randint(0, SCREEN_WIDTH - OBJECT_SIZE), random.randint(0, SCREEN_HEIGHT - OBJECT_SIZE), OBJECT_SIZE, OBJECT_SIZE) for _ in range(OBJECT_COUNT)]

# Create a clock object to control the frame rate
clock = pygame.time.Clock()

# Main game loop
running = True
score = 0

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Move the player
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_x -= PLAYER_SPEED
    if keys[pygame.K_RIGHT]:
        player_x += PLAYER_SPEED
    if keys[pygame.K_UP]:
        player_y -= PLAYER_SPEED
    if keys[pygame.K_DOWN]:
        player_y += PLAYER_SPEED

    player_rect = pygame.Rect(player_x, player_y, PLAYER_SIZE, PLAYER_SIZE)

    # Check for collisions with objects
    for obj in objects[:]:
        if player_rect.colliderect(obj):
            objects.remove(obj)
            score += 1
            new_obj = pygame.Rect(random.randint(0, SCREEN_WIDTH - OBJECT_SIZE), random.randint(0, SCREEN_HEIGHT - OBJECT_SIZE), OBJECT_SIZE, OBJECT_SIZE)
            objects.append(new_obj)

    # Draw everything
    screen.fill(BACKGROUND_COLOR)
    pygame.draw.rect(screen, PLAYER_COLOR, player_rect)

    for obj in objects:
        pygame.draw.rect(screen, OBJECT_COLOR, obj)

    pygame.display.flip()

    # Cap the frame rate
    clock.tick(30)

# Clean up and quit
pygame.quit()
