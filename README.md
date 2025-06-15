此应用仅供娱乐，请勿商用

V1.1.0

资源 URL必须输入完整链接（含http/https），支持图片、文档、视频等资源。

保存路径	D:\downloads（Windows）	可选，不填则保存到程序当前目录；需确保目录存在或允许程序自动创建。

文件名	必须带后缀（如.jpg .pdf），否则文件格式错误。

```python
import requests
import os
import tkinter as tk
from tkinter import messagebox

def get_info(url):
    try:
        # 发起一个网络请求，获取资源的内容，使用 stream=True 以便逐块下载
        response = requests.get(url, stream=True)
        # 检查状态码
        response.encoding = response.apparent_encoding
        # 返回 response 对象
        return response
    except Exception as e:
        # 打印错误信息
        print(e)
        return None

def start_download():
    # 获取输入框中的内容
    resource_url = url_entry.get()
    save_path = path_entry.get()
    file_name = name_entry.get()

    if not save_path:
        save_path = '.'

    # 拼接完整的文件路径
    full_path = os.path.join(save_path, file_name)

    # 创建保存路径的目录（如果不存在）
    os.makedirs(os.path.dirname(full_path), exist_ok=True)

    # 获取资源信息
    response = get_info(resource_url)
    if response:
        try:
            with open(full_path, 'wb') as file:
                for data in response.iter_content(1024):
                    file.write(data)
            messagebox.showinfo("下载成功", f'资源已成功保存到 {full_path}')
        except Exception:
            messagebox.showerror("下载失败", "下载出现问题，请检查网络连接。")
    else:
        messagebox.showerror("下载失败", '资源爬取失败，请检查 URL 或网络连接。')

# 创建主窗口
root = tk.Tk()
root.title("爬虫(下载大文件时,时间会有点长,请耐心等待,本程序仅供娱乐)")

# 创建标签和输入框
tk.Label(root, text="资源 URL:").grid(row=0, column=0, padx=10, pady=5)
url_entry = tk.Entry(root, width=50)
url_entry.grid(row=0, column=1, padx=10, pady=5)

tk.Label(root, text="保存路径:").grid(row=1, column=0, padx=10, pady=5)
path_entry = tk.Entry(root, width=50)
path_entry.grid(row=1, column=1, padx=10, pady=5)

tk.Label(root, text="文件名(在文件名后加上后缀,如XX.png):").grid(row=2, column=0, padx=10, pady=5)
name_entry = tk.Entry(root, width=50)
name_entry.grid(row=2, column=1, padx=10, pady=5)

# 创建下载按钮
download_button = tk.Button(root, text="开始下载", command=start_download)
download_button.grid(row=3, column=0, columnspan=2, pady=20)

# 运行主循环
root.mainloop()
```