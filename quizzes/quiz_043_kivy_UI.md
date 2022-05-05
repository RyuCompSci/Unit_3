### create a simple UI on kivy

#### python code

```.py
from kivy.lang import Builder
from kivymd.app import MDApp
from kivymd.uix.screen import MDScreen


class MainScreen(MDScreen):
    def attempt_new(self):
        self.parent.current = "ItemScreen"

    def attempt_list(self):
        pass

    def attempt_logout(self):
        self.parent.current = "LoginScreen"


class LoginScreen(MDScreen):
    ''' This class creates the log in window for the application'''

    def attempt_login(self):
        print("User tried to log in.")
        self.parent.current = "MainScreen"


class ItemScreen(MDScreen):
    def back_login(self):
        self.parent.current = "MainScreen"


class quiz_43(MDApp):
    def build(self):
        return


m = quiz_43()
m.run()
```

#### kivy code

```.py
ScreenManager:
    # controls all the screens
    id: scr_manager
    size: 500, 300

    LoginScreen:
        name: "LoginScreen"

    MainScreen:
        name: "MainScreen"

    ItemScreen:
        name: "ItemScreen"



<LoginScreen>:
    # button to main screen
    MDRaisedButton:
        text: "Log in"
        on_release:
            root.attempt_login()
        size_hint: .2, .1
        size_hint: .5, .1
        font_size: "20px"
        pos_hint: {"center_x":.5, "center_y":.2}


<MainScreen>
    MDLabel:
        id: main_label_text
        text: "Main Window"
        font_size: "30px"
        size_hint: .3, .3
        pos_hint: {"center_x":.55, "center_y":.8}

    MDRaisedButton:
        # button to item screen
        id: new_button
        text: "New"
        on_release:
            root.attempt_new()
        size_hint: .5, .1
        pos_hint: {"center_x":.5, "center_y":.5}

    MDRaisedButton:
        # empty button
        text: "List"
        on_release:
            root.attempt_list()
        size_hint: .5, .1
        pos_hint: {"center_x":.5, "center_y":.35}

    MDRaisedButton:
        # button to go back to login screen
        text: "Log out"
        on_release:
            root.attempt_logout()
        size_hint: .5, .1
        pos_hint: {"center_x":.5, "center_y":.2}

<ItemScreen>
    MDLabel:
        id: main_label_text
        text: "New"
        font_size: "30px"
        size_hint: .1, .1
        pos_hint: {"center_x":.5, "center_y":.8}

    MDRaisedButton:
        # button to main screen
        text: "Back"
        on_release:
            root.back_login()
        size_hint: .5, .1
        font_size: "20px"
        pos_hint: {"center_x":.5, "center_y":.2}
```



https://user-images.githubusercontent.com/89348765/166895911-8f747f98-2f1f-44cd-bbe4-8b4c564c6048.mov




