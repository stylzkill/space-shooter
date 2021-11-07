# space-shooter
game
import pygame
import math
import random
#intialize the game
pygame.init()
#creat screen
screen=pygame.display.set_mode((800,800))
#background
background=pygame.image.load('SPACE.png')
#Title and icon
pygame.display.set_caption("space invaders ")
icon=pygame.image.load('ufo.png')
pygame.display.set_icon((icon))
#creat a screen
#player
playerimage=pygame.image.load('spaceship.png')
playerX =400
playerY=700
playerx_change=0
def player(x,y):
    screen.blit(playerimage,(x,y))
#enemie
enemie_img=[]
enemieX=[]
enemieY=[]
enemiex_change=[]
enemiey_change=[]
for i in range(6):

   enemie_img.append(pygame.image.load('enemy.png'))
   enemieX.append(random.randint(0,720))
   enemieY.append(random.randint(50,150))
   enemiex_change.append(2)
   enemiey_change.append(40)
def enemie(x,y,i):
    screen.blit(enemie_img[i],(x,y))
#bullet
bullet_img=pygame.image.load(('bullet.png'))
bulletX=0
bulletY=710
bulletx_change=0
bullety_change=10
bullet_state='ready'
score=0
def bullet(x,y):
    global bullet_state
    bullet_state='fire'
    screen.blit(bullet_img, (x+16, y+10))
def iscollision(enemieX,enemieY,bulletX,bulletY):
    distance=math.sqrt(math.pow((enemieX-bulletX),2)+math.pow((enemieY-bulletY),2))
    if distance<27:
        return  True
    else :
        return False

#game loope
running=True
while running :
    screen.fill((0, 0, 0))
    screen.blit(background,(0,0))
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            running=False


        if event.type==pygame.KEYDOWN:
            if event.key==pygame.K_LEFT:
                playerx_change=-5
            if event.key == pygame.K_RIGHT:
                playerx_change=5
            if event.key == pygame.K_SPACE:
                if bullet_state=='ready':
                    bulletX=playerX
                    bullet(bulletX,bulletY)

        if event.type==pygame.KEYUP:
            if event.key==pygame.K_LEFT or event.key == pygame.K_RIGHT:

                playerx_change=0
    playerX += playerx_change
    if playerX < 0:
        playerX = 0
    elif playerX >= 738:
        playerX = 738

    #enemie mvmt
    for i in range(6):

        enemieX[i] += enemiex_change[i]
        if enemieX[i]<=0:
           enemiex_change[i]=2
           enemieY[i]+=enemiey_change[i]
        elif enemieX[i]>=710:
           enemiex_change[i]=-2
           enemieY[i]+=enemiey_change[i]
        # collision
           # collision

        collision = iscollision(enemieX[i], enemieY[i], bulletX, bulletY)
        if collision:
            bulleyY = 710
            bullet_state = 'ready'
            score += 1
            enemieX[i] = random.randint(0, 720)
            enemieY[i] = random.randint(50, 150)

            print(score)

    if bulletY<=0:
        bulletY=710
        bullet_state='ready'
    if bullet_state=='fire':
        bullet(bulletX,bulletY)
        bulletY-=bullety_change

    player(playerX,playerY)
    for i in range(6):
        enemie(enemieX[i], enemieY[i], i)
    pygame.display.update()









