# Troubleshoot

** groundingdino-py 安装失败 **

错误信息如下：

```Plain Text
Defaulting to user installation because normal site-packages is not writeable
Looking in indexes: https://mirror.nju.edu.cn/pypi/web/simple
Collecting groundingdino-py==0.4.0
  Downloading https://mirror.nju.edu.cn/pypi/web/packages/da/26/af35089119098ecd9d2e174b9cfbeb0c1246d65449c718b4e57328a08c1c/groundingdino-py-0.4.0.tar.gz (82 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 82.3/82.3 kB 4.8 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... error
  error: subprocess-exited-with-error

  × python setup.py egg_info did not run successfully.
  │ exit code: 1
  ╰─> [7 lines of output]
      Traceback (most recent call last):
        File "<string>", line 2, in <module>
        File "<pip-setuptools-caller>", line 34, in <module>
        File "C:\Users\LiveR\AppData\Local\Temp\pip-install-x2mhlg21\groundingdino-py_d95a0eee1ce8466981beb059ff6a26bf\setup.py", line 41, in <module>
          readme = readme_file.read()
                   ^^^^^^^^^^^^^^^^^^
      UnicodeDecodeError: 'gbk' codec can't decode byte 0xa4 in position 2878: illegal multibyte sequence
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.
```

如果是 Windows, 可能会碰到这个问题，解决办法是把系统区域设置为 `en-us`,
参考 GroundingDINO 的 [Issue](https://github.com/IDEA-Research/GroundingDINO/issues/206)

或者也可以手动下载安装包，然后修改其中 `setup.py` 第 41 为 `with open('readme.md', 'r+', encoding='utf-8') as readme:`, 然后重新安装。
