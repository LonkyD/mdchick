���콫ԭ���Ĳ��͵Ĳ���ȫ����ֲ���� [codecos](codecos.com) ���棬��ʵblogjava.net�����й��ܵģ����ǵ���Ϊxml�������ҿ��������xml��������������Ҫ��ֱ�ӽ���html�鷳Щ�����Ի�����pythonд��һ��ץȡ�Ľű���

�ű��Ĺ����õ��˶��̡߳�urllib2����GET����urllib2��urllib1��cookielib����POST����BeautifulSoup��html���ݽ��з�����

1. ����ԭ���Ĳ��� [http://www.blogjava.net/vagasnail](http://www.blogjava.net/vagasnail)��ץȡ���ݣ��õ�urllib2��

        req = urllib2.Request(self.url)
        response = urllib2.urlopen(req)
        the_page = response.read()

2. ���ڿ�ʼʹ�õ��̣߳��������������ϲ������гɹ���������ʹ���˶��г̵ķ�ʽ��ÿһ��url��ץȡ���������ݺ���Ŀ�격��POST���ݶ�������һ���µ��̡߳�

        ### �����߳����threading.Thread�̳�
        class FetchThread(threading.Thread):
            def run(self):
                # ��������Ǳ���ʵ�ֵģ���ʵ��JAVA�е�Thread������˵���
                # �߳�ʵ�����о����������
                pass

3. �õĽ�������BeautifulSoup������ǳ����ð���

        soup = BeautifulSoup(data)
        #�õ����е�����
        alinks =  soup.find_all('a')
        #������ʽ����Ϊ�������е����Ӷ����õ�
        pattern1 = re.compile(r'^http://www.blogjava.net/vagasnail/articles/[0-9]+\.html$')
        pattern2 = re.compile(r'^http://www.blogjava.net/vagasnail/archive/[0-9]{4}/[0-9]{2}/[0-9]{2}/[0-9]+\.html$')
        pattern3 = re.compile(r'^http://www.blogjava.net/vagasnail/category/[0-9]+\.html$')
        pattern4 = re.compile(r'^http://www.blogjava.net/vagasnail/archive/[0-9]{4}/[0-9]{2}\.html$')
        #�õ����⣬������ID��ʶ��
        title = soup.find_all(id="viewpost1_TitleUrl")
        #ֻҪ�ı����ݾ�����
        art['title'] = title[0].get_text()
        #�õ�����
        content = soup.find_all('div', class_="postbody")
        #����Ӧ������HTML�ַ�
        art['content'] = str(content[0])
        #����������ֱ����Ϊ��ͨ���ı��ڵ�������ģ�û�취ֻ����������
        re.M��ʾ����ģʽ
        pattern_date = re.compile(r'([0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2})',re.M)
        #����Ҫ��findall�����ѯ�ַ�����ƥ��ģʽ���������ַ���
        pdates = pattern_date.findall(str_postfoot) 

4. ����POST��[Ŀ�격��](codecos.com)��

        #����Ҫ�õĿ�
        import urllib,urllib2,cookielib
         
        cj = cookielib.CookieJar()
        #����POST���ݵ�URL
        b_url ='http://codecos.com/****'
        art['tag'] = ",".join(art['tag'])
        #art���б��������ת��Ԫ��
        body = art.items()
        #����cookie
        opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cj))
        #ģ�����������
        opener.addheaders = [('User-agent',
            'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)')]
        urllib2.install_opener(opener)
        #��������
        req = urllib2.Request(b_url, urllib.urlencode(body))
        u = urllib2.urlopen(req)
        u.read()

> ����[�ҵĲ���](codecos.com)���ڵ��Ѿ��кܶ������ˣ�������Щ������ʽ��Щ���⣬ֻ���ֶ������ˡ�
> ������Щ������[markdown](http://wowubuntu.com/markdown/)д�ģ����������鷳�������л���[markdown](http://wowubuntu.com/markdown/)�������
    