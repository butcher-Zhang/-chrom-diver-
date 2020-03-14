# -chrom-diver-
在python 中利用谷歌浏览器和 自动驱使软件 实现自动创建网页并爬取
准备：
1、jupyter 或者是 pycharm
2、谷歌浏览器
3、chrome diver （自动化软件）
4、selenium库 可使用pip 从豆瓣安装
实现方法：
定义四大模块
1、def csv_writer(item):#写入函数
    with open('新浪.csv','a',newline='')as f:
        writer = csv.writer(f)#writer调用了cvs里面的写入函数
        try:
            writer.writerow(item)
        except Exception as e:
            print('保存错误:',e)
        print('正在保存。。。'）
首先是 写入模块 为什么先定义呢 为后面将text类型的微博文章存入定义的微博.csv 写入查错模型
2、def login():#自动登陆函数
    driver.get('https://www.weibo.com')#使用Chorme浏览器的driver 建立虚拟浏览器访问
    time.sleep(random.randint(5,8))#访问后等待页面的载入
    username = driver.find_element_by_id('loginname')#找到输入用户名的一栏
    username.send_keys('13225221072')#通过sand.keys把用户名输入
    password = driver.find_element_by_name('password')#找到输入密码的一栏
    password.send_keys('xxxxxx')#同理输入密码
    submit = driver.find_element_by_xpath('//*[@id="pl_login_form"]/div/div[3]/div[6]/a')#找到登陆按钮
    submit.click()#点击登陆按钮
    time.sleep(15)
自动登陆模块：
这里是在chrome 打开weibo.com后 使用 首先找到我们需要操作的各大模块在web上的位置（使用xpath确定具体位置） 其次利用send。keys 将用户个人信息传入
最后点击登陆。
可能存在的问题： 要求输入验证码 这个呢我学了一部分但是对于微博而言效果不好 因为它的验证码 加入了很多干扰图像识别的因素 后面等我学习了深度学习 用图像识别给大家做
建议呢 先输入一次验证码 结束进程 重新开一次 就不用输入了
3、def spider():#定义爬虫函数
    driver.get('https://www.weibo.com')
    time.sleep(random.randint(2,5))
    all_weibo = driver.find_elements_by_xpath('//*[@class="WB_text W_f14"]')#找到微博文章的共同路径
    for weibo in all_weibo:
        text_write(weibo.text)#通过函数写入内容
    这个就不做过多解释了 可以参照 微博的代码自己分析
4、def text_write(item):#去重并且将内容写入文档
    md5 = hashlib.md5()#初始化一个哈希表
    md5.update(item.encode('utf-8'))#将爬取到的一条微博进行编码
    weibo_md5 = md5.hexdigest()#将编码与微博对应
    if weibo_md5 not in only_weibo:#如果不在已经爬取到的列表中则写入
        with open('微博.text','a',encoding='utf-8')as f:
            f.write(item +'\n')
        only_weibo.update(weibo_md5)#将得到的新的一篇微博的编码放入列表当中
 这里就是之前提及到的text存入文档 利用哈希表 进行去重 具体注释后面已经写了 就不再赘述了
 最后就是使用我们的代码
 if __name__=="__main__":
    driver = webdriver.Chrome('d:/slm/chromedriver.exe')
    login()
    only_weibo = set()
    while True:
        spider()
        time.sleep(random.randint(60,80))
  来操作chromedriver 使用的就是之前定义的四大模块

