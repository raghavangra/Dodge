# Dodge
# -*- coding: utf-8 -*-
"""
Created on Thu Aug 30 00:02:40 2018
@author: RAGHAV ANGRA
"""
# Importing Libraries
import pygame
import time
import random
import sys
pygame.init()
pygame.mixer.pre_init(44100, 16, 2, 4096)
# Music 
pygame.mixer.music.load(r"C:\Users\RAGHAV ANGRA\Downloads\Music\halka halka.mp3")
# Defining colors and game display
BLACK = ( 0, 0, 0)
WHITE = ( 255, 255, 255)
GREEN=(0, 200, 0)
BRIGHT_GREEN = ( 0, 255, 0)
BRIGHT_RED = (255, 0, 0)
ORANGE=(255, 140, 25)
RED=(200, 0, 0)
LIGHT_BLUE = (5, 186, 245)
BLUE=(5, 140, 200)
YELLOW = (255, 255, 0)
BROWN=(56, 26, 0)
size = (700, 500)
screen = pygame.display.set_mode(size)
pygame.display.set_caption("DodgeBall")
clock = pygame.time.Clock()
# Start menu
def gameIntro():
     carryOn=True 
     while carryOn:
         # Records anything done by the user    
         for event in pygame.event.get():
            if event.type == pygame.QUIT: 
                pygame.display.quit()  
                pygame.quit()
                sys.exit(0)
         screen.fill(BLACK)          
         font=pygame.font.Font('freesansbold.ttf',70)
         text=font.render('Dodge Ball', True, YELLOW)
         textrect=text.get_rect()
         textrect.center=((700/2, 500/4))
         screen.blit(text,textrect)
         button("PLAY",700/2.75,500/2.7,200,60,BRIGHT_GREEN,GREEN)
         button("QUIT",700/2.75, 500/1.7, 200,60,BRIGHT_RED,RED)
         pygame.display.flip()
         clock.tick(15)         
def gamePause():
     while pause:
         pygame.mixer.music.pause()  
         # Records anything done by the user    
         for event in pygame.event.get():
            if event.type == pygame.QUIT: 
                pygame.display.quit()  
                pygame.quit()
                sys.exit(0)
            if event.type==pygame.KEYDOWN:
              if event.key==pygame.K_ESCAPE:
                    resume()
         screen.fill(BLACK)          
         font=pygame.font.Font('freesansbold.ttf',60)
         text=font.render('PAUSED', True, ORANGE)
         textrect=text.get_rect()
         textrect.center=((700/2, 500/4))
         screen.blit(text,textrect)
         button("CONTINUE",700/2.75,500/2.7,200,60,LIGHT_BLUE,BLUE)
         button("QUIT",700/2.75, 500/1.7, 200,60,BRIGHT_RED,RED)
         pygame.display.flip()
         clock.tick(15)
def resume():
         global pause
         pause=False
         pygame.mixer.music.unpause()
# Game over funnction
def gameOver():
     carryOn=True 
     while carryOn:
         # Records anything done by the user    
         for event in pygame.event.get():
            if event.type == pygame.QUIT: 
                pygame.display.quit()  
                pygame.quit()
                sys.exit(0)
         screen.fill(BLACK)          
         font=pygame.font.Font('freesansbold.ttf',60)
         text=font.render('GAME OVER!', True, WHITE)
         textrect=text.get_rect()
         textrect.center=((700/2, 500/4))
         screen.blit(text,textrect)
         button("PLAY AGAIN",700/2.75,500/2.7,200,60,LIGHT_BLUE,BLUE)
         button("QUIT",700/2.75, 500/1.7, 200,60,BRIGHT_RED,RED)
         pygame.display.flip()
         clock.tick(15)
# Start menu button function         
def button(msg,x,y,w,h,ac,ic):
       mouse=pygame.mouse.get_pos()
       click=pygame.mouse.get_pressed()
       # Button logic
       if x+w > mouse[0] > x and y+h > mouse[1] > y :
              pygame.draw.rect(screen, ac, [x,y,w,h] )
              if click[0]==1 and (msg=="PLAY" or msg=="PLAY AGAIN"):
                    game()
              if click[0]==1 and msg=="QUIT":
                    pygame.display.quit()
                    pygame.quit()
                    sys.exit(0)
              if click[0]==1 and msg=="CONTINUE":
                  resume()      
       else:  
              pygame.draw.rect(screen, ic, [x,y,w,h] )
       font=pygame.font.Font('freesansbold.ttf',30)
       text=font.render(msg, True, BLACK)
       textrect=text.get_rect()
       textrect.center=((x+w/2, y+h/2))
       screen.blit(text,textrect)   
# Defining game score       
def score(count):
      font=pygame.font.Font('freesansbold.ttf',20)
      text=font.render('SCORE: '+str(count), True, WHITE)
      screen.blit(text,(580,0))
# Defining things to be dodged      
def things(j,k,length,breadth):
      pygame.draw.rect(screen, WHITE, [j, k, length, breadth])
# Message displayed after crashing      
def crash():
      pygame.mixer.music.stop()
      font=pygame.font.Font('freesansbold.ttf',50)
      text=font.render('You Crashed', True, WHITE)
      textrect=text.get_rect()
      textrect.center=((700/2, 500/2))
      screen.blit(text,textrect)
      pygame.display.flip()
      time.sleep(2)
      gameOver()
# The final game loop      
def game(): 
  pygame.mixer.music.play(0)    
  global pause    
  x=350
  y=440
  r=20
  x_change=0  
  speed=7
  j=random.randrange(0,700)
  length=random.randrange(40,100)
  breadth=random.randrange(40,100)
  k=-600
  count=0
  carryOn = False
  while not carryOn:
     # Records anything done by the user on game display   
     for event in pygame.event.get():
        if event.type == pygame.QUIT: 
              pygame.display.quit()
              pygame.quit()
              sys.exit(0)
        # Movement of ball with arrow keys      
        if event.type==pygame.KEYDOWN:
              if event.key==pygame.K_LEFT:
                  x_change=-5
              if event.key==pygame.K_RIGHT:
                    x_change=5
              # To pause the game
              if event.key==pygame.K_ESCAPE:
                    pause=True
                    gamePause()               
     # Calling functions defined               
     screen.fill(BLACK)
     pygame.draw.circle(screen, YELLOW, [x,y],r,0)
     things(j,k,length,breadth)
     score(count)
     k+=speed
     x+=x_change
     # Game logic 
     # Collision with boundary
     if x<40 or x>660:
          crash()
     # Generating random blocks from random positions      
     if k>500:
           k=0-breadth
           j=random.randrange(0,700)
           length=random.randrange(40,100)
           breadth=random.randrange(40,100)
           count+=1
           # Increasing speed of block to make the game challenging
           if speed<26:
               speed+=1
     # Collision with block          
     if (y-r)<(k+breadth) and k<(y+r):
           if (x-r)>j and (x-r)<(j+length) or (x+r)>j and (x+r)<(j+length):
                crash()
     pygame.display.flip()
     clock.tick(40)
gameIntro()     
game()    
pygame.quit()
sys.exit(0)

