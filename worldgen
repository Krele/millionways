import random, pygame, sys, math, time
from pygame.locals import *
from random import randrange      


global dyngrid,grid,m,n,brd
n = 22
m = 44
brd = 2 #border size
clouds=0
grid = [[0 for y in range(n)] for x in range(m)] #filling MxN space with 0
dyngrid = [[0 for y in range(n)] for x in range(m)] #this grid can be changed during the game instead of previous one

def distance(x1,y1,x2,y2): #distance between 2 hexagons
  L=0
  L=math.sqrt((abs(x1-x2))**2+(abs(y1-y2))**2)
  return(L)
  

def zerocheck(x,y):  #TRUE if at least 1 water hexagon nearby
  waternearby=0
  if x%2==0 :
   surr = grid[x+1][y-1]*grid[x+1][y]*grid[x][y+1]*grid[x-1][y]*grid[x-1][y-1]*grid[x][y-1]  
  else:
   surr = grid[x+1][y+1]*grid[x+1][y]*grid[x][y+1]*grid[x-1][y]*grid[x-1][y+1]*grid[x][y-1]  
  if surr==0 :
      waternearby=1      
  return(waternearby)

def nearby(GridType,HexType,x,y): #shows how many hexagons of "HexType" is nearby
    HexTypeCount=0
    if x%2==0:
        if GridType[x][y-1]==HexType:
          HexTypeCount+=1
        if GridType[x+1][y-1]==HexType:
          HexTypeCount+=1
        if GridType[x+1][y]==HexType:
          HexTypeCount+=1          
        if GridType[x][y+1]==HexType:
          HexTypeCount+=1          
        if GridType[x-1][y]==HexType:
          HexTypeCount+=1
        if GridType[x-1][y-1]==HexType:
          HexTypeCount+=1
    else:
        if GridType[x][y-1]==HexType:
          HexTypeCount+=1
        if GridType[x+1][y+1]==HexType:
          HexTypeCount+=1
        if GridType[x+1][y]==HexType:
          HexTypeCount+=1          
        if GridType[x][y+1]==HexType:
          HexTypeCount+=1          
        if GridType[x-1][y]==HexType:
          HexTypeCount+=1
        if GridType[x-1][y+1]==HexType:
          HexTypeCount+=1      
    return(HexTypeCount)


def generate(m,n,grid):  #map generation

 earth = 0             
 while earth<0.075*m*n :  #1 hexagon at time
  x = randrange(brd,m-brd)   
  y = randrange(brd,n-brd)
  if grid[x][y] == 0 :
     grid[x][y] = 1
     earth+=1

 while earth<=0.13*m*n :   #4 hexagons at time
  x = randrange(brd,m-brd)   
  y = randrange(brd,n-brd)  
  if grid[x][y] == 0 :
    grid[x][y] = 1
    earth+=1
    if x%2==0 :
     grid[x+1][y] = 1
     if grid[x+1][y] == 0 : earth+=1
     grid[x][y+1] = 1
     if grid[x][y+1] == 0 : earth+=1
     grid[x+1][y-1] = 1
     if grid[x+1][y-1] == 0 : earth+=1
    else: 
     grid[x+1][y+1] = 1
     if grid[x+1][y+1] == 0 : earth+=1
     grid[x][y+1] = 1
     if grid[x][y+1] == 0 : earth+=1
     grid[x-1][y] = 1
     if grid[x-1][y] == 0 : earth+=1

 while earth<=0.17*m*n :   #3 hexagons at time
  x = randrange(brd,m-brd)   
  y = randrange(brd,n-brd)  
  if grid[x][y] == 0 :
    grid[x][y] = 1
    earth+=1
    grid[x+1][y] = 1
    if grid[x+1][y] == 0 : earth+=1
    if x%2==0 :
        grid[x][y+1] = 1
        if grid[x][y+1] == 0 : earth+=1
    else:
        grid[x+1][y+1] = 1
        if grid[x+1][y+1] == 0 :
            earth+=1

 while earth<=0.28*m*n :   #additional fill
  x = randrange(brd,m-brd)   
  y = randrange(brd,n-brd)   
  if nearby(grid,1,x,y)>=4:
   if grid[x][y]==0:
    if randrange(100)<50 :  
      grid[x][y] = 1
      earth+=1
   if nearby(grid,1,x,y)>=5:
    if grid[x][y]==0:
     if randrange(100)<80 :  
      grid[x][y] = 1
      earth+=1
   if nearby(grid,1,x,y)>=3:
    if grid[x][y]==0:
     if randrange(100)<25 :  
      grid[x][y] = 1
      earth+=1

 mountains = 0         #mountais placement on "earth"
 while mountains<=0.08*earth:   
   x = randrange(brd,m-brd)   
   y = randrange(brd,n-brd)
   if zerocheck(x,y)==0:
      grid[x][y]=4
      mountains+=1
 while mountains<=0.15*earth:
  for x in range(brd,m-brd):
   for y in range(brd,n-brd):
    if grid[x][y]==4 :
      if zerocheck(x,y)==0:
         if randrange(100)<50:
          rx=randrange(3)-1
          ry=randrange(3)-1
          if ry!=rx:
            grid[x+rx][y+ry]=4
            mountains+=1         
     

 forest = 0          #forest placement on "earth"
 while forest<=0.27*(mountains+earth):        
  x = randrange(brd,m-brd)   
  y = randrange(brd,n-brd)  
  if randrange(100)<15:
   if grid[x][y] == 1 :
       grid[x][y]=7
       forest+=1
   else:
     if grid[x][y+1]==1 and grid[x+1][y]==1 and grid[x+1][y+1]==1:
       grid[x][y]=7
       grid[x][y+1]=7
       grid[x+1][y+1]=7
       grid[x+1][y]=7

 cities=0                 #cities - simple placement way
 for i in range(4):         
  citycheck=0
  while citycheck==0:   
    if i==0:
      x = randrange(brd,m/2-3)   
      y = randrange(brd,n/2-3)
    if i==1:
      x = randrange(m/2+3,m-brd)   
      y = randrange(n/2+3,n-brd)
    if i==2:
      x = randrange(m/2+3,m-brd)   
      y = randrange(brd,n-brd)
    if i==3:
      x = randrange(brd,m/2-3)   
      y = randrange(n/2+3,n-brd)
    if grid[x][y] == 1 :
       grid[x][y]=2
       citycheck=1  
 earth=0
 for x in range(m):               
   for y in range (n):
     if grid[x][y]!=0:
       earth+=1

 factories=0
 while factories<5 :
  x = randrange(brd,m-brd)   
  y = randrange(brd,n-brd)
  if grid[x][y] == 1 and nearby(grid,4,x,y)>0:
    grid[x][y] = 5
    factories+=1

 return              #END OF "GENERATE" 



