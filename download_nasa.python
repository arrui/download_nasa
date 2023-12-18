import os
import requests
from bs4 import BeautifulSoup

# 创建存储图片的文件夹
if not os.path.exists('nasa'):
    os.makedirs('nasa')

# 获取网页内容
url = 'https://apod.nasa.gov/apod/archivepix.html'
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

# 找到所有的图片链接
for link in soup.find_all('a'):
    href = link.get('href')
    if href and 'ap' in href:
        # 获取包含图片的网页
        image_page_url = 'https://apod.nasa.gov/apod/' + href
        try:
            image_page_response = requests.get(image_page_url)
            image_page_soup = BeautifulSoup(image_page_response.text, 'html.parser')
        except Exception as e:
            print(f'获取网页失败，错误信息：{e}')
            continue

        # 从网页中获取图片链接
        image_tag = image_page_soup.find('img')
        if image_tag is None:
            print(f'在网页{image_page_url}中没有找到图片')
            continue
        image_url = image_tag.get('src')
        if not image_url.startswith('http'):
            image_url = 'https://apod.nasa.gov/apod/' + image_url

        # 检查图片是否已经被下载过
        filename = os.path.join('nasa', image_url.split('/')[-1])
        if os.path.exists(filename):
            print(f'图片已经存在，跳过下载：{filename}')
            continue

        # 下载图片
        try:
            image_response = requests.get(image_url)
            image_content = image_response.content
        except Exception as e:
            print(f'下载图片失败，错误信息：{e}')
            continue

        # 保存图片到文件
        try:
            with open(filename, 'wb') as f:
                f.write(image_content)
            print(f'图片保存成功：{filename}')
        except Exception as e:
            print(f'保存图片失败，错误信息：{e}')
