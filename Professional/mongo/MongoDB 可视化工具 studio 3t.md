## MongoDB 可视化工具 studio 3t 

#### 更改使用时间：

```javascript
@echo off
ECHO 重置Studio 3T的使用日期......
FOR /f "tokens=1,2,* " %%i IN ('reg query "HKEY_CURRENT_USER\Software\JavaSoft\Prefs\3t\mongochef\enterprise" ^| find /V "installation" ^| find /V "HKEY"') DO ECHO yes | reg add "HKEY_CURRENT_USER\Software\JavaSoft\Prefs\3t\mongochef\enterprise" /v %%i /t REG_SZ /d ""
ECHO 重置完成, 按任意键退出......
pause>nul
exit
```

#### 运行即可破解试用

另外可以加到开机启动运行，这样就不用那么麻烦了。
 详细设置方式:将脚本放到以下位置就可以了

```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
```