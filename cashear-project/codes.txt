# import libraries
from tkinter import * 
from tkinter import messagebox
from abc import ABC, abstractmethod
from datetime import *
import time
from tkinter import font as tkFont

# varibles to creat opgects
num_orders = 0
order = {}
num_phone_orders = 0
phone_order = {}

# class
class orders:
	def __init__(self, item, tybe, size, adds, number, coom):
		global num_orders
		num_orders += 1
		self.__num = self.get_num()
		self.__item = item
		self.__tybe = tybe
		self.__adds = adds
		self.__number = number
		self.__size = size
		self.__coom = coom
		# get date and time
		now = datetime.now()
		today = date.today()
		self.__date_time = ((today.strftime("%d/%m/%Y"))+" "+(now.strftime("%H:%M:%S")))
		self.__price = self.get_price()
		print(self.desplay())

	def get_price(self):
		i=0
		i = self.__adds

		if self.__item == "PIZZA":
			if (self.__tybe) == "beef":
				i+=15
			else :
				pass
			if (self.__size) == "SMALL":
				i+=55
			elif (self.__size) == "MEDIUM":
				i+=65
			else :
				i+=75
		else:
			if (self.__tybe) == "beef":
				i+=10
			else :
				pass
			if (self.__size) == "SMALL":
				i+=25
			elif (self.__size) == "MEDIUM":
				i+=30
			else :
				i+=35
		i *= self.__number
		return i

	def get_num(self):
		return num_orders

	def desplay(self):
		return f"{self.__num}    {self.__item}    {self.__tybe}    {self.__size}    \
{self.__adds}    {self.__number}    {self.__price}    {self.__coom}    {self.__date_time}"


class phone_orders(orders):
	def __init__(self, p_name, phone, addres, item, tybe, size, adds, number, coom):
		self.__p_name = p_name
		self.__phone = phone
		self.__addres = addres
		global num_orders
		num_orders-=1
		global num_phone_orders
		num_phone_orders+=1
		super().__init__(item, tybe, size, adds, number, coom)

	def get_price(self):
		i = super().get_price()
		i += 15
		return i

	def get_num(self):
		return num_phone_orders

	def desplay(self):
		return ((super().desplay()) + f"    {self.__p_name}    {self.__phone}    {self.__addres}")



# functions
def create_order():
	item = choose_item.get()
	tybe = choose_type.get()
	size = choose_size.get()
	adds = int(adds_box.get())
	coom = coom_box.get()
	number = int(number_box.get())
	name = f'obj_{num_orders}_order'
	order[name] = order.get(name, orders(item, tybe, size, adds, number, coom))
	or_lab = Label(root_frame_u, text=(order[f'obj_{num_orders-1}_order'].desplay())\
, fg = "black",font=regular1)
	or_lab.place(relwidth = 1, relheight = 1)

def create_phone_order():
	item = choose_item.get()
	tybe = choose_type.get()
	size = choose_size.get()
	adds = int(adds_box.get())
	coom = coom_box.get()
	p_name = name_box.get()
	phone = phone_box.get()
	addres = addres_box.get()
	number = int(number_box.get())
	name = f'obj_{num_phone_orders}_phone_order'
	phone_order[name] = phone_order.get(name, phone_orders(p_name, phone, addres, item, tybe, size, adds, number, coom))
	or_lab = Label(root_frame_u, text=(phone_order[f'obj_{num_phone_orders-1}_phone_order'].desplay())\
, fg = "black",font=regular2)
	or_lab.place(relwidth = 1, relheight = 1)

def show_all():
	show = Toplevel()
	show.title("ALL ORDERS")
	show.geometry("1300x400")

	scrollbar = Scrollbar(show)
	scrollbar.pack( side = RIGHT, fill = Y )

	mylist = Listbox(show, yscrollcommand = scrollbar.set , fg = "black",font=regular2)
	mylist.insert(END, "all orders:")
	for i in range(0,num_orders):
		mylist.insert(END, (order[f'obj_{i}_order'].desplay()))
		i+=1
	
	mylist.insert(END, "")
	mylist.insert(END, "all phone orders:")
	for i in range(0,num_phone_orders):
		mylist.insert(END, (phone_order[f'obj_{i}_phone_order'].desplay()))
		i+=1


	mylist.pack( side = LEFT,fill=BOTH,expand=True)
	scrollbar.config( command = mylist.yview )

	


