# PYTHON
from tkinter import * #используем библиотеку tkinter
import random
import time


player_score = 0 #переменная счёт игрока

tk = Tk()
tk.title(" KUVALDIN PING PONG")# название игры
tk.resizable(0, 0)
tk.wm_attributes("-topmost", 1)
canvas = Canvas(tk, width=800, height=600, bd=0,#размер окна/холста
highlightthickness=0)
canvas.pack()
tk.update()
p_text = canvas.create_text(60,25,
                         text='score = %s' %(player_score),
                         font="Arial 15",
                         fill="black") #создаем текст для счетчика, шрифт и цвет
canvas.create_text(80,570,text="press \"RIGHT\" to move paddle right, press \"LEFT\" to move paddle left, press \"DOWN\" to stop paddle ",
          font="Verdana 10",anchor="w",fill="black") #создаем текст-инструкцию для управления ракеткой(paddle)

class Ball: #здесь будут хранится все параметры для объекта мяч
    def __init__(self, canvas, paddle, block1, block2, block3, block4, block5, block6, block7, block8, block9, block10, color):#инициализирует себя, и взаимодействие с остальными объектами
        self.canvas = canvas
        self.paddle = paddle
        self.block1 = block1
        self.block2 = block2
        self.block3 = block3
        self.block4 = block4
        self.block5 = block5
        self.block6 = block6
        self.block7 = block7
        self.block8 = block8
        self.block9 = block9
        self.block10 = block10
        self.id = canvas.create_oval(10, 10, 25, 25, fill=color)# задаем конкретные параметры мяча(форма, размер, заливка цветом)
        self.canvas.move(self.id, 245, 100) #место, откуда начнется его движение
        starts = [-3, -2, -1, 1, 2, 3]#создали переменную starts, поместив туда список из шести чисел
        random.shuffle(starts)#перемешали элементы списка с помощью random.shuffle
        self.x = starts[0]#Теперь в x может попасть любое значение из исходного списка, от –3 до 3
        self.y = -3#помещаем в свойство y значение –3 (чтобы ускорить движение мяча)
        self.canvas_height = self.canvas.winfo_height()
        self.canvas_width = self.canvas.winfo_width()#существование в пределах ширины и высоты холста
        self.hit_bottom = False #если попал вниз, возвращает значение false, не отскакивает
        
   
    def draw(self):
        self.canvas.move(self.id, self.x, self.y)
        pos = self.canvas.coords(self.id)
        if pos[1] <= 0:
            self.y = 3#мяч двигается с одинаковой скоростью( задали скорость) по оси 
        if pos[3] >= self.canvas_height:#отскакивает от верхней границы холста
            self.hit_bottom = True
        if self.hit_paddle(pos) == True:#отскок от ракетки
            self.y = -3
        if pos[0] <= 0:
            self.x = 3#мяч двигается с одинаковой скоростью( задали скорость) по оси
        if pos[2] >= self.canvas_width: #отскакивает от стенки
            self.x = -3
        if self.hit_block(pos) == True:
            self.y = +3#при отскоке от блока направление меняется с прежней скоростью
            global player_score#без global не работает счетчик
            player_score += 10#если отскочил от блока то счетчик увеличивается на 10 единиц
            self.canvas.itemconfig(p_text, text='score = %s' %(player_score))#itemconfig для работы счетчика
       
            

    def hit_paddle(self, pos):#объявляем функцию с аргументом pos, в котором будем передавать текущие координаты мяча
        paddle_pos = self.canvas.coords(self.paddle.id)#получаем координаты ракетки и сохраняем их в переменной paddle_pos
        if pos[2] >= paddle_pos[0] and pos[0] <= paddle_pos[2]:#конструкция if, условие которой означает:
