from app import App
import asyncio
from app_components import Menu, Notification, clear_background
from events.input import Buttons, BUTTON_TYPES
from tildagonos import tildagonos, led_colours
import urequests

#
main_menu_items = [" ", "A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "Y", "Z", "0", "1", "2", "3", "4", "5", "6", "7", "8", "9"]
class SnakeApp(App):
    def __init__(self):
        self.menu = Menu(
            self,
            main_menu_items,
            select_handler=self.select_handler,
            back_handler=self.back_handler,
        )
        self.notification = None
        self.selected_letters = []
        self.button_states = Buttons(self)
        self.lowercase_toggle = False
        self.input_sequence = ''

    def select_handler(self, item, item_index):
        if self.lowercase_toggle == True:
            item = item.lower()
        else:
            item = item.upper()
        self.selected_letters.append(item)
        sequence = ''.join(self.selected_letters)
        self.input_sequence = sequence  # update the input sequence with each letter pressed
        if len(self.selected_letters) == 4:
            self.notification = Notification(sequence + '!')
            print(sequence)
            self.send_word(sequence)
            self.selected_letters = []  # reset the list of selected letters
        else:
           self.notification = Notification(item + '!')


    def send_word(self, word):
        print(f'Word: {word}')
        wxrd = word.replace(' ', 'x')
        wxrd = wxrd.replace('-', '£')
        url = 'https://trigger.comnoco.com/t/5a6579e7-3666-4aa2-b94f-d743535bb1a9/DisplayWord'
        headers = {'accept': 'application/json', 'Content-Type': 'application/json'}
        data = {'Word': wxrd}
        response = urequests.post(url, headers=headers, json=data)
        print(f'Data sent: {data}')
        print(response)  # print the response object
        print(response.text)  # print the response from the server

    def back_handler(self):
        self.minimise()

    def draw(self, ctx):
        clear_background(ctx)
        if self.lowercase_toggle:
            self.menu.items = [item.lower() for item in main_menu_items]
        else:
            self.menu.items = [item.upper() for item in main_menu_items]
        self.menu.draw(ctx)
        if self.notification:
            self.notification.draw(ctx)
        guifont = 0
        inputfont = 0
        ctx.font = ctx.get_font_name(inputfont)
        ctx.rgb(255, 255, 255).move_to(60,0).text(self.input_sequence)
        ctx.font = ctx.get_font_name(guifont)
        ctx.rgb(255, 255, 255).move_to(-70,60).text('←DEL')
        ctx.font = ctx.get_font_name(guifont)
        ctx.rgb(255, 255, 255).move_to(60,60).text('SEND→')
        ctx.font = ctx.get_font_name(guifont)
        ctx.rgb(255, 255, 255).move_to(60,-80).text('CAPS→')
        ctx.font = ctx.get_font_name(guifont)
        ctx.rgb(255, 255, 255).move_to(60,-60).text('LOCK')
        ctx.font = ctx.get_font_name(guifont)
        ctx.rgb(255, 255, 255).move_to(-60,-60).text('QUIT')

    def update(self, delta):
        self.menu.update(delta)
        if self.notification:
            self.notification.update(delta)
        if self.button_states.get(BUTTON_TYPES["RIGHT"]):
            self.lowercase_toggle = not self.lowercase_toggle  # toggle the value
            if self.lowercase_toggle:
                self.notification = Notification('Lowercase')
                  # change menu items to lowercase
            else:
                self.notification = Notification('Uppercase')
                 # change menu items to uppercase
            self.button_states.clear()

        
        #backspace implementation
        if self.button_states.get(BUTTON_TYPES["LEFT"]):
            self.selected_letters = self.selected_letters[:-1]
            self.input_sequence = ''.join(self.selected_letters)
            self.notification = Notification('Backspace')
            self.button_states.clear()

__app_export__ = SnakeApp


