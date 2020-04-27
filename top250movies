import requests
from bs4 import BeautifulSoup
import threading
from multiprocessing.pool import ThreadPool
from queue import Queue
import time

qu = Queue(4)
pool = ThreadPool(3)
a = 0


def request(num):
    url = "https://movie.douban.com/top250?start=%d" % num

    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4090.0 Safari/537.36 Edg/83.0.467.0'}
    session = requests.session()
    text = session.get(url, headers=headers).text
    qu.put(text)


def parser():
    global a
    f = open('./scripy/douban.txt', 'w', encoding='utf-8')
    while True:
        try:
            soup = BeautifulSoup(qu.get(timeout=0.5), 'html.parser')
            for li in soup.ol.find_all('li'):
                text = str()
                titles = li.find_all('span', {'class': 'title'})
                for title in titles:
                    text = text + str(title.string)

                bodes = li.p.contents
                a += 1
                movie = '%d: ' % a + text + '\n' + bodes[0] + bodes[2] + '\n'
                f.write(movie)
        except:
            break
    f.close()


def main():
    pool.map_async(request, list(range(0, 250, 25)))
    th = threading.Thread(target=parser)
    th.start()
    print('开始下载：')
    t1 = time.time()
    pool.close()
    pool.join()
    t2 = time.time()
    print('下载完成，耗时：', t2 - t1)


if __name__ == "__main__":

    main()
