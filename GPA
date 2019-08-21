import re
from tkinter import *
from tkinter import messagebox
from selenium import webdriver

options = webdriver.ChromeOptions()
options.add_argument('--headless')
browser = webdriver.Chrome(options=options)

root = Tk()
root.title('四川大学绩点查询--By. June')
root.geometry('450x260')

Label(root, text="学号：", width=5, font=('楷体', 14)).grid(row=0, column=0, padx=80, pady=40)
Label(root, text="密码：", width=5, font=('楷体', 14)).grid(row=1, column=0, padx=20, pady=10)

v1 = StringVar()
v2 = StringVar()

e1 = Entry(root, textvariable=v1)
e2 = Entry(root, textvariable=v2, show="*")
e1.grid(row=0, column=1, sticky=W)
e2.grid(row=1, column=1, sticky=W)

num = 0

def get_point():
	global num
	e1 = Entry(root, textvariable=v1)
	e2 = Entry(root, textvariable=v2, show="*")
	e1.grid(row=0, column=1, sticky=W)
	e2.grid(row=1, column=1, sticky=W)
	username = e1.get()
	password = e2.get()

	if num == 0:
		url = 'http://ehall.scu.edu.cn/gsapp/sys/xscjfxapp/*default/index.do?#/grcjfx'
		browser.get(url)

	else:
		url = 'http://ehall.scu.edu.cn/gsapp/sys/xscjfxapp/*default/index.do?#/grcjfx'
		browser.get(url)
		browser.delete_all_cookies()
		browser.refresh()

	browser.find_element_by_xpath('//*[@id="username"]').send_keys(username)
	browser.find_element_by_xpath('//*[@id="password"]').send_keys(password)
	browser.find_element_by_xpath('//*[@id="loginBtn123"]').click()

	r = browser.get('http://ehall.scu.edu.cn/gsapp/sys/xscjfxapp/modules/grcjfx/queryXscjtjxx.do')
	content = browser.page_source
	pattern = re.compile(r'<html.*?"XH":"(.*?)".*?"XM":"(.*?)".*?"PJFCYBNJZYBFB":([1-9]\d*\.\d*|0\.\d*[1-9]\d*$).*?"NJDM_DISPLAY":"(.*?)"'
						+ r'.*?"PJXFJDCYBNJZYBFB":([1-9]\d*\.\d*|0\.\d*[1-9]\d*$).*?"ZYDM_DISPLAY":"(.*?)"'
						+ r'.*?"PJF":([1-9]\d*\.\d*|0\.\d*[1-9]\d*$).*?"YXDM_DISPLAY":"(.*?)".*?"PJJD":([1-9]\d*\.\d*|0\.\d*[1-9]\d*$).*?</html>', re.S)
	results = re.findall(pattern, content)

	for i in results:
		result = [i[1], i[0], i[3], i[7], i[5], i[6], i[2], i[8], i[4]]

	return result
	num += 1

def callback():
	global num
	result = get_point()

	content = '姓名：{0}\n学号：{1}\n年级：{2}\n学院：{3}\n专业：{4}\n平均分：{5}分\nTop：{6}%\n平均绩点：{7}\nTop：{8}%'\
				.format(result[0], result[1], result[2], result[3], result[4], result[5], 100-float(result[6]), result[7], 100-float(result[8]))

	messagebox.showinfo("信息查询", content)
	num += 1

Button(root, text="登录", font=('楷体', 14), width=10, command=callback)\
             .grid(row=3, column=0, sticky=W, padx=60, pady=50)
Button(root, text="退出", font=('楷体', 14), width=10, command=root.quit)\
             .grid(row=3, column=1, sticky=E, padx=60, pady=50)

mainloop()
