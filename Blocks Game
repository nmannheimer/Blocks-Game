import pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Constants
WINDOW_WIDTH = 800
WINDOW_HEIGHT = 600
PLAYER_SIZE = 20
BLOCK_SIZE = 30
BLOCK_SPEED = 5
PLAYER_SPEED = 7

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Set up the display
screen = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))
pygame.display.set_caption("Dodge the Blocks")
clock = pygame.time.Clock()

class Player:
    def __init__(self):
        self.x = WINDOW_WIDTH // 2
        self.y = WINDOW_HEIGHT - PLAYER_SIZE - 10
        self.speed = PLAYER_SPEED
        self.rect = pygame.Rect(self.x, self.y, PLAYER_SIZE, PLAYER_SIZE)

    def move(self, direction):
        if direction == 'left' and self.x > 0:
            self.x -= self.speed
        if direction == 'right' and self.x < WINDOW_WIDTH - PLAYER_SIZE:
            self.x += self.speed
        self.rect.x = self.x

    def draw(self):
        pygame.draw.circle(screen, BLUE, (self.x + PLAYER_SIZE//2, self.y + PLAYER_SIZE//2), PLAYER_SIZE//2)

class Block:
    def __init__(self):
        self.x = random.randint(0, WINDOW_WIDTH - BLOCK_SIZE)
        self.y = -BLOCK_SIZE
        self.speed = BLOCK_SPEED
        self.rect = pygame.Rect(self.x, self.y, BLOCK_SIZE, BLOCK_SIZE)

    def move(self):
        self.y += self.speed
        self.rect.y = self.y

    def draw(self):
        pygame.draw.rect(screen, RED, self.rect)

    def is_off_screen(self):
        return self.y > WINDOW_HEIGHT

def main():
    player = Player()
    blocks = []
    score = 0
    game_over = False
    font = pygame.font.Font(None, 36)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN and game_over:
                if event.key == pygame.K_SPACE:
                    # Reset game
                    player = Player()
                    blocks = []
                    score = 0
                    game_over = False

        if not game_over:
            # Handle player movement
            keys = pygame.key.get_pressed()
            if keys[pygame.K_LEFT]:
                player.move('left')
            if keys[pygame.K_RIGHT]:
                player.move('right')

            # Spawn new blocks
            if random.random() < 0.02:  # 2% chance each frame
                blocks.append(Block())

            # Update blocks
            for block in blocks[:]:
                block.move()
                if block.is_off_screen():
                    blocks.remove(block)
                    score += 1
                if block.rect.colliderect(player.rect):
                    game_over = True

        # Draw everything
        screen.fill(BLACK)
        player.draw()
        for block in blocks:
            block.draw()

        # Draw score
        score_text = font.render(f'Score: {score}', True, WHITE)
        screen.blit(score_text, (10, 10))

        if game_over:
            game_over_text = font.render('Game Over! Press SPACE to restart', True, WHITE)
            text_rect = game_over_text.get_rect(center=(WINDOW_WIDTH//2, WINDOW_HEIGHT//2))
            screen.blit(game_over_text, text_rect)

        pygame.display.flip()
        clock.tick(60)

if __name__ == "__main__":
    main() 