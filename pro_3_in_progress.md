```.py
import datetime

from kivy.lang import Builder
from kivy.metrics import dp

from kivymd.app import MDApp
from kivymd.uix.datatables import MDDataTable
from kivy.core.window.window_sdl2 import WindowSDL
from kivymd.uix.screen import MDScreen
from kivymd.uix.pickers import MDDatePicker
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.schema import Column
from sqlalchemy.types import Integer, String
from passlib.context import CryptContext

db_engine = create_engine = create_engine("sqlite:///stop&clean.db")
Base = declarative_base()


class User_database(Base):
    __tablename__ = "user_info"  # name the table
    user_id = Column(Integer, primary_key=True)
    user_name = Column(String(255))
    email = Column(String(255))
    password = Column(Integer)


class Stop_and_clean_database(Base):
    __tablename__ = "user_item"
    stop_clean_id = Column(Integer, primary_key=True)
    date = Column(String(255))
    person_on_duty = Column(String(255))
    duration = Column(Integer)
    task = Column(String(255))
    feedback = Column(String(255))
    rating = Column(Integer)
    start_time = Column(Integer)
    tool = Column(String(255))


Base.metadata.create_all(db_engine)

dbsession = sessionmaker(db_engine)  # Create a class for session
session = dbsession()


class stop_clean(MDApp):
    def build(self):
        return


class LSScreen(MDScreen):
    def go_login(self):
        self.parent.current = "LoginScreen"

    def go_signup(self):
        self.parent.current = "RegisterScreen"


class LoginScreen(MDScreen):
    ''' This class creates the log in window for the application'''

    def __init__(self, **kw):
        super().__init__(**kw)
        self.pwd_context = CryptContext(
            schemes=["pbkdf2_sha256"],
            default="pbkdf2_sha256",
            pbkdf2_sha256__default_rounds=30000
        )

    def encrypt_password(self, password):
        return self.pwd_context.hash(password)

    def check_password(self, password, hashed):
        return self.pwd_context.verify(password, hashed)

    def try_login(self):
        print("User tried to log in.")
        email_entered = self.ids.email.text
        password_entered = self.ids.password.text
        print(f"password_entered: {password_entered}")
        all_user = session.query(User_database).all()
        for u in all_user:
            print(u.user_id, u.user_name, u.email, u.password)
        result = session.query(User_database).filter(User_database.email == email_entered).first()
        print(result)
        if result:
            pass_hash = result.password
            if self.check_password(password_entered, pass_hash):
                self.parent.current = "MainScreen"
            else:
                u = False
                self.ids.login_label_text.text = "Either your email or password is wrong"
        else:
            u = False
            self.ids.login_label_text.text = "Either your email or password is wrong"


class RegisterScreen(MDScreen):
    def __init__(self, **kw):
        super().__init__(**kw)
        self.pwd_context = CryptContext(
            schemes=["pbkdf2_sha256"],
            default="pbkdf2_sha256",
            pbkdf2_sha256__default_rounds=30000
        )

    def encrypt_password(self, password):
        return self.pwd_context.hash(password)

    def try_signup(self):
        print("User is trying to sign up")
        encrypted_password = self.encrypt_password(self.ids.reg_password.text)
        user_a = User_database(user_name=self.ids.reg_name.text, email=self.ids.reg_email.text,
                               password=encrypted_password)
        session.add(user_a)
        session.commit()
        self.parent.current = "LoginScreen"


class MainScreen(MDScreen):
    def go_item(self):
        self.parent.current = "NewItemScreen"

    def go_history(self):
        self.parent.current = "TableScreen"


class NewItemScreen(MDScreen):
    def on_save(self, instance, value, date_range):
        self.ids.date_label.text = str(value)

    def on_cancel(self, instance, value):
        pass

    def show_date_picker(self):
        date_dialog = MDDatePicker(year=2022, month=4, day=1)
        date_dialog.bind(on_save=self.on_save, on_cancel=self.on_cancel)
        date_dialog.open()

    def enter_item(self):
        user_a = Stop_and_clean_database(date=self.ids.date_label.text, person_on_duty=self.ids.person_duty.text,
                                         duration=self.ids.duration.value, task=self.ids.task.text,
                                         feedback=self.ids.feedback.text, rating=self.ids.rating.value,
                                         start_time=self.ids.start_time.text, tool=self.ids.tool.text)
        session.add(user_a)
        session.commit()

    def back_main(self):
        self.parent.current = "MainScreen"


class TableScreen(MDScreen):
    def on_pre_enter(self, *args):
        db = Stop_and_clean_database("stop&clean.db")
        data = session.query(Stop_and_clean_database).all()
        session.commit()

        # The MDtable needs at least two values as input
        if len(data) < 2:
            data.append(["", "", "", "", "", "", "", ""])

        self.data_tables = MDDataTable(
            size_hint=(0.8, 0.5),
            pos_hint={"center_x": 0.5, "top": 0.8},
            column_data=[
                ("stop_and_clean_id", 80),
                ("person_on_duty", 80),
                ("duration", 80),
                ("task", 80),
                ("feedback", 80),
                ("rating", 80),
                ("start_time", 80),
                ("tool", 80)
            ],
            row_data=data
        )
        # add the function to detect when a check in a row is pressed
        self.add_widget(self.data_tables)


m = stop_clean()
m.run()
```

```.py
ScreenManager:
    id: scr_manager
    size: 500, 300

    LSScreen:
        name: "LSScreen"

    LoginScreen:
        name: "LoginScreen"

    MainScreen:
        name: "MainScreen"

    TableScreen:
        name: "TableScreen"

    RegisterScreen:
        name: "RegisterScreen"

    NewItemScreen:
        name: "NewItemScreen"


<LSScreen>:
    FitImage:
        source: "stop_and_clean2.jpeg"

    MDCard:
        size_hint: 1, 1
        md_bg_color: .2, .6, .6, .6
        elevation: 10

    MDBoxLayout:
        size_hint: 0.6, 0.8
        orientation: "vertical"
        pos_hint: {"center_x":.5}

        MDLabel:
            id: login_label_text
            text: "   Stop and Clean record manager"
            font_style: "Caption"
            font_size: "30px"
            size_hint: 1, .2

        MDRaisedButton:
            text: "Sign up"
            on_release:
                root.go_signup()
            size_hint: .5, .1
            font_size: "20px"
            pos_hint: {"center_x":.5, "center_y":.5}
            md_bg_color: .6, .3, .3, 1

        MDLabel:
            size_hint: 1, .05

        MDRaisedButton:
            text: "Log in"
            on_release:
                root.go_login()
            size_hint: .5, .1
            font_size: "20px"
            pos_hint: {"center_x":.5, "center_y":.4}
            md_bg_color: .6, .5, .3, 1

        MDLabel:
            size_hint: 1, .1


<LoginScreen>:
    FitImage:
        source: 'st_cl.jpeg'

    MDCard:
        size_hint: .5, .8
        pos_hint: {"center_x":.5, "center_y":.45}
        md_bg_color: .2, .5, .6, .6

    MDBoxLayout:
        size_hint: 0.4, 0.8
        orientation: "vertical"
        pos_hint: {"center_x":.5}

        MDLabel:
            id: login_label_text
            text: "Log in"
            font_size: "20px"
            halign: 'center'
            size_hint: 1, .3

        MDTextField:
            id: email
            hint_text: "email"
            color_mode: 'custom'
            line_color_focus: 1, 1, 0, 1
            icon_right: "email"
            mode: "rectangle"
            size_hint: 1, .1

        MDLabel:
            size_hint: 1, .03

        MDTextField:
            id: password
            password: True
            hint_text: "password"
            color_mode: 'custom'
            line_color_focus: 1, 1, 0, 1
            icon_right: "eye-off"
            mode: "rectangle"
            size_hint: 1, .1

        MDLabel:
            size_hint: 1, .1

        MDRaisedButton:
            text: "enter"
            on_release:
                root.try_login()
            size_hint: .2, .07
            md_bg_color: 0.25, 0, 5, .3

        MDLabel:
            size_hint: 1, .1


<MainScreen>:
    MDCard:
        size_hint: 1, 1
        md_bg_color: .2, .6, .6, .6

    MDBoxLayout:
        size_hint: 1, 0.8
        orientation: "vertical"
        pos_hint: {"center_x":.5}

        MDLabel:
            id: login_label_text
            text: "Welcome to Stop and Clean record manager"
            halign: 'center'
            font_style: "Caption"
            font_size: "30px"
            size_hint: 1, .2

        MDRaisedButton:
            id: new_item_button
            on_release:
                root.go_item()
            text: "Add new"
            size_hint: .3, .1
            font_size: "20px"
            pos_hint: {"center_x":.5, "center_y":.5}
            md_bg_color: .6, .3, .3, 1

        MDLabel:
            size_hint: 1, .05

        MDRaisedButton:
            text: "History"
            on_release:
                root.go_history()
            size_hint: .3, .1
            font_size: "20px"
            pos_hint: {"center_x":.5, "center_y":.4}
            md_bg_color: .6, .5, .3, 1

        MDLabel:
            size_hint: 1, .1


<TableScreen>:
    MDLabel:
        text: "You can click on the checkbox next to the row, the values will be loaded into the boxes below and then you can save"
        font_size: 30
        pos_hint : {"top":1, "center_x":0.5}
        size_hint: 0.7, 0.3

    MDLabel:
        text: "Edit entry"
        font_size: 30
        pos_hint : {"top":0.3, "center_x":0.2}
        size_hint: 0.3, 0.3


<RegisterScreen>:
    FitImage:
        source: 'stopandclean_brooms.jpg'

    MDCard:
        size_hint: .5, .8
        pos_hint: {"center_x":.5, "center_y":.45}
        md_bg_color: .2, .5, .6, .6

    MDBoxLayout:
        size_hint: 0.4, 0.8
        orientation: "vertical"
        pos_hint: {"center_x":.5}

        MDLabel:
            id: signup_label_text
            text: "Register"
            font_size: "30px"
            size_hint: .4, .2

        MDTextField:
            id: reg_name
            hint_text: "name"
            icon_right: "account"
            color_mode: 'custom'
            line_color_focus: 1, 1, 0, 1
            mode: "rectangle"
            size_hint: 1, .18

        MDTextField:
            id: reg_email
            hint_text: "email"
            icon_right: "email"
            color_mode: 'custom'
            line_color_focus: 1, 1, 0, 1
            mode: "rectangle"
            size_hint: 1, .18

        MDTextField:
            id: reg_password
            password: True
            hint_text: "password"
            icon_right: "eye-off"
            color_mode: 'custom'
            line_color_focus: 1, 1, 0, 1
            mode: "rectangle"
            size_hint: 1, .18

        MDLabel:
            size_hint: 1, .03

        MDRaisedButton:
            text: "enter"
            on_release:
                root.try_signup()
            size_hint: .2, .05

        MDLabel:
            size_hint: 1, .1


<NewItemScreen>:
    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .9}

    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .78}

    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .66}

    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .54}

    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .42}

    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .3}

    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .18}

    MDCard:
        size_hint: 1, .2
        md_bg_color: .5, .1, .3, .3
        pos_hint: {'center_y': .06}



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': .91}

        MDLabel:
            id: date_label
            text: "Date"
            size_hint: .1, .5

        MDRaisedButton:
            text: "Calendar"
            pos_hint: {'center_x': .75}
            on_release:
                root.show_date_picker()



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': .79}

        MDLabel:
            text: "Person on duty"
            size_hint: .5, .5

        MDTextField:
            id: person_duty
            hint_text: "Person on duty"
            mode: "rectangle"
            size_hint: .4, .35
            pos_hint: {'center_x': .9}



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': .66}

        MDLabel:
            text: "Duration"
            size_hint: .5, .5

        MDSlider:
            id: duration
            min: 0
            max: 180
            value: 30
            hint: True
            size_hint: .3, .5
            pos_hint: {'center_x': .9}



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': .55}

        MDLabel:
            text: "Task"
            size_hint: .5, .5

        MDTextField:
            id: task
            hint_text: "Task"
            mode: "rectangle"
            size_hint: .4, .35
            pos_hint: {'center_x': .9}



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': .43}

        MDLabel:
            text: "Feedback from teachers"
            size_hint: .5, .5

        MDTextField:
            id: feedback
            hint_text: "Feedback"
            mode: "rectangle"
            size_hint: .4, .35
            pos_hint: {'center_x': .9}



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': .3}

        MDLabel:
            text: "Rating on the stop&clean"
            size_hint: .5, .5

        MDSlider:
            id: rating
            min: 0
            max: 10
            value: 5
            hint: True
            size_hint: .3, .5
            pos_hint: {'center_x': .9}



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': 19}

        MDLabel:
            text: "Time you started cleaning"
            size_hint: .5, .5

        MDTextField:
            id: start_time
            hint_text: "Time you started"
            mode: "rectangle"
            size_hint: .4, .35
            pos_hint: {'center_x': .9}



    MDBoxLayout:
        size_hint: 1, 0.2
        orientation: "horizontal"
        pos_hint: {'center_y': .19}

        MDLabel:
            text: "Tools"
            size_hint: .5, .5

        MDTextField:
            id: tool
            hint_text: "tool"
            mode: "rectangle"
            size_hint: .4, .35
            pos_hint: {'center_x': .9}

    MDRaisedButton:
        id: enter_item
        text: "Save"
        pos_hint: {'center_x': .9, 'center_y': .04}
        on_release:
            root.enter_item()

    MDRaisedButton:
        id: back_main
        text: "Back"
        pos_hint: {'center_x': .7, 'center_y': .04}
        on_release:
            root.back_main()
```