#x-координата правой стороны мяча больше, чем x-координата левой стороны ракетки, и x-координата левой стороны мяча меньше, чем x-координата правой стороны ракетки?
#Здесь pos[2] соответствует x-координате правой стороны мяча, а pos[0] — x-координате его левой стороны.
#При этом paddle_pos[0] соответствует x-координате левой стороны ракет-ки, а paddle_pos[2] — х-координате ее правой стороны. 
            if pos[3] >= paddle_pos[1] and pos[3] <= paddle_pos[3]:#проверяем, не находится ли нижняя сторона мяча (pos[3]) между верхом ракетки (paddle_pos[1]) и ее низом (paddle_pos[3])
                return True
        return False
    
    def hit_block(self,pos):#по такому же принципу описываем блоки
        block1_pos = self.canvas.coords(self.block1.id)
        if block1_pos and (pos[2] >= block1_pos[0] and pos[0] <= block1_pos[2]):
            if block1_pos and(pos[3] >= block1_pos[1] and pos[3] <= block1_pos[3]):
                self.canvas.delete(self.block1.id)#при попадании мяча блок удаляется
                return True

        block2_pos = self.canvas.coords(self.block2.id)
        if block2_pos and (pos[2] >= block2_pos[0] and pos[0] <= block2_pos[2]):
            if block2_pos and(pos[3] >= block2_pos[1] and pos[3] <= block2_pos[3]):
                self.canvas.delete(self.block2.id)

                return True

        block3_pos = self.canvas.coords(self.block3.id)
        if block3_pos and (pos[2] >= block3_pos[0] and pos[0] <= block3_pos[2]):
            if block3_pos and(pos[3] >= block3_pos[1] and pos[3] <= block3_pos[3]):
                self.canvas.delete(self.block3.id)

                return True

        block4_pos = self.canvas.coords(self.block4.id)
        if block4_pos and (pos[2] >= block4_pos[0] and pos[0] <= block4_pos[2]):
            if block4_pos and(pos[3] >= block4_pos[1] and pos[3] <= block4_pos[3]):
                self.canvas.delete(self.block4.id)

                return True

        block5_pos = self.canvas.coords(self.block5.id)
        if block5_pos and (pos[2] >= block5_pos[0] and pos[0] <= block5_pos[2]):
            if block5_pos and(pos[3] >= block5_pos[1] and pos[3] <= block5_pos[3]):
                self.canvas.delete(self.block5.id)

                return True
        block6_pos = self.canvas.coords(self.block6.id)
        if block6_pos and (pos[2] >= block6_pos[0] and pos[0] <= block6_pos[2]):
            if block6_pos and(pos[3] >= block6_pos[1] and pos[3] <= block6_pos[3]):
                self.canvas.delete(self.block6.id)

                return True
        block7_pos = self.canvas.coords(self.block7.id)
        if block7_pos and (pos[2] >= block7_pos[0] and pos[0] <= block7_pos[2]):
            if block7_pos and(pos[3] >= block7_pos[1] and pos[3] <= block7_pos[3]):
                self.canvas.delete(self.block7.id)

                return True
        block8_pos = self.canvas.coords(self.block8.id)
        if block8_pos and (pos[2] >= block8_pos[0] and pos[0] <= block8_pos[2]):
            if block8_pos and(pos[3] >= block8_pos[1] and pos[3] <= block8_pos[3]):
                self.canvas.delete(self.block8.id)

                return True
        block9_pos = self.canvas.coords(self.block9.id)
        if block9_pos and (pos[2] >= block9_pos[0] and pos[0] <= block9_pos[2]):
            if block9_pos and(pos[3] >= block9_pos[1] and pos[3] <= block9_pos[3]):
                self.canvas.delete(self.block9.id)

                return True
        block10_pos = self.canvas.coords(self.block10.id)
        if block10_pos and (pos[2] >= block10_pos[0] and pos[0] <= block10_pos[2]):
            if block10_pos and(pos[3] >= block10_pos[1] and pos[3] <= block10_pos[3]):
                self.canvas.delete(self.block10.id)#при попадании мяча блок удаляется
                return True
        return False

           

class Paddle:
    def __init__(self, canvas, color):
        self.canvas = canvas
        self.id = canvas.create_rectangle(0, 0, 100, 10, fill=color)
        self.canvas.move(self.id, 400, 500)#местоположение ракетки
        self.x = 0
        self.canvas_width = self.canvas.winfo_width()
        self.canvas.bind_all('<KeyPress-Left>', self.turn_left)#управление ракеткой
        self.canvas.bind_all('<KeyPress-Right>', self.turn_right)
        self.canvas.bind_all('<KeyPress-Down>', self.turn_down)
    def draw(self):
        self.canvas.move(self.id, self.x, 0)
        pos = self.canvas.coords(self.id)
        if pos[0] <= 0:
            self.x = 0
        elif pos[2] >= self.canvas_width:
            self.x = 0
    def turn_left(self, evt):
        self.x = -3
    def turn_right(self, evt):
        self.x = 3
    def turn_down(self, evt):
        self.x = 0

class Block:
    def __init__(self, canvas, x, y, color):
        self.canvas = canvas
        self.x = int(x)
        self.y = int(y)
        self.id = canvas.create_rectangle(self.x, self.y, self.x + 50, self.y + 10, fill=color)
        self.canvas.move(self.id, self.x, self.y)
        


block1 = Block(canvas, 50, 25, 'orange')#рисуем блоки в заданное место холста, задаем цвет
block2 = Block(canvas, 90, 25, 'yellow')
block3 = Block(canvas, 130, 25, 'green')
block4 = Block(canvas, 170, 25, 'blue')
block5 = Block(canvas, 210, 25, 'violet')
block6 = Block(canvas, 250, 25, 'black')
block7 = Block(canvas, 290, 25, 'gray')
block8 = Block(canvas, 330, 25, 'purple')
block9 = Block(canvas, 370, 25, 'brown')
block10 = Block(canvas, 10, 25, 'pink')
paddle = Paddle(canvas, 'blue')
ball = Ball(canvas, paddle, block1, block2, block3, block4, block5, block6, block7, block8, block9, block10,'red')


while 1:
    if ball.hit_bottom == False:
        ball.draw()
        paddle.draw()
    tk.update_idletasks()
    tk.update()
    time.sleep(0.01)
    if ball.hit_bottom == True:
        canvas.create_text(300,300,text="GAME\nOVER",
          font="Verdana 50",anchor="w",justify=CENTER,fill="red")
    if player_score == 100:#костыль! Если счетчик равен количеству всех заработанных от разбитых блоков очков, то вы победили!
        canvas.create_text(300,300,text="YOU\nWIN!",
          font="Verdana 50",anchor="w",justify=CENTER,fill="green")
        break #выходим из цикла(конец игры)
