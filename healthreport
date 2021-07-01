import requests
from lxml import etree
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
import time
import smtplib
from email.mime.text import MIMEText
from email.header import Header
from PIL import Image
import pytesseract
url = 'https://cas.gzhu.edu.cn/cas_server/login?service=http%3A%2F%2Fyqtb.gzhu.edu.cn%2Finfoplus%2Flogin%3FretUrl%3Dhttp%253A%252F%252Fyqtb.gzhu.edu.cn%252Finfoplus%252Foauth2%252Fauthorize%253Fx_redirected%253Dtrue%2526scope%253Dprofile%252Bprofile_edit%252Bapp%252Btask%252Bprocess%252Bsubmit%252Bprocess_edit%252Btriple%252Bstats%252Bsys_profile%252Bsys_enterprise%252Bsys_triple%252Bsys_stats%252Bsys_entrust%252Bsys_entrust_edit%2526response_type%253Dcode%2526redirect_uri%253Dhttp%25253A%25252F%25252Fyq.gzhu.edu.cn%25252Ftaskcenter%25252Fwall%25252Fendpoint%25253FretUrl%25253Dhttp%2525253A%2525252F%2525252Fyq.gzhu.edu.cn%2525252Ftaskcenter%2525252Fworkflow%2525252Findex%2526client_id%253D1640e2e4-f213-11e3-815d-fa163e9215bb'
headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.128 Safari/537.36 Edg/89.0.774.77'
}
reportsuccess1 = False
def report(username,password):
    global reportsuccess1
    webdriver_path = r'E:\Projects\scrapy\chromedriver.exe'
    options = Options()
    options.add_argument('--headless')
    browser = webdriver.Chrome(executable_path=webdriver_path, options=options)
    browser.get(url)
    time.sleep(1)
    browser.find_element_by_id('username').send_keys(username)
    browser.find_element_by_id('password').send_keys(password)
    img = browser.find_element_by_tag_name('img').screenshot_as_png
    time.sleep(1)
    with open('./验证码1.png', 'wb') as fp:
        fp.write(img)
    image = Image.open('./验证码1.png')
#    print(result)
    captcha = pytesseract.image_to_string(image, config='digits')
    print(captcha)
    time.sleep(2)
    try:
        browser.find_element_by_id('captcha').send_keys(captcha)
        current_window1 = browser.current_window_handle
    #    browser.find_element_by_name('submit').click()
        time.sleep(3)
        browser.find_element_by_class_name('widget-elli').click()
        time.sleep(2)
        current_window2 = browser.window_handles
        browser.switch_to.window(current_window2[1])
        time.sleep(5)
        browser.find_element_by_xpath('/html/body/div[4]/form/div/div/div/div/div[1]/div[4]/span/a').click()
        current_window3 = browser.current_window_handle
        time.sleep(5)
        browser.find_element_by_xpath('/html/body/div[4]/form/div/div[2]/div[3]/div/div[1]/div[1]/table/tbody/tr[2]/td/div/table/tbody/tr[71]/td[2]/div/div/input[1]').click()
        time.sleep(3)
        #新加2
        browser.find_element_by_xpath('/html/body/div[4]/form/div/div[2]/div[3]/div/div[1]/div[1]/table/tbody/tr[2]/td/div/table/tbody/tr[81]/td[2]/div/div/input[1]').click()
        time.sleep(1)
        browser.find_element_by_xpath('/html/body/div[4]/form/div/div[2]/div[3]/div/div[1]/div[1]/table/tbody/tr[2]/td/div/table/tbody/tr[82]/td[2]/div/div/input').send_keys('2021-06-24')
        time.sleep(1)
        browser.find_element_by_xpath('/html/body/div[4]/form/div/div[2]/div[3]/div/div[1]/div[1]/table/tbody/tr[2]/td/div/table/tbody/tr[83]/td[1]/div/input').click()
        time.sleep(2)
        browser.find_element_by_xpath('/html/body/div[4]/form/div/div[1]/div[2]/ul/li[1]/a/nobr').click()
        time.sleep(3)
    except Exception as es:
        browser.quit()
        reportsuccess1 = False
        return reportsuccess1
    else:
        browser.quit()
        reportsuccess1 = True
        return reportsuccess1
    #    print('打卡成功')
        time.sleep(5)

#发送邮件到个人邮箱
#创建邮件参数
def send_mail(from_qq_mail, to_qq_mail):
    host_server = 'smtp.qq.com'                 #邮件服务器，这里相当于邮箱账号
    pwd = pw                    #配置邮件客户端的授权码，这里相当于邮箱密码
    from_qq_mail = from_qq_mail           #邮箱个发件人
    to_qq_mail = to_qq_mail             #邮箱收件人

    #创建邮件格式
    msg = MIMEText('打卡成功！', 'plain', 'utf-8')                      #创建一个邮件格式
    msg['Subject'] = Header('每日健康打卡', 'utf-8')           #邮件的主题
    msg['From'] = from_qq_mail
    msg['To'] = from_qq_mail

    #发送邮件
    smtp = smtplib.SMTP_SSL(host_server)         #链接服务器
    smtp.login(from_qq_mail, pwd)                #登录邮箱
    smtp.sendmail(from_qq_mail, to_qq_mail, msg.as_string())      #发送邮件
    smtp.quit()

if __name__ == "__main__":
    reportsuccess = False
    while reportsuccess == False or reportsuccess == None:
    #这里填写用户名和密码
        reportsuccess = report(username, password)
        time.sleep(5)
    print(reportsuccess)
    print('打卡成功')
    #打卡成功发送到邮箱，这里填邮箱号
    send_mail(from_qq_mail, to_qq_mail)
   