def blit_alpha(target, source, location, opacity): #in the name of transparency!
        x = location[0]
        y = location[1]     
        temp = pygame.Surface((source.get_width(), source.get_height())).convert()
        temp.blit(target, (-x, -y))
        temp.blit(source, (0, 0))
        temp.set_alpha(opacity)        
        target.blit(temp, location)
   
  

def DisplayGrid(): #display grid : “odd-q” vertical layout
 dx=0       
 for x in range(m):
      dy=0
      if x%2 == 0:
       for y in range (n):
           if grid[x][y]==1:
               DISPLAYSURF.blit(groundImg,(x+dx,y+dy))
           elif grid[x][y]==0:
               DISPLAYSURF.blit(waterImg,(x+dx,y+dy))
           elif grid[x][y]==7:
               DISPLAYSURF.blit(forestImg,(x+dx,y+dy))
           elif grid[x][y]==2:
               DISPLAYSURF.blit(cityImg,(x+dx,y+dy))
           elif grid[x][y]==5:
               DISPLAYSURF.blit(factoryImg,(x+dx,y+dy))  
           else:
               DISPLAYSURF.blit(rockImg,(x+dx,y+dy))
           dy+=22
      else:
       dy=11
       for y in range (n):
           if grid[x][y]==1:
               DISPLAYSURF.blit(groundImg,(x+dx,y+dy))
           elif grid[x][y]==0:
               DISPLAYSURF.blit(waterImg,(x+dx,y+dy))
           elif grid[x][y]==7:
               DISPLAYSURF.blit(forestImg,(x+dx,y+dy))
           elif grid[x][y]==2:
               DISPLAYSURF.blit(cityImg,(x+dx,y+dy))
           elif grid[x][y]==5:
               DISPLAYSURF.blit(factoryImg,(x+dx,y+dy))  
           else:
               DISPLAYSURF.blit(rockImg,(x+dx,y+dy))
           dy+=22
      dx+=17
 return


def DisplayDynGrid(opacity):  #display dyngrid
 dx=0             
 for x in range(m):
      dy=0
      if x%2 == 0:
       for y in range (n):           
           if dyngrid[x][y]==9:
               blit_alpha(DISPLAYSURF, cloudImg, (x+dx,y+dy), opacity)
           dy+=22
      else:
       dy=11
       for y in range (n):              
           if dyngrid[x][y]==9:
               blit_alpha(DISPLAYSURF, cloudImg, (x+dx,y+dy), opacity)
           dy+=22
      dx+=17

