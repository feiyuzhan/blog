
### 需求
mac上运行3个 command line 的 python脚本，脚本py1和py2是server，脚本py3依赖前两个脚本server的运行。在运行py3并生成report之后关闭3个脚本

----


### 一开始的shell脚本方式
思路：将py1和py2写入command，执行后删除该command

最直接的方法，但是不够优雅，出现异常可能导致需要手动删除生成的文件，需要手动关闭py1和py2的terminal，优点是出现异常也容易在terminal内正常关闭server

```bash
#!/bin/bash
# 判断 python_server1.command 是否存在，如存在则删除该文件

touch python_server1.command
echo "python3 -m http.server 10000"  >> python_server1.command
chmod +x python_server1.command
open python_server1.command
sleep(1)
rm python_server1.command

# do same to py2
```

---

### python os.fork 的方式

利用os.fork API实现，需要安装psutil，在关闭的时候要注意异常的处理，否则可能导致server未正常关闭的情况

```python
import os
import psutil
import signal
import subprocess
import time


def kill_child_processes(parent_pid, sig=signal.SIGINT):
    try:
        parent = psutil.Process(parent_pid)
    except psutil.NoSuchProcess:
        return

    children = parent.children(recursive=True)  
    # 这里需要递归查询，因为main进程中使用了subprocess，实际为
    #  main
    #     |---- pid_py1
    #     |           |----- subprocess server1
    #     |---- pid_py2
    #     |           |----- subprocess server2
    #     |
    #     |---- subprocess RunXX
        
    for process in children:
        process.send_signal(sig)


if __name__ == '__main__':
    pid_py1 = os.fork()
    if pid_py1 == 0:
        try:
            subprocess.run('python3 -m http.server 10000', shell=True)
            print('server1 is running ...')
        except KeyboardInterrupt:
            print('Stop python server1 on port 10000')
    else:
        pid_py2 = os.fork()
        if pid_py2 == 0:
            try:
                subprocess.run('python3 server2', shell=True)
                print('server2 is running ...')
            except KeyboardInterrupt:
                print('Stop server2 ')
        else:
            time.sleep(5)
            subprocess.run('python3 RunXX.py', shell=True)
            print('waiting for 15s...')
            time.sleep(15)
            subprocess.run('python3 GenerateReport.py', shell=True)
            os.popen('open -a "Google Chrome" "./report.html"')
            try:
                while True:
                    print('Hold on, press Ctrl+C to exit...')
                    time.sleep(5)
            except (KeyboardInterrupt, psutil.NoSuchProcess):
                print('Exit...')
                kill_child_processes(os.getpid())
```