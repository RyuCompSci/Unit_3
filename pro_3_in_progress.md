```.py
from kivy.lang import Builder
from kivy.metrics import dp

from kivymd.app import MDApp
from kivymd.uix.screen import MDScreen
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


class stop_and_clean_database(Base):
    __tablename__ = "user_item"
    stop_clean_id = Column(Integer, primary_key=True)
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
        result = session.query(User_database).filter(User_database.email == email_entered, self.check_password(
            password_entered, User_database.password) == True).first()
        print(result)
        if result:
            self.parent.current = "MainScreen"
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
    pass


class TableScreen(MDScreen):
    pass


m = stop_clean()
m.run()
```
