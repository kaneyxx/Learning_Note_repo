# Python PyQt5 學習筆記
Credit : [Super Coders](https://youtu.be/28dR-KM5ZH8) from YouTube  
學習歷程跟紀錄全部是看Super Coders影片的, 建議配合使用效果比較好  
* 單純為了自己學習歷程做紀錄, 並非教學文章有錯誤之處請見諒  
## 安裝
在終端機執行(windows就是命令提示字元或power shell)  
```pip install PyQt5```

## 1. 建立第一個GUI視窗
> 重點：  
> * import的模組
> * 基本widget的建構，會一直用到
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QWidget

# 建構第一個視窗

app = QApplication(sys.argv) # app變數將用sys來控制我們程式離開

qwidget = QWidget() # 建構我們的視窗
qwidget.setWindowTitle("First GUI window") # 設定視窗標題
qwidget.show() # 讓視窗顯現出來

sys.exit(app.exec_()) #sys.exit()當我們關閉程式時可以幫助我們離開
```
## 2. 加入第一個按鍵(button)
> 重點：
> * 從PyQt5.QtWidgets多import一個**QPushButton**(之後也都只寫重點)  
> * 按鍵建構以及功能函數建置與連結
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton

#定義按鍵動作用的函式
def buttonClick():
    print("Button Clicked")

app = QApplication(sys.argv)
qwidget = QWidget()
qwidget.setWindowTitle("My first GUI button")

#建構按鍵
button = QPushButton("Fist Button", qwidget) #("按鍵名稱", 放置的widget)
#setToolTip是鼠標移動到按鍵上顯示的提示(不用按)
button.setToolTip("This will display message when I take mouse on button")
#移動按鍵(可以先不加執行看看位置)
button.move(100,100)  #視窗左上=(0, 0)  向右為+x, 向下為+y
#將按鍵與函式作結合
button.clicked.connect(buttonClick) #connect(函數名稱不用括號)

qwidget.show()

sys.exit(app.exec_())
```
## 3. 建構對話窗(dialog box)
> 重點：
> * 從PyQt5.QtWidgets 多import **QMessageBox**
> * 建構時的變數，以及其中的Yes/No大小寫要注意
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QMessageBox

app = QApplication(sys.argv)

qwidget = QWidget()
qwidget.setWindowTitle("First_DialogBox")

#(放置的widget, 標題, 問題/敘述, 選項, 預設值)
button_reply_from_user = QMessageBox.question(qwidget,"PyQt5 Diaglog", "Do you like this program?", QMessageBox.Yes | QMessageBox.No, QMessageBox.No)
# 辨識使用者的選擇
if button_reply_from_user == QMessageBox.Yes:
    print("Thanks!")
else:
    print("Don't worry, we will be better!")

#如果不想顯示視窗可以註解掉<或是放到if/else中按鍵後產生
qwidget.show()

sys.exit(app.exec_())
```
## 4. 建構文本框(input box)
> 重點：
> * 從PyQt5.QtWidgets 多 import **QLineEdit**
> * resize調整尺寸
```python=1
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication, QWidget, QPushButton, QLineEdit

app = QApplication(sys.argv)

#建構一個函式給click用來抓取inputbox的內容
def getTextValue():
    print("You type: " + inputbox.text())
    #獲取後要移除內容
    inputbox.setText("")

# 建構主視窗
mainwindow = QMainWindow()
mainwindow.resize(300, 100)
mainwindow.setWindowTitle("Python Window with Input Text")

# 建構文本框
inputbox = QLineEdit(mainwindow)
# 重新調整大小, 並移動位置
inputbox.resize(200, 20) #(寬, 高)
inputbox.move(10, 20)

# 建構按鈕並移動位置
button = QPushButton("Click This", mainwindow)
button.move(60, 60)
button.clicked.connect(getTextValue)

mainwindow.show()

sys.exit(app.exec_())
```
## 5. 建構目錄條(menu bar)
> 重點：
> * 從PyQt5.QtWidgets多import QAction
> * 子母目錄選項要分清楚
> * 建構選項功能連結後要記得addAction
```python=1
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication, QAction

# 建構給下面目錄選項用的函式
def EditClick():
    print("Edit clicked!")

def OpenClick():
    print("Open clicked")

app = QApplication(sys.argv)

mainwindow = QMainWindow()
mainwindow.resize(300, 200)

# 建構目錄條
mainMenu = mainwindow.menuBar()
# 在目錄條上加入主選項
fileMenu = mainMenu.addMenu("File")
#加入子選項
openMenu = fileMenu.addMenu("Open")
# 加入"可點擊"的項目
openAction = QAction("Open File")
openAction.triggered.connect(OpenClick)
openMenu.addAction(openAction)

editMenu = mainMenu.addMenu("Edit")
# 為選項連結功能(方式類似按鍵), 記得連結後還要addAction
editAction = QAction("Edit")
editAction.triggered.connect(EditClick)
editMenu.addAction(editAction)

mainwindow.show()
sys.exit(app.exec_())
```
## 6. 建構表格(table)
> 重點：
> * import **QTableWidget, QTableWidgetItem, QVBoxLayout**
> * 設置column, row, 欄目
> * 放入表格中的物件/數值
> * 佈局layout
```python=1
import sys
from PyQt5.QtWidgets import QWidget, QApplication, QTableWidget, QTableWidgetItem, QVBoxLayout

# 此函式為點擊表格中物件會列印品項及位置
def getSelectedItemData():
    for currentItem in tableWidget.selectedItems():
        print("Row: " + str(currentItem.row()) + " Column: " + str(currentItem.column()) + " Item:" + currentItem.text())

app = QApplication(sys.argv)

qwidget = QWidget()
qwidget.setWindowTitle("First Table")
qwidget.resize(300, 200)

# 建構垂直布局
layout = QVBoxLayout()

# 建構表格
tableWidget = QTableWidget()
tableWidget.setRowCount(4)  # 設定row數 (從0算起)
tableWidget.setColumnCount(3)  # 設定column數 (從0算起)
# 設定欄目
tableWidget.setHorizontalHeaderItem(0, QTableWidgetItem("Name"))
tableWidget.setHorizontalHeaderItem(1, QTableWidgetItem("Age"))
tableWidget.setHorizontalHeaderItem(2, QTableWidgetItem("Job"))
# 表格內放入物件 (row, column, 物件)
tableWidget.setItem(0, 0, QTableWidgetItem("Peter"))
tableWidget.setItem(0, 1, QTableWidgetItem("18"))
tableWidget.setItem(0, 2, QTableWidgetItem("student"))

tableWidget.setItem(1, 0, QTableWidgetItem("Allen"))
tableWidget.setItem(1, 1, QTableWidgetItem("34"))
tableWidget.setItem(1, 2, QTableWidgetItem("worker"))

tableWidget.setItem(2, 0, QTableWidgetItem("John"))
tableWidget.setItem(2, 1, QTableWidgetItem("23"))
tableWidget.setItem(2, 2, QTableWidgetItem("thief"))

tableWidget.setItem(3, 0, QTableWidgetItem("Hilton"))
tableWidget.setItem(3, 1, QTableWidgetItem("27"))
tableWidget.setItem(3, 2, QTableWidgetItem("lawyer"))

# 將函式作連結
tableWidget.doubleClicked.connect(getSelectedItemData)
# 把表格layout出來
layout.addWidget(tableWidget)
qwidget.setLayout(layout)

qwidget.show()
sys.exit(app.exec_())
```
## 7. 建構標籤(tab)
> 重點:
> * 從PyQt5.QtWidgets import **QTabWidget**
> * 各層layout的設置關係(不懂的話後面有附圖)
```python=1
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication, QLineEdit, QPushButton, QTabWidget, QVBoxLayout, QWidget

app = QApplication(sys.argv)
mainwindow = QMainWindow()
mainwindow.setWindowTitle("Tab Test")
mainwindow.resize(300, 500)

qwidget = QWidget()
layout = QVBoxLayout()

# 創建tabs
tabs = QTabWidget()
tabs.resize(200,300)
# 創建tab items
tab1 = QWidget()
tab2 = QWidget()
# 把tab items加到tab上
tabs.addTab(tab1, "Tab 1")
tabs.addTab(tab2, "Tab 2")

tablayout1 = QVBoxLayout()
editBox1 = QLineEdit()
button1 = QPushButton("Tab 1 Item")
tablayout1.addWidget(editBox1)
tablayout1.addWidget(button1)
tab1.setLayout(tablayout1)

tablayout2 = QVBoxLayout()
editBox2 = QLineEdit()
button2 = QPushButton("Tab 2 Item")
tablayout2.addWidget(editBox2)
tablayout2.addWidget(button2)
tab2.setLayout(tablayout2)

# Add tab to main layout(first one)
layout.addWidget(tabs)
# set this layout to QWidget
qwidget.setLayout(layout)

# set QWidget to main window
mainwindow.setCentralWidget(qwidget)

mainwindow.show()
sys.exit(app.exec_())
```
<img src="https://i.imgur.com/F7g1oNJ.jpg" width="500" height="400">  

## 8. 水平/垂直 布局 (Horizontal/Vertical Layout)
> 重點：
> * from PyQt5.QtWidgets import **QVBoxLayout, QHBoxLayout**
> * 布局的先後關係
```python=1
import sys
from PyQt5.QtWidgets import QWidget, QMainWindow, QVBoxLayout, QHBoxLayout, QLineEdit, QPushButton, QApplication

app = QApplication(sys.argv)

mainWindow = QMainWindow()
mainWindow.setWindowTitle("H/V Layout")
qwidget = QWidget()

'''
講一下要做的目標, 3個文本框 + 3個按鈕, 文本框在上 按鍵在下
所以最終會是文本框和按鍵各自的水平布局, 
而文本框及按鍵這倆群體則呈現垂直布局
'''
# 建構三個文本框
input_1 = QLineEdit()
input_2 = QLineEdit()
input_3 = QLineEdit()

# 建構三個按鈕
button1 = QPushButton("Button 1")
button2 = QPushButton("Button 2")
button3 = QPushButton("Button 3")

# 分別將文本框及按鍵放入水平布局
horizontal_layout_1 = QHBoxLayout()
horizontal_layout_1.addWidget(input_1)
horizontal_layout_1.addWidget(input_2)
horizontal_layout_1.addWidget(input_3)

horizontal_layout_2 = QHBoxLayout()
horizontal_layout_2.addWidget(button1)
horizontal_layout_2.addWidget(button2)
horizontal_layout_2.addWidget(button3)

# 建構倆群體要放的垂直布局, 並放上去
vertical_layout = QVBoxLayout(qwidget) #不要qwidget也可以因為最終下方還是得setLayout
vertical_layout.addLayout(horizontal_layout_1)
vertical_layout.addLayout(horizontal_layout_2)

qwidget.setLayout(vertical_layout) #qwidget設定上主layout
mainWindow.setCentralWidget(qwidget) #主視窗放上qwidget
mainWindow.show()

sys.exit(app.exec_())
```
## 9. 方形布局 (Grid Layout)
> 重點：
> * from PyQt5.QtWidgets import **QGridLayout**
> * 物件間的空白去除
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QLineEdit, QPushButton, QGridLayout, QWidget, QVBoxLayout, QHBoxLayout

app = QApplication(sys.argv)

qwidget = QWidget()
mainWindow = QMainWindow()
mainWindow.setWindowTitle("Calculator")

# 建構方形布局, 並以座標形式加上物件
grid_layout = QGridLayout()
# grid_layout.addWidget(QLineEdit(), 0, 0)
grid_layout.addWidget(QPushButton("1"), 1, 0)
grid_layout.addWidget(QPushButton("2"), 1, 1)
grid_layout.addWidget(QPushButton("3"), 1, 2)
grid_layout.addWidget(QPushButton("4"), 2, 0)
grid_layout.addWidget(QPushButton("5"), 2, 1)
grid_layout.addWidget(QPushButton("6"), 2, 2)
grid_layout.addWidget(QPushButton("7"), 3, 0)
grid_layout.addWidget(QPushButton("8"), 3, 1)
grid_layout.addWidget(QPushButton("9"), 3, 2)
# 將佈局中物件之間的空白去除
grid_layout.setVerticalSpacing(0)
grid_layout.setHorizontalSpacing(0)

# 建構一個水平布局來放文本框, 因為如果放在方形布局內會直接限制住它的大小
horizontal_layout = QHBoxLayout()
editLine = QLineEdit()
horizontal_layout.addWidget(editLine)

# 再用垂直布局放這倆布局群體
vertical_layout = QVBoxLayout(qwidget)
vertical_layout.addLayout(horizontal_layout)
vertical_layout.addLayout(grid_layout)

qwidget.setLayout(vertical_layout)
mainWindow.setCentralWidget(qwidget)
mainWindow.show()

sys.exit(app.exec_())
```
## 10. Number/Text/Choice Dialog (各類對話框)
> 重點：
> * from PyQt5.QtWidgets import QInputDialog
> * 各種Dialog參數的設置
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QWidget, QPushButton, QLineEdit, QVBoxLayout, QInputDialog

def showNumberDialog():
    # (widget, 視窗標題, 標籤名稱, 預設值, 最小值, 最大值, 步伐)
    data = QInputDialog.getInt(qwidget, "Number Dialog", "Number", 1, 1, 10, 1)
    # 回傳一個tuple(數值, True/False), 當點擊ok會是True, cancel則是False
    if data[1]:
        print("Pressed OK: Value is "+ str(data[0]))
    else:
        print("Pressed Cancel: Value is " + str(data[0]))

def showTextDialog():
    # (widget, 視窗標題, 標籤名稱, QLineEdit.Normal, 預設值)
    data = QInputDialog.getText(qwidget, "Text Dialog", "Text Label", QLineEdit.Normal, "")
    if data[1]:
        print("Pressed OK: Text is "+ str(data[0]))
    else:
        print("Pressed Cancel: Text is " + str(data[0]))

def showChoiceDialog():
    # 用tuple建構一些選項
    items = ("Red", "Blue", "Green")
    # (widget, 視窗標題, 標籤名稱, tuple items, 預設值索引)
    data = QInputDialog.getItem(qwidget, "Choice Dialog", "Colors", items, 0)
    if data[1]:
        print("Pressed OK: Choice is " + str(data[0]))
    else:
        print("Pressed Cancel")


app = QApplication(sys.argv)

mainWindow = QMainWindow()
mainWindow.resize(400, 500)
mainWindow.setWindowTitle("All GUI Element")

qwidget = QWidget()

vertical_layout = QVBoxLayout()
# 建構input number dialog button
button_number_dialog = QPushButton("Show Number Dialog")
button_number_dialog.clicked.connect(showNumberDialog)
vertical_layout.addWidget(button_number_dialog)
# 再建構個文本的
button_text_dialog = QPushButton("Show Text Dialog")
button_text_dialog.clicked.connect(showTextDialog)
vertical_layout.addWidget(button_text_dialog)
# 再建構個選單的
button_choice_dialog = QPushButton("Show Choice Dialog")
button_choice_dialog.clicked.connect(showChoiceDialog)
vertical_layout.addWidget(button_choice_dialog)

qwidget.setLayout(vertical_layout)

mainWindow.setCentralWidget(qwidget)
mainWindow.show()

sys.exit(app.exec_())
```
## 11. 只接受數字/字母的文本框(Validator for int/str), 下拉式選單(Combobox)
> 重點:
> * from PyQt5.QtWidgets import QLineEdit, QLabel, QCombobox
> * from PyQt5.QtGui import QIntValidator, QRegExpValidator
> * from PyQt5.QtCore import QRegExp
> * 繼承上面的程式, 因此只列出新增的程式碼部分, import部分從上面看
```python=
def showItem(text):
    print("Your choice is " + text)
# 建構只接受數字輸入的文本框number input box

numberLabel = QLabel("Number Input")
# setGeometry跟resize一樣能調整尺寸, 但後面還能放座標
numberLabel.setGeometry(200, 20, 10, 10) 
inputNumber = QLineEdit()
int_validator = QIntValidator()
inputNumber.setValidator(int_validator)
vertical_layout.addWidget(numberLabel)
vertical_layout.addWidget(inputNumber)

# 建構只接受文字(a-z)輸入的文本框 character input box
stringLabel = QLabel("ABC Input")
stringLabel.setGeometry(200, 20, 10, 10)
vertical_layout.addWidget(stringLabel)
letterInput = QLineEdit()
reg = QRegExp("[A-Za-z_]+") # + 表示可以打多個字
str_validator = QRegExpValidator(reg) # setup what you accept
letterInput.setValidator(str_validator)
vertical_layout.addWidget(letterInput)

# 建構下拉式選單
dropDown = QComboBox()
drop_down_items = ["Red", "Green", "Blue", "Yellow"]
dropDown.addItems(drop_down_items) # 也能用additem一個一個加
dropDown.activated[str].connect(showItem) #顯示選擇, []內放變數為int或str
vertical_layout.addWidget(dropDown)
```
## 12. 選項按鈕(Radiobutton)/核取方塊(Check box)/圖片
> 重點:
> * 從PyQt5.QtWidgets import QRadioButton, QHBoxLayout, QGroupBox, QCheckBox
> * 從PyQt5.QtGui import QPixmap
> * 一樣只列出多的程式碼
```python
def radioCheck(radiobutton):
    if radiobutton.isChecked():
        print("Checked: " + str(radiobutton.color))
    else:
        print("Unchecked: " + str(radiobutton.color))

def radioCheck2(radiobutton):
    if radiobutton.isChecked():
        print("Checked: " + str(radiobutton.country))
    else:
        print("Unchecked: " + str(radiobutton.country))

def getCheckValue(checkbox):
    if checkbox.isChecked():
        print("Checked: " + str(checkbox.text()))
    else:
        print("Unchecked: " + str(checkbox.text()))

# 建構選項按鈕(radiobutton),要注意的地方是raidobutton選項是一個一個的群組, 
# 不同群組的選擇不會互相影響, 為了達到這個目的我們用groupbox
radiobutton1 = QRadioButton("Red")
radiobutton1.color = "Red" # 加入按鈕內的"資料"
radiobutton1.toggled.connect(lambda: radioCheck(radiobutton1))
radiobutton2 = QRadioButton("Blue")
radiobutton2.color = "Blue"
radiobutton2.toggled.connect(lambda: radioCheck(radiobutton2))
radiobutton3 = QRadioButton("Green")
radiobutton3.color = "Greem"
radiobutton3.toggled.connect(lambda: radioCheck(radiobutton3))
# 做成水平布局並放上去
horizontal_layout1 = QHBoxLayout()
horizontal_layout1.addWidget(radiobutton1)
horizontal_layout1.addWidget(radiobutton2)
horizontal_layout1.addWidget(radiobutton3)
# 建構一個groupbox來放radiobutton
groupbox_radio1 = QGroupBox()
groupbox_radio1.setLayout(horizontal_layout1) # 放上去
vertical_layout.addWidget(groupbox_radio1) #再放到主要的垂直佈局上

# 建構第二個radio button group
radiobutton4 = QRadioButton("Taiwan")
radiobutton4.country = "Taiwan" # 加入按鈕內的"資料"
radiobutton4.toggled.connect(lambda: radioCheck2(radiobutton4))
radiobutton5 = QRadioButton("Japan")
radiobutton5.country = "Japan"
radiobutton5.toggled.connect(lambda: radioCheck2(radiobutton5))
radiobutton6 = QRadioButton("America")
radiobutton6.country = "America"
radiobutton6.toggled.connect(lambda: radioCheck2(radiobutton6))
# 做成水平布局並放上去
horizontal_layout2 = QHBoxLayout()
horizontal_layout2.addWidget(radiobutton4)
horizontal_layout2.addWidget(radiobutton5)
horizontal_layout2.addWidget(radiobutton6)
# 建構一個groupbox來放radiobutton
groupbox_radio2 = QGroupBox()
groupbox_radio2.setLayout(horizontal_layout2) # 放上去
vertical_layout.addWidget(groupbox_radio2)

# 建構核取方塊(check box)
checkbox1 = QCheckBox("Python")
checkbox1.stateChanged.connect(lambda: getCheckValue(checkbox1))
checkbox2 = QCheckBox("JAVA")
checkbox2.stateChanged.connect(lambda: getCheckValue(checkbox2))
checkbox3 = QCheckBox("PHP")
checkbox3.stateChanged.connect(lambda: getCheckValue(checkbox3))
horizontal_layout3 = QHBoxLayout()
horizontal_layout3.addWidget(checkbox1)
horizontal_layout3.addWidget(checkbox2)
horizontal_layout3.addWidget(checkbox3)
vertical_layout.addLayout(horizontal_layout3)

# 建構個放圖片的物件
image_label = QLabel()
img_px = QPixmap("pythonicon.png")
image_label.setPixmap(img_px)
vertical_layout.addWidget(image_label)
```
## 13. WebView
> 重點:
> * 先安裝 `pip install PyQtWebKit`
> * from PyQt5.QtWebKitWidgets import *
> * from PyQt5.QtCore import QUrl
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QWidget
from PyQt5.QtWebKitWidgets import *
from PyQt5.QtCore import QUrl

app = QApplication(sys.argv)
mainWindow = QMainWindow()
mainWindow.resize(1800, 900)
qwidget = QWidget()

# 建構Webview物件
web = QWebView()
web.load(QUrl("https://twitch.tv/")) # 將網址傳入

vertical_layout = QVBoxLayout()
vertical_layout.addWidget(web)

qwidget.setLayout(vertical_layout)
mainWindow.setCentralWidget(qwidget)
mainWindow.show()
sys.exit(app.exec_())
```
## 14. 建構進度條(Progress Bar)
> 重點:
> * from PyQt5.QtWidgets import QProgressBar
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QMainWindow, QProgressBar, QPushButton, QVBoxLayout, QHBoxLayout

def increaseValue():
    progress_bar.setValue(progress_bar.value() + 10)

def decreaseValue():
    progress_bar.setValue(progress_bar.value() - 10)

app = QApplication(sys.argv)
mainWindow = QMainWindow()
mainWindow.setWindowTitle("Progress Bar")
mainWindow.resize(300, 400)
widget = QWidget()


vertical_layout = QVBoxLayout()
horizontal_layout = QHBoxLayout()
# 建構調整進度的按鈕
increase_button = QPushButton("Increase")
increase_button.clicked.connect(increaseValue)
decrease_button = QPushButton("Decrease")
decrease_button.clicked.connect(decreaseValue)
horizontal_layout.addWidget(increase_button)
horizontal_layout.addWidget(decrease_button)
# 建構進度條
progress_bar = QProgressBar()
progress_bar.resize(30, 400)
progress_bar.setValue(0)

vertical_layout.addWidget(progress_bar)
vertical_layout.addLayout(horizontal_layout)

widget.setLayout(vertical_layout)
mainWindow.setCentralWidget(widget)
mainWindow.show()
sys.exit(app.exec_())
```
## 15. 開啟檔案/儲存檔案(Save/Open File Dialog)
> 重點:
> * from PyQt5.QtWidgets import QFileDialog
> * get(Save/Open)FileName()的參數設定
> * 原生與非原生的視窗介面
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QWidget, QPushButton, QHBoxLayout, QFileDialog

def saveDialog():
    option = QFileDialog.Options()
    # (widget, 視窗標題, 預設檔案名稱, 檔案類型, 選項)
    # option |= QFileDialog.DontUseNativeDialog # 用來override 原生的(native) save dialog
    file = QFileDialog.getSaveFileName(widget, "Save File Window", "default.txt", "All files (*)", options = option)
    print(file[0])

def openFileDialog():
    option = QFileDialog.Options()
    # (widget, 視窗標題, 預設檔案名稱, 檔案類型, 選項)
    # option |= QFileDialog.DontUseNativeDialog # 用來override 原生的(native) save dialog
    file = QFileDialog.getOpenFileName(widget, "Open File Window", "Default File", "All files (*)", options = option)
    print(file[0])

def openMultiFiles():
    option = QFileDialog.Options()
    # (widget, 視窗標題, 預設檔案名稱, 檔案類型, 選項)
    # option |= QFileDialog.DontUseNativeDialog # 用來override 原生的(native) save dialog
    file = QFileDialog.getOpenFileNames(widget, "Select Multi Files", "Default", "All files (*)", options = option)
    print(file[0])

app = QApplication(sys.argv)

mainWindow = QMainWindow()
widget = QWidget()

button_save = QPushButton("Save Dialog")
button_save.clicked.connect(saveDialog)
button_single_file = QPushButton("Open file")
button_single_file.clicked.connect(openFileDialog)
button_multi_files = QPushButton("Select files")
button_multi_files.clicked.connect(openMultiFiles)

horizontal_layout = QHBoxLayout()
horizontal_layout.addWidget(button_save)
horizontal_layout.addWidget(button_single_file)
horizontal_layout.addWidget(button_multi_files)

widget.setLayout(horizontal_layout)
mainWindow.setCentralWidget(widget)
mainWindow.show()
sys.exit(app.exec_())
```
## 16. 計算器製作(Calculator)
> 重點:
> * 幾乎沒有新的import, decimal是為了不顯示小數運算後的誤差才放入的
> * Python中強大的eval()使用
```python=1
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QWidget, QPushButton, QLineEdit, QGridLayout
from decimal import Decimal # 為了調整顯示計算

def clickEvent(value):
    new_value = num_input.text() + "" + value
    num_input.setText(new_value)

def calculate():
    data = num_input.text()
    cal = eval(data) # 用python超強的eval()自行判斷並計算
    dec = Decimal(cal)
    cal = round(dec, 2) # 四捨五入到小數後第二位
    num_input.setText(str(cal)) # 要顯示所以記得改回str

def backspaceclick():
    data = num_input.text()
    new_data = data[:-1]
    num_input.setText(new_data)

app = QApplication(sys.argv)
mainWindow = QMainWindow()
mainWindow.setWindowTitle("Calculator")
widget = QWidget()

# 方形布局
grid_layout = QGridLayout()
# 建構按鈕
one_b = QPushButton("1")
two_b = QPushButton("2")
three_b = QPushButton("3")
four_b = QPushButton("4")
five_b = QPushButton("5")
six_b = QPushButton("6")
seven_b = QPushButton("7")
eight_b = QPushButton("8")
nine_b = QPushButton("9")
zero_b = QPushButton("0")
plus_b = QPushButton("+")
sub_b = QPushButton("-")
mul_b = QPushButton("*")
divide_b = QPushButton("/")
dots_b = QPushButton(".")
equal_b = QPushButton("=")
back_b = QPushButton("<")
#
num_input = QLineEdit()
# (item, row index, column index, 初始size, 終末size)
grid_layout.addWidget(num_input, 0, 0, 1, 3) # 終末3格因為要塞一個倒退
grid_layout.addWidget(back_b, 0, 3)
grid_layout.addWidget(seven_b, 1, 0)
grid_layout.addWidget(eight_b, 1, 1)
grid_layout.addWidget(nine_b, 1, 2)
grid_layout.addWidget(four_b, 2, 0)
grid_layout.addWidget(five_b, 2, 1)
grid_layout.addWidget(six_b, 2, 2)
grid_layout.addWidget(one_b, 3, 0)
grid_layout.addWidget(two_b, 3, 1)
grid_layout.addWidget(three_b, 3, 2)
grid_layout.addWidget(zero_b, 4, 0)
grid_layout.addWidget(dots_b, 4, 1)
grid_layout.addWidget(equal_b, 4, 2)

grid_layout.addWidget(divide_b, 1, 3)
grid_layout.addWidget(mul_b, 2, 3)
grid_layout.addWidget(sub_b, 3, 3)
grid_layout.addWidget(plus_b, 4, 3)
# 布局中物件的空白取消
grid_layout.setSpacing(0)
# grid_layout.setVerticalSpacing(0)
# grid_layout.setHorizontalSpacing(0)

# 把按鍵們連結上功能
one_b.clicked.connect(lambda: clickEvent("1"))
two_b.clicked.connect(lambda: clickEvent("2"))
three_b.clicked.connect(lambda: clickEvent("3"))
four_b.clicked.connect(lambda: clickEvent("4"))
five_b.clicked.connect(lambda: clickEvent("5"))
six_b.clicked.connect(lambda: clickEvent("6"))
seven_b.clicked.connect(lambda: clickEvent("7"))
eight_b.clicked.connect(lambda: clickEvent("8"))
nine_b.clicked.connect(lambda: clickEvent("9"))
zero_b.clicked.connect(lambda: clickEvent("0"))
plus_b.clicked.connect(lambda: clickEvent("+"))
sub_b.clicked.connect(lambda: clickEvent("-"))
mul_b.clicked.connect(lambda: clickEvent("*"))
divide_b.clicked.connect(lambda: clickEvent("/"))
dots_b.clicked.connect(lambda: clickEvent("."))
back_b.clicked.connect(backspaceclick)
equal_b.clicked.connect(calculate)

# 布局放上去
widget.setLayout(grid_layout)
mainWindow.setCentralWidget(widget)
mainWindow.show()

sys.exit(app.exec_())
```
## 17. 簡易記事本(NotePad)製作
> 重點:  
> * import sys, os, json  
> * from PyQt5.QtGui import QFont (我自己加的, 原影片沒有)  
> * 除了config建置及字體字型設置外, 其他都是之前學過的內容  
```python=1
import sys
import os
import json
from PyQt5.QtWidgets import QApplication, QMainWindow, QWidget, QVBoxLayout, QTextEdit, QAction, QFileDialog, QInputDialog
from PyQt5.QtGui import QFont

def openFile():
    options = QFileDialog.Options()
    data = QFileDialog.getOpenFileName(widget, "Select File", "", "All files(*)", options = options)
    # 讀取文檔並放入編輯區
    file_content = open(data[0], "r")
    textboard.setPlainText(file_content.read())
    file_content.close()

def saveFile():
    options = QFileDialog.Options()
    data = QFileDialog.getSaveFileName(widget, "Save File", "", options = options)
    file_content = open(data[0], "w")
    file_content.write(textboard.toPlainText())
    file_content.close()

# 建構config檔
def initNotepad():
    if os.path.exists("./config.json"):
        data = open("./config.json", "r")
        # 將json檔轉成字典
        dictdata = json.loads(data.read())
        data.close()
        return dictdata
    else:
        data = open("./config.json", "w")
        dictdata = {"size":15}
        data.write(json.dumps(dictdata))
        data.close()
        return dictdata

def fontAction_func():
    data = QInputDialog.getInt(widget, "Font Size", "Font Size", initData["size"], 12, 48, 1)
    # 使用者按OK確認後, 直接修改游標處或是選取的字
    # "備註掉的部分"如果要修改config並改動, 由於會存在config中, 所以下次開啟會是最後的字體大小
    if data[1]:
        # initData["size"] = data[0]
        # file_handle = open("./config.json", "w")
        # file_handle.write(json.dumps(initData))
        # file_handle.close()
        # # 這段設定為設定字體大小改變時, 整個notepad的字體大小全部都會跟著變動
        # textdata = textboard.toPlainText()
        # textboard.setPlainText("")
        textboard.setFontPointSize(data[0])
        # textboard.setPlainText(textdata)

app = QApplication(sys.argv)
mainWindow = QMainWindow()
mainWindow.setWindowTitle("NotePad by PyQt5")
widget = QWidget()

# 建構目錄條
menubar = mainWindow.menuBar()
# 建構"File"
filemenu = menubar.addMenu("File")
# 加入要點擊的項目
openaction = QAction("Open")
openaction.triggered.connect(openFile)
filemenu.addAction(openaction)

saveaction = QAction("Save")
saveaction.triggered.connect(saveFile)
filemenu.addAction(saveaction)

# 建構setting
setting_menu = menubar.addMenu("Setting")
# 加入要點擊的項目
font_action = QAction("Font Size")
font_action.triggered.connect(fontAction_func)
setting_menu.addAction(font_action)

v_layout = QVBoxLayout()
v_layout.setContentsMargins(0, 0, 0, 0) # 設定布局物件與邊界的距離

# 建構文字編輯區
textboard = QTextEdit()
v_layout.addWidget(textboard)  # 放到垂直布局上

initData = initNotepad() # 讀取我們設定的config檔案

# textboard.setFontPointSize(15) # 設定初始字體大小
textboard.setFont(QFont("Hack", 20)) # 設定初始字型

widget.setLayout(v_layout)
mainWindow.setCentralWidget(widget)
mainWindow.show()
mainWindow.showMaximized() # 開啟即是全螢幕

sys.exit(app.exec_())
```