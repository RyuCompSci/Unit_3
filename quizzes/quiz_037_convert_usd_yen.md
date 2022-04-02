### convert usd into jpy, jpy into usd

#### Python code

```.py
from kivymd.app import MDApp
from kivymd.uix.screen import MDScreen
from kivymd.uix.label import MDLabel


class Converter(MDApp):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)

    def build(self):
        return

    def pm_btn(self):
        """convert yen into dollar by deviding the value by 122"""
        if self.root.ids.text_field_1.text == '':
            self.root.ids.text_field_1.text = str((int(self.root.ids.text_field_2.text)) // 122)
        """convert dollar into yen by multipying the value by 122"""
        elif self.root.ids.text_field_2.text == '':
            self.root.ids.text_field_2.text = str((int(self.root.ids.text_field_1.text)) * 122)


m = Converter()
m.run()
```

#### Kivy code

```.py
Screen:
    size: 500, 300

    MDBoxLayout:
        orientation: "vertical"
        size_hint: 1, 1

        MDLabel:
            size_hint: .3, 1

        MDBoxLayout:
            orientation: "horizontal"
            size_hint: 1, 1

            MDLabel:
                size_hint: .015, 1

            MDTextField:
                id: text_field_1
                size_hint: .3, 1
                hint_text: "USD"
                helper_text_mode: "on_focus"
                mode: "rectangle"

            MDLabel:
                size_hint: .015, 1

            MDRaisedButton:
                id: convert_button
                size_hint: .3, 1
                text: "Convert"
                font_size: "30px"
                #RGB code
                md_bg_color: .91, .84, .67, 1
                on_release:
                    app.pm_btn()

            MDLabel:
                size_hint: .015, 1

            MDTextField:
                id: text_field_2
                size_hint: .3, 1
                hint_text: "YEN"
                helper_text_mode: "on_focus"
                mode: "rectangle"

            MDLabel:
                size_hint: .015, 1

        MDLabel:
            size_hint: .3, 1
```

![]()
