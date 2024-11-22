## 🙌 Hello 🙌
<img src="https://capsule-render.vercel.app/api?type=Venom&color=gradient&height=300&section=header&text=welcome%20to%20혜인%20Git%20Hub&fontSize=50" />

## 🌟introduce🌟
안녕하세요! 이혜인입니다.

🖌️ Art and Technology 전공자

💻 관심 분야: 게임 개발, 3D 아트, 포토샵

🎮 게임과 🎨 드로잉, 그리고 🐟 해산물을 좋아합니다.

GitHub에서 코딩과 창작의 즐거움을 함께 나누고 싶어요!

### Image link
![onealog](/assets/readme/easyme.png)   
   

## 😀 Tech Stack 😀
<img src="https://img.shields.io/badge/Adobe-20232a.svg?style=for-the-badge&logo=Adobe&logoColor=FF0000" />  <img src="https://img.shields.io/badge/photoshop-20232a.svg?style=for-the-badge&logo=adobephotoshop&logoColor=9999FF" />  <img src="https://img.shields.io/badge/premierepro-20232a.svg?style=for-the-badge&logo=adobepremierepro&logoColor=9999FF" />  <img src="https://img.shields.io/badge/cplusplus-20232a.svg?style=for-the-badge&logo=cplusplus&logoColor=00599C" />  
<img src="https://img.shields.io/badge/python-20232a.svg?style=for-the-badge&logo=python&logoColor=3776AB" />  <img src="https://img.shields.io/badge/illustrator-20232a.svg?style=for-the-badge&logo=adobeillustrator&logoColor=FF9A00" />

## 🌈 content
<details><summary>phone number
</summary>

*010-3141-2376*
</details>
<a href="https://www.instagram.com/hyein_0217/"><img src="https://img.shields.io/badge/instagram-E4405F?style=flat-square&logo=instagram&logoColor=white"/></a>


# 🛠️ project 🛠️   

##우주 침략자 게임 (Space Invaders Game) 🚀
이 프로젝트는 간단한 2D 우주 침략자 게임입니다. Python의 pygame 라이브러리를 활용하여 구현되었으며, 플레이어는 우주선을 조종해 외계인을 물리치고 점수를 획득하는 것을 목표로 합니다.

## 💻 코드보기
<details>
  <summary>Click to see the code</summary>