# root start
root = Tk()

# head of the project
root.title("Halab Al Shahba")
root.iconbitmap(r'C:\Users\kbara\Desktop\KHALED\projects\paython\cashear-project\img/icon2.ico')
root.state('zoomed')
root.configure(bg='#AB2626')
regular = tkFont.Font(family='Helvetica', size=25, weight=tkFont.BOLD)
regular1 = tkFont.Font(family='Helvetica', size=17, weight=tkFont.BOLD)
regular2 = tkFont.Font(family='Helvetica', size=12, weight=tkFont.BOLD)
frame_now = "order"

# frames
root_frame_r = Frame(root)
root_frame_r.place(relwidth=0.20, relheight= 0.88, relx= 0.795, rely= 0.11)
root_frame_u = Frame(root)
root_frame_u.place(relwidth=0.99, relheight= 0.09, relx= 0.005, rely= 0.01)
order_frame = Frame(root)
order_frame.place(relwidth=0.785, relheight= 0.88, rely= 0.11, relx= 0.005 )


# components
order_button = Button(root_frame_r, bg= "red", text= "order",\
   fg = "yellow",font=regular, command = create_order)
phone_order_button = Button(root_frame_r, bg= "red", text= "phone order",\
   fg = "yellow",font=regular, command = create_phone_order)
show_orders_button = Button(root_frame_r, bg= "red", text= "show all orders",\
   fg = "yellow",font=regular, command = show_all)

save_label = Label(root_frame_r, text= "save as", fg = "black",font=regular)
order_label = Label(order_frame, text= "order", fg = "black",font=regular)
choose_item = StringVar()
choose_item.set('choose item')
choose_type = StringVar()
choose_type.set('choose type')
choose_size = StringVar()
choose_size.set('choose size')
item_box = OptionMenu(order_frame, choose_item, 'SANDWICH', 'PIZZA')
type_box = OptionMenu(order_frame, choose_type, 'CHICKEN', 'BEEF')
size_box = OptionMenu(order_frame, choose_size, 'SMALL', 'MEDIUM', 'LARGE')
adds_label = Label(order_frame, text= 'additions')
adds_box = Spinbox(order_frame, from_=0, to=100)
number_label = Label(order_frame, text= "quntity")
number_box = Spinbox(order_frame, from_=0, to=100)
name_box = Entry(order_frame)
phone_box = Entry(order_frame)
addres_box = Entry(order_frame)
coom_box = Entry(order_frame)
name_box.insert(0, 'inter customer name')
phone_box.insert(0, 'inter customer phone')
addres_box.insert(0, 'inter customer addres')
coom_box.insert(0, 'inter any comment')


# components.place()
save_label.place(relwidth = 0.90, relheight = 0.1, relx= 0.05)
order_button.place(relwidth = 0.90, relheight = 0.20, relx= 0.05, rely= 0.1)
phone_order_button.place(relwidth = 0.90, relheight = 0.20, relx= 0.05, rely= 0.34)
show_orders_button.place(relwidth = 0.90, relheight = 0.20, relx= 0.05, rely= 0.75)

order_label.place(relwidth = 0.1, relheight = 0.1, relx= 0.45)
item_box.place(relwidth = 0.25, relheight = 0.15, relx= 0.0625, rely= 0.20)
type_box.place(relwidth = 0.25, relheight = 0.15, relx= 0.375, rely= 0.20)
size_box.place(relwidth = 0.25, relheight = 0.15, relx= 0.6875, rely= 0.20)
adds_box.place(relwidth = 0.25, relheight = 0.08, relx= 0.0625, rely= 0.53)
number_box.place(relwidth = 0.25, relheight = 0.08, relx= 0.375, rely= 0.53)
adds_label.place(relwidth = 0.25, relheight = 0.07, relx= 0.0625, rely= 0.45)
number_label.place(relwidth = 0.25, relheight = 0.07, relx= 0.375, rely= 0.45)
name_box.place(relwidth = 0.25, relheight = 0.15, relx= 0.6875, rely= 0.45)
phone_box.place(relwidth = 0.25, relheight = 0.15, relx= 0.0625, rely= 0.70)
addres_box.place(relwidth = 0.25, relheight = 0.15, relx= 0.375, rely= 0.70)
coom_box.place(relwidth = 0.25, relheight = 0.15, relx= 0.6875, rely= 0.70)


root.mainloop()
# root end



