import pygame
pygame.init()

# Screen
WIDTH, HEIGHT = 800, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("2 Player Fight")

# Colors
WHITE = (255,255,255)
RED = (200,50,50)
BLUE = (50,50,200)
BLACK = (0,0,0)

# Players
p1 = pygame.Rect(100, 300, 50, 50)
p2 = pygame.Rect(650, 300, 50, 50)

p1_vel = 0
p2_vel = 0
gravity = 1

p1_hp = 100
p2_hp = 100

clock = pygame.time.Clock()

def draw():
    screen.fill(WHITE)

    # Draw players
    pygame.draw.rect(screen, RED, p1)
    pygame.draw.rect(screen, BLUE, p2)

    # Health bars
    pygame.draw.rect(screen, BLACK, (50, 20, 200, 20), 2)
    pygame.draw.rect(screen, RED, (50, 20, 2 * p1_hp, 20))

    pygame.draw.rect(screen, BLACK, (550, 20, 200, 20), 2)
    pygame.draw.rect(screen, BLUE, (550, 20, 2 * p2_hp, 20))

    pygame.display.flip()

running = True
while running:
    clock.tick(60)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()

    # Player 1 controls
    if keys[pygame.K_a]: p1.x -= 5
    if keys[pygame.K_d]: p1.x += 5
    if keys[pygame.K_w] and p1.y == 300: p1_vel = -15
    if keys[pygame.K_f]:
        if p1.colliderect(p2):
            p2_hp -= 1

    # Player 2 controls
    if keys[pygame.K_LEFT]: p2.x -= 5
    if keys[pygame.K_RIGHT]: p2.x += 5
    if keys[pygame.K_UP] and p2.y == 300: p2_vel = -15
    if keys[pygame.K_SLASH]:
        if p2.colliderect(p1):
            p1_hp -= 1

    # Gravity
    p1_vel += gravity
    p2_vel += gravity
    p1.y += p1_vel
    p2.y += p2_vel

    if p1.y > 300:
        p1.y = 300
        p1_vel = 0
    if p2.y > 300:
        p2.y = 300
        p2_vel = 0

    # Keep players on screen
    p1.x = max(0, min(WIDTH - 50, p1.x))
    p2.x = max(0, min(WIDTH - 50, p2.x))

    # Win condition
    if p1_hp <= 0 or p2_hp <= 0:
        print("Game Over")
        running = False

    draw()

pygame.quit()