```python
import sys
from random import randint
import pygame
from pygame.locals import Rect, QUIT, KEYDOWN, \
     K_LEFT, K_RIGHT, K_SPACE

pygame.init()
pygame.key.set_repeat(5, 5)
SURFACE = pygame.display.set_mode((600, 600))
FPSCLOCK = pygame.time.Clock()

class Drawable:
    def __init__(self, rect, offset0, offset1):
        strip = pygame.image.load("strip.png")
        self.images=(pygame.Surface((24, 24), pygame.SRCALPHA),
                     pygame.Surface((24, 24), pygame.SRCALPHA))
        self.rect = rect
        self.count = 0
        self.images[0].blit(strip, (0, 0),
                            Rect(offset0, 0, 24, 24))
        self.images[1].blit(strip, (0, 0),
                            Rect(offset1, 0, 24, 24))

    def move(self, diff_x, diff_y):
        self.count += 1
        self.rect.move_ip(diff_x, diff_y)

    def draw(self):
        image=self.images[0] if self.count % 2 == 0 \
              else self.images[1]
        SURFACE.blit(image, self.rect.topleft)

class Ship(Drawable):
    def __init__(self):
        super().__init__(Rect(300, 550, 24, 24), 192, 192)

class Beam(Drawable):
    def __init__(self):
        super().__init__(Rect(300, 0, 24, 24), 0, 24,)


class Bomb(Drawable):
    def __init__(self):
        super(). __init__(Rect(300, -50, 24, 24), 48, 72)
        self.time=randint(5, 220)

class Alien(Drawable):
    def __init__(self, rect, offset, score):
        super().__init__(rect, offset, offset+24)
        self.score=score

def main():
    sysfont =pygame.font.SysFont(None, 72)
    scorefont = pygame.font.SysFont(None, 36)
    message_clear = sysfont.render("!!CLEAR!!",
                                 True, (0, 255, 255))
    message_over = sysfont.render("!!YOU DIED!!",
                                True, (0, 255, 255))
    message_rect = message_clear.get_rect()
    message_rect.center = (300, 300)
    game_over = False
    moving_left = True
    moving_down = False
    move_interval = 20
    counter = 0
    score = 0
    aliens = []
    bombs = []
    ship = Ship()
    beam = Beam()

    #외계인 나열, 초기화
    for ypos in range(4):
        offset = 96 if ypos < 2 else 144
        for xpos in range(10):
            rect = Rect(100+xpos*50, ypos*50 + 50, 24, 24)
            alien = Alien(rect, offset, (4-ypos)*10)
            aliens.append(alien)
            
    #폭탄 설정
    for _  in range(4):
        bombs.append(Bomb())


    while True:
        ship_move_x = 0
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
            elif event.type == KEYDOWN:
                if event.key == K_LEFT:
                    ship_move_x = -5
                elif event.key == K_RIGHT:
                    ship_move_x = +5
                elif event.key == K_SPACE and beam.rect.bottom < 0:
                    beam.rect.center = ship.rect.center

        if not game_over:
            counter += 1
            #내캐릭 이동
            ship.move(ship_move_x, 0)

            #빔이동
            beam.move(0, -15)

            #외계인 이동
            area = aliens[0].rect.copy()
            for alien in aliens:
                area.union_ip(alien.rect)

            if counter % move_interval == 0:
                move_x = -5 if moving_left else 5
                move_y = 0

                if (area.left < 10 or area.right > 590) and \
                    not moving_down:
                    moving_left = not moving_left
                    move_x, move_y = 0, 24
                    move_interval = max(1, move_interval - 2)
                    moving_down = True
                else:
                    moving_down = False

                for alien in aliens:
                    alien.move(move_x, move_y)

            if area.bottom > 550:
                game_over = True

            #폭탄이동
            for bomb in bombs:
                if bomb.time < counter and bomb.rect.top < 0:
                    enemy = aliens[randint(0, len(aliens) - 1)]
                    bomb.rect.center = enemy.rect.center

                if bomb.rect.top > 0:
                    bomb.move(0, 10)

                if bomb.rect.top > 600:
                    bomb.time += randint(50, 250)
                    bomb.rect.top = -50

                if bomb.rect.colliderect(ship.rect):
                    game_over = True

            #빔과 외계인 충돌
            tmp= []
            for alien in aliens:
                if alien.rect.collidepoint(beam.rect.center):
                    beam.rect.top = -50
                    score += alien.score
                else:
                    tmp.append(alien)
            aliens = tmp
            if len(aliens) == 0:
                    game_over = True
                    
        #그리기
        SURFACE.fill((0, 0, 0))
        for alien in aliens:
            alien.draw()
        ship.draw()
        beam.draw()
        for bomb in bombs:
            bomb.draw()

        score_str = str(score).zfill(5)
        score_image = scorefont.render(score_str,
                                     True, (0, 255, 0))
        SURFACE.blit(score_image, (500, 10))

        if game_over:
            if len(aliens) == 0:
                SURFACE.blit(message_clear, message_rect.topleft)
            else:
                SURFACE.blit(message_over, message_rect.topleft)

        pygame.display.update()
        FPSCLOCK.tick(20)

if __name__=='__main__':
    main()
```

<br>   
</details>

## 🎮게임기능

### 🕹️ 플레이어 컨트롤
화살표 키로 우주선을 좌우로 이동.

스페이스바를 눌러 빔 발사.

### 👾 적군(외계인) 행동
외계인이 좌우로 이동하며 점점 아래로 내려옵니다.

외계인마다 다른 점수를 부여.

#### 👽 외계인 이미지 👽
![strip](https://github.com/user-attachments/assets/ce85bb6c-660e-4f29-88b1-88e080f062dd)
 
<br>

### 💣 폭탄
외계인이 무작위로 폭탄을 투하하며 플레이어가 피해야 합니다.

### 🚫 게임 종료 조건
모든 외계인을 처치하면 "!!CLEAR!!" 메시지와 함께 승리.

외계인이 화면 아래로 내려오거나 폭탄에 맞으면 "!!YOU DIED!!" 메시지와 함께 게임 종료.

## ⚙️ 기술 스택
언어: Python 3.x

라이브러리: Pygame


## ✨ 개발 동기
Python 학습과 pygame 라이브러리 활용 능력을 높이기 위해 제작했습니다. 

간단하지만 게임 설계와 충돌 처리, 이벤트 제어 등의 핵심 개념을 익히는 데 좋은 경험이 되었습니다.


