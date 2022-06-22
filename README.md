# LEYES-DE-KIRCHHOFF
# -*- coding: utf-8 -*-
import pygame
import numpy as np
pygame.init()   #Inicia PyGame
window = pygame.display.set_mode((1101, 700))
run = True
pygame.display.set_caption("Leyes de Kirchhoff")
track = pygame.image.load('track3.jpg')
clock = pygame.time.Clock()   #Inicio cod mov
font = pygame.font.SysFont('Calibri', 22)
sw = True
cargasSup = []
cargasInf = []
class Button():
    def __init__(self, color, x, y, width, height, text=''):
        self.color = color
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.text = text
        self.over = False
        self.image = font.render(self.text, 1, (0,0,0))
    def draw(self,window,outline=None):
                #Llama a este método para dibujar el botón en la pantalla
        if outline:
            pygame.draw.rect(window, outline, (self.x-2,self.y-4,self.width+4,self.height+8),0)
                    
        pygame.draw.rect(window, self.color, (self.x,self.y-2,self.width,self.height+4),0)
                
        if self.text != '':
            w, h = self.image.get_size()
            window.blit(self.image, (self.x + (self.width//2 - w//2), self.y + (self.height//2 - h//2 + 2)))

    def isOver(self, pos):
                #pos es la posición del ratón o una tupla de coordenadas (x,y)
        if pos[0] > self.x and pos[0] < self.x + self.width:
            if pos[1] > self.y and pos[1] < self.y + self.height:
                return True
        return False
class Calculate:
    def __init__(self):
        self.currentValue = 0   
        self.newNumber = 0      
        self.currentOperation = None   
        self.currentText = ""          
        self.sw = False         
        self.pot = 1
        self.fem1 = 0;         
        self.fem2 = 0;
        self.fem3 = 0;
        self.r1 = 1;
        self.r2 = 1;
        self.r3 = 1;  
        self.i1 = 0;
        self.i2 = 0;
        self.i3 = 0; 
    def newDigit(self, text):
        if(text != "."):
            if(self.sw == False):
                self.newNumber = self.newNumber * 10 + int(text) 
                self.currentText = str(self.newNumber)
            else:
                self.newNumber = self.newNumber+(int(text)/(10**self.pot))
                self.newNumber = round(self.newNumber, self.pot)
                self.pot = self.pot + 1
                self.currentText = str(self.newNumber)
        else:
            self.sw = True
            self.newNumber = self.newNumber + 0.0
            self.currentText = str(self.newNumber)
            
    def newOperation(self, op):
        try:
            if(op != "Guardar Dato"):
                if(op == 'FEM1[V]'):
                    self.currentOperation ='FEM1[V]'
                elif(op == 'FEM2[V]'):
                    self.currentOperation ='FEM2[V]'
                elif(op == 'FEM3[V]'):
                    self.currentOperation ='FEM3[V]'
                elif(op == 'R1[Ω]'):
                    self.currentOperation ='R1[Ω]'
                elif(op == 'R2[Ω]'):
                    self.currentOperation ='R2[Ω]'
                elif(op == 'R3[Ω]'):
                    self.currentOperation ='R3[Ω]'
                elif(op == 'Calcular Corrientes'):
                    A = np.array([[1.0,-1.0,-1.0],[self.r3,self.r2,0.0],[0.0,self.r2,-self.r1]])
                    B = np.array([0.0,self.fem3-self.fem2,self.fem1-self.fem2])
                    x = np.linalg.solve(A,B)
                    self.i1 = round(x[0], 2)
                    self.i2 = round(x[1], 2)
                    self.i3 = round(x[2], 2)
                    global sw, cargasSup, cargasInf
                    sw = True
                    cargasSup.clear()
                    cargasInf.clear()
                    print(self.i1,self.i2,self.i3)      
            else:
                if self.currentOperation == 'FEM1[V]':
                    self.fem1 = self.newNumber
                elif self.currentOperation == 'FEM2[V]':
                    self.fem2 = self.newNumber
                elif self.currentOperation == 'FEM3[V]':
                    self.fem3 = self.newNumber
                elif self.currentOperation == 'R1[Ω]':
                    self.r1 = self.newNumber
                elif self.currentOperation == 'R2[Ω]':
                    self.r2 = self.newNumber
                elif self.currentOperation == 'R3[Ω]':
                    self.r3 = self.newNumber  
                self.newNumber = 0
                self.sw = False
                self.pot = 1;
                print(self.fem1,self.fem2,self.fem3,self.r1,self.r2,self.r3)
        except:
            self.newNumber = 0
        self.currentText = str(self.newNumber)
calculator = Calculate()                  
silver = (192,192,192)
verde = (0,255,0)
naranja = (255,128,0)
blanco = (255,255,255)
cyan = (0,255,255)
azul = (0,128,255)
s_1s = Button(silver,630,280,  60,30, '1')
s_2s = Button(silver,710,280,  60,30, '2')
s_3s = Button(silver,790,280,  60,30, '3')
s_4s = Button(silver,630,230, 60,30, '4')
s_5s = Button(silver,710,230, 60,30, '5')
s_6s = Button(silver,790,230, 60,30, '6')
s_7s = Button(silver,630,180, 60,30, '7')
s_8s = Button(silver,710,180, 60,30, '8')
s_9s = Button(silver,790,180, 60,30, '9')
s_0s = Button(silver,630,330, 100,30, '0')
s_punto = Button(silver,750,330, 100,30, '.')
numbers = [s_1s,s_2s,s_3s,s_4s,s_5s,s_6s,s_7s,s_8s,s_9s,s_0s,s_punto]
d_1s = Button(verde,980,180,80,30, 'FEM1[V]')   
d_2s = Button(verde,980,230,80,30, 'FEM2[V]')   
d_3s = Button(verde,980,280,80,30, 'FEM3[V]')
d_4s = Button(verde,880,180,80,30, 'R1[Ω]')   
d_5s = Button(verde,880,230,80,30, 'R2[Ω]')   
d_6s = Button(verde,880,280,80,30, 'R3[Ω]')
d_7s = Button(verde,870,330,200,30, 'Calcular Corrientes')
d_8s = Button(verde,870,130,200,30, 'Guardar Dato')  
symbols = [d_1s,d_2s,d_3s,d_4s,d_5s,d_6s,d_7s,d_8s]
clearButton = Button(naranja,630,130,60,30, 'AC')
allButtons = numbers + symbols + [clearButton]
inputtap = Button(blanco,630,50,450,50,"")
def redraw():
    for button in allButtons:
        button.draw(window)
    inputtap.draw(window)
    inputtext = fuente2.render(calculator.currentText, True, (0, 0, 0))
    #dibujar el texto en una nueva superficie (texto, antialias, color, fondo = Ninguno)
    window.blit(inputtext, (inputtap.x + inputtap.width - inputtext.get_width() - 4, inputtap.y + 4))
    #surface.blit() dibuja una superficie de origen sobre otra superficie.
    #blit(origen, destino, area=Ninguno, special_flags=0) -> Rect
    valor_r1 = Button(cyan,630,570,150,30, 'R1 = '+str(calculator.r1)+" [Ω]"); valor_r1.draw(window)
    valor_r2 = Button(cyan,630,610,150,30, 'R2 = '+str(calculator.r2)+" [Ω]"); valor_r2.draw(window)
    valor_r3 = Button(cyan,630,650,150,30, 'R3 = '+str(calculator.r3)+" [Ω]"); valor_r3.draw(window)
    valor_e1 = Button(azul,780,570,150,30, 'FEM1 = '+str(calculator.fem1)+" [V]"); valor_e1.draw(window)
    valor_e2 = Button(azul,780,610,150,30, 'FEM2 = '+str(calculator.fem2)+" [V]"); valor_e2.draw(window)
    valor_e3 = Button(azul,780,650,150,30, 'FEM3 = '+str(calculator.fem3)+" [V]"); valor_e3.draw(window)
def Symbols():  
    global calculator
    if event.type == pygame.MOUSEBUTTONDOWN:
        for button in symbols:
            if button.isOver(event.pos):
                print(button.text)
                calculator.newOperation(button.text)
        if clearButton.isOver(event.pos):
            calculator = Calculate()          
def MOUSEOVERnumbers():
    if event.type == pygame.MOUSEBUTTONDOWN:
        for button in numbers:
            if button.isOver(event.pos):
                print(button.text)  
                calculator.newDigit(button.text)
class carga():
     def __init__(self,x, y, d, o, r):
         self.car = pygame.image.load('carga.png')
         self.car = pygame.transform.scale(self.car, (30, 30))
         self.car_x = x
         self.car_y = y
         self.direction = d;
         self.orientation = o;
         self.car = pygame.transform.rotate(self.car, r)
cam_x_offset = 0  #la camara o sensor se acomode correctamente cuando se gira en x
cam_y_offset = 0  #la camara o sensor se acomode correctamente cuando se gira en y
focal_dis = 20 #Constante Distancia focal
def creaDatosSupHorario():
    #-I1-I2
    car_1 = carga(82,230,'up','up',0)
    car_2 = carga(82,100,'up','up',0)
    car_3 = carga(160,50,'right','up',-90)
    car_4 = carga(290,50,'right','up',-90)
    car_5 = carga(420,50,'right','up',-90)
    car_6 = carga(526,80,'down','up',-180)
    car_7 = carga(526,210,'down','up',-180)
    car_8 = carga(460,272,'left','up',-270)
    car_9 = carga(330,272,'left','up',-270)
    car_10 = carga(200,272,'left','up',-270)
    cargasSup.append(car_1);cargasSup.append(car_2);cargasSup.append(car_3);cargasSup.append(car_4);
    cargasSup.append(car_5);cargasSup.append(car_6);cargasSup.append(car_7);cargasSup.append(car_8);
    cargasSup.append(car_9);cargasSup.append(car_10);
def creaDatosSupAnti():
    #+I1+I2
    car_1 = carga(82,230,'down','down',-180)
    car_2 = carga(82,100,'down','down',-180)
    car_3 = carga(160,50,'left','down',90)
    car_4 = carga(290,50,'left','down',90)
    car_5 = carga(420,50,'left','down',90)
    car_6 = carga(526,80,'up','down',0)
    car_7 = carga(526,210,'up','down',0)
    car_8 = carga(460,272,'right','down',-90)
    car_9 = carga(330,272,'right','down',-90)
    car_10 = carga(200,272,'right','down',-90)
    cargasSup.append(car_1);cargasSup.append(car_2);cargasSup.append(car_3);cargasSup.append(car_4);
    cargasSup.append(car_5);cargasSup.append(car_6);cargasSup.append(car_7);cargasSup.append(car_8);
    cargasSup.append(car_9);cargasSup.append(car_10);
def creaDatosInfHorario():
    #-I3+I2
    car_11 = carga(82,570,'up','up',0)
    car_12 = carga(82,440,'up','up',0)
    car_13 = carga(160,388,'right','up',-90)
    car_14 = carga(290,388,'right','up',-90)
    car_15 = carga(420,388,'right','up',-90)
    car_16 = carga(528,410,'down','up',-180)
    car_17 = carga(528,540,'down','up',-180)
    car_18 = carga(480,622,'left','up',-270)
    car_19 = carga(350,622,'left','up',-270)
    car_20 = carga(220,622,'left','up',-270)
    cargasInf.append(car_11);cargasInf.append(car_12);cargasInf.append(car_13);cargasInf.append(car_14);
    cargasInf.append(car_15);cargasInf.append(car_16);cargasInf.append(car_17);cargasInf.append(car_18);
    cargasInf.append(car_19);cargasInf.append(car_20);
def creaDatosInfAnti():
    #+I3-I2
    car_11 = carga(82,570,'down','down',-180)
    car_12 = carga(82,440,'down','down',-180)
    car_13 = carga(160,388,'left','down',90)
    car_14 = carga(290,388,'left','down',90)
    car_15 = carga(420,388,'left','down',90)
    car_16 = carga(528,410,'up','down',0)
    car_17 = carga(528,540,'up','down',0)
    car_18 = carga(480,622,'right','down',-90)
    car_19 = carga(350,622,'right','down',-90)
    car_20 = carga(220,622,'right','down',-90)
    cargasInf.append(car_11);cargasInf.append(car_12);cargasInf.append(car_13);cargasInf.append(car_14);
    cargasInf.append(car_15);cargasInf.append(car_16);cargasInf.append(car_17);cargasInf.append(car_18);
    cargasInf.append(car_19);cargasInf.append(car_20);
def runCircuito(estructuraCargas):
    for cargas in estructuraCargas:
        cam_x = cargas.car_x + cam_x_offset + 15
        cam_y = cargas.car_y + cam_y_offset + 15
    
        up_px = window.get_at((cam_x, cam_y - focal_dis))[0]
        down_px = window.get_at((cam_x, cam_y + focal_dis))[0]
        right_px = window.get_at((cam_x + focal_dis, cam_y))[0]
        left_px = window.get_at((cam_x - focal_dis, cam_y))[0]
        if(cargas.orientation == 'up'): #horario
            if(cargas.direction == 'up' and up_px != 255 and right_px == 255):
                cargas.direction = 'right'
                cargas.car = pygame.transform.rotate(cargas.car, -90)  #Imagen rota a la derecha                
            elif(cargas.direction == 'right' and right_px != 255 and down_px == 255):
                cargas.direction = 'down'
                cargas.car = pygame.transform.rotate(cargas.car, -90)               
            elif(cargas.direction == 'down' and down_px != 255 and left_px == 255):
                cargas.direction = 'left'
                cargas.car = pygame.transform.rotate(cargas.car, -90)                
            elif(cargas.direction == 'left' and left_px != 255 and up_px == 255):
                cargas.direction = 'up'
                cargas.car = pygame.transform.rotate(cargas.car, -90)
        else:   #antihorario
            if(cargas.direction == 'down' and down_px != 255 and right_px == 255):
                cargas.direction = 'right'
                cargas.car = pygame.transform.rotate(cargas.car, 90)
            elif(cargas.direction == 'right' and right_px != 255 and up_px == 255):
                cargas.direction = 'up'
                cargas.car = pygame.transform.rotate(cargas.car, 90)  #Imagen rota a la izquierda
            elif(cargas.direction == 'up' and up_px != 255 and left_px == 255):
                cargas.direction = 'left'
                cargas.car = pygame.transform.rotate(cargas.car, 90)               
            elif(cargas.direction == 'left' and left_px != 255 and down_px == 255):
                cargas.direction = 'down'
                cargas.car = pygame.transform.rotate(cargas.car, 90)
        if(cargas.direction == 'up' and up_px == 255):
            cargas.car_y = cargas.car_y - 2
        elif(cargas.direction == 'right' and right_px == 255):
            cargas.car_x = cargas.car_x + 2
        elif(cargas.direction == 'down' and down_px == 255):
            cargas.car_y = cargas.car_y + 2
        elif(cargas.direction == 'left' and left_px == 255):
            cargas.car_x = cargas.car_x - 2
        window.blit(cargas.car, (cargas.car_x, cargas.car_y))
        pygame.draw.circle(window, (0, 255, 0), (cam_x, cam_y), 5, 5)
fuente1 = pygame.font.SysFont("arial black", 32)
fuente2 = pygame.font.SysFont("consolas", 30)
while run:
    window.fill(((128,128,192)))
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            run = False
        MOUSEOVERnumbers()
        Symbols()
    window.blit(track, (0, 0)) #fondo
    clock.tick(60) 
    redraw()
    texto1 = fuente1.render('Corriente I1: '+str(calculator.i1)+' [A]',True, blanco, naranja)
    texto2 = fuente1.render("Corriente I2: "+str(calculator.i2)+" [A]",True, blanco, verde)
    texto3 = fuente1.render("Corriente I3: "+str(calculator.i3)+" [A]",True, blanco, silver)
    window.blit(texto1, (680,400))
    window.blit(texto2, (680,450))
    window.blit(texto3, (680,500)) 
    if(calculator.i1 < 0 and sw):
        creaDatosSupHorario()
        flechaSup = pygame.image.load('flechaArriba.png')
        flechaSup = pygame.transform.scale(flechaSup, (20, 100))
    elif(calculator.i1 > 0 and sw):
        creaDatosSupAnti()
        flechaSup = pygame.image.load('flechaAbajo.png')
        flechaSup = pygame.transform.scale(flechaSup, (20, 100))
    if(calculator.i2 < 0 and sw):
        flechaMed = pygame.image.load('flechaIzquierda.png')
        flechaMed = pygame.transform.scale(flechaMed, (60, 20))
    elif(calculator.i2 > 0 and sw):
        flechaMed = pygame.image.load('flechaDerecha.png')
        flechaMed = pygame.transform.scale(flechaMed, (60, 20))
    if(calculator.i3 < 0 and sw):
        sw = False
        creaDatosInfHorario()
        flechaInf = pygame.image.load('flechaArriba.png')
        flechaInf = pygame.transform.scale(flechaInf, (20, 100))
    elif(calculator.i3 > 0 and sw):
        sw = False
        creaDatosInfAnti()
        flechaInf = pygame.image.load('flechaAbajo.png')
        flechaInf = pygame.transform.scale(flechaInf, (20, 100))
    if(sw == False):
        window.blit(flechaSup, (45,100))
        window.blit(flechaInf, (45,460))
        window.blit(flechaMed, (5,380))
    runCircuito(cargasSup)
    runCircuito(cargasInf)
    pygame.display.update()
pygame.quit() #Cierra PyGame