def CreateClouds():  #cloud generation               
 clouds=0
 while clouds<=0.08*m*n:
    x = randrange(brd+3,m-brd-3)   
    y = randrange(brd+3,n-brd-3)
    if dyngrid[x][y]!=4: 
     if nearby(dyngrid,9,x,y)>2 and grid[x][y]!=0 :
       if randrange(100)<45 :
         dyngrid[x][y]=9
         clouds+=1
     if nearby(dyngrid,9,x,y)>1 and grid[x][y]==0 :
       if randrange(100)<40:
         dyngrid[x][y]=9
         clouds+=1
     if nearby(dyngrid,9,x,y)>0 and grid[x][y]==0 :
       if randrange(100)<35:
         dyngrid[x][y]=9
         clouds+=1
     if randrange(100)<2:
         dyngrid[x][y]=9
         clouds+=1 
    
def RN(x,y): #RANDOM NEIGHBOUR
  rx=x+randrange(3)-1
  ry=y+randrange(3)-1
  if x%2==0:
   while (rx==1 and ry==1) or (rx==-1 and ry==1):
    rx=x+randrange(3)-1
    ry=y+randrange(3)-1
  else:
   while (rx==1 and ry==-1) or (rx==-1 or ry==-1):
    rx=x+randrange(3)-1
    ry=y+randrange(3)-1
  return([rx,ry])

def MoveClouds(): #moves random cloud for 1 hexagon
  for x in range(brd,m-brd):
    for y in range (brd,n-brd):
      if dyngrid[x][y]==9:
        if x<=brd:
          dyngrid[x][y]=0
          dyngrid[x+1][y]=9
        if x>=m-brd-1:
          dyngrid[x][y]=0
          dyngrid[x-1][y]=9
        if y<=brd:
          dyngrid[x][y]=0
          dyngrid[x][y+1]=9
        if y>=n-brd-1:
          dyngrid[x][y]=0
          dyngrid[x][y-1]=9
      rx=RN(x,y)[0]
      ry=RN(x,y)[1]
      if dyngrid[x][y]==9 and dyngrid[rx][ry]==0 :
       if nearby(dyngrid,9,rx,ry)>0:
        if randrange(100)<75:
           dyngrid[x][y]=0
           dyngrid[rx][ry]=9
       if nearby(dyngrid,9,rx,ry)>1:
        if randrange(100)<95:
           dyngrid[x][y]=0
           dyngrid[rx][ry]=9
       if nearby(dyngrid,9,rx,ry)==0:
        if randrange(100)<10:
           dyngrid[x][y]=0
           dyngrid[rx][ry]=9

pygame.init()#*****************PROGRAMM****************
FPS = 30
WIDTH = 800
HEIGHT = 600

fpsClock = pygame.time.Clock()
DISPLAYSURF = pygame.display.set_mode((WIDTH,HEIGHT))

pygame.display.set_caption('Map')
groundImg = pygame.image.load('G.png')
waterImg = pygame.image.load('W.png')
rockImg = pygame.image.load('M.png')
forestImg = pygame.image.load('F.png')
cityImg = pygame.image.load('C.png')
factoryImg = pygame.image.load('factory.png')
cloudImg = pygame.image.load('cloud.png').convert_alpha()

font = pygame.font.Font('GOST_A.ttf',28) #set font file
TurnMade = font.render('Player has made a turn...',True,(247,120,36)) 
textRectObj = TurnMade.get_rect() 
surface = pygame.Surface((300,32)) 
surface.blit(TurnMade, textRectObj) 
surface.set_alpha(0)
textRectObj.center = (150, 560)

generate(m,n,grid)                             
CreateClouds()
 


i=16         #opacity modifier
opacity=32
CloudCounter=0
TextAlpha=0
while True:     #MAIN LOOP

 DISPLAYSURF.fill((0,0,0))
 DisplayGrid()
 DISPLAYSURF.blit(surface,textRectObj)
 if CloudCounter>15:
  TextAlpha = int(round(TextAlpha*0.8))
 
 if CloudCounter==30:
   MoveClouds()
   CloudCounter=0
   TextAlpha=255

  
 if opacity <= 96 and  i>0:
  i=  16
 if opacity >= 225 and  i>0:
  i= -16
 if opacity <= 225 and  i<0:
  i= -16
 if opacity <= 96 and  i<0:
  i=  16
 opacity = opacity + i
 DisplayDynGrid(opacity)
 surface.set_alpha(TextAlpha)
 CloudCounter+=1

    
 for event in pygame.event.get():
         if event.type == QUIT:
             pygame.quit()
             sys.exit()
 pygame.display.update()
 fpsClock.tick(FPS)   
    
            
     



