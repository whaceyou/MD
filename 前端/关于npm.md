```bash
cnpm : 无法加载文件 C:\Users\hp\AppData\Roaming\npm\cnpm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的  
about_Execution_Policies。
所在位置 行:1 字符: 1
 cnpm install amfe-flexible
+ ~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess

```

解决方法:
1.
以管理员身份运行power shell
2.
输入set-ExecutionPolicy RemoteSigned
然后输入A 回车
问题解决

缺少node-sass

```bash
1.npm install -g cnpm --registry=https://registry.npm.taobao.org

2.cnpm install node-sass --save
```









