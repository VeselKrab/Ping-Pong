from pygame import *

# Необходимые классы
class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed, width, height):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (width, height))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update_r(self):  # Для правой ракетки (управление стрелками)
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_height - 80:
            self.rect.y += self.speed
    
    def update_l(self):  # Для левой ракетки (управление WS)
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 80:
            self.rect.y += self.speed

# Игровая сцена:
back = (200, 255, 255)  # цвет фона (background)
win_width = 600
win_height = 500
window = display.set_mode((win_width, win_height))
window.fill(back)

# картинки
racket1 = Player("6095521876.jpg", 30, 200, 4, 50, 150)
racket2 = Player("unnamed.jpg", 520, 200, 4, 50, 150)
ball = GameSprite("билаз.jpg", 200, 200, 0, 50, 50)


# Скорость мяча
speed_x = 3
speed_y = 3

# Флаги игры
game = True 
finish = False 
clock = time.Clock()
FPS = 60





# Основной игровой цикл
while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
    
    if not finish:
        window.fill(back)
        
    
        racket1.update_l()
        racket2.update_r()
        
        # Движение мяча
        ball.rect.x += speed_x
        ball.rect.y += speed_y
        
        # Отскок от стен
        if ball.rect.y > win_height-50 or ball.rect.y < 0:
            speed_y *= -1
        
        # Отскок от ракеток
        if sprite.collide_rect(ball, racket1) or sprite.collide_rect(ball, racket2):
            speed_x *= -1
        
        # гол)
        if ball.rect.x < 0 or ball.rect.x > win_width:
            finish = True
        
        # Отрисовка объектов
        racket1.reset()
        racket2.reset()
        ball.reset()
    
    display.update()
    clock.tick(FPS)
