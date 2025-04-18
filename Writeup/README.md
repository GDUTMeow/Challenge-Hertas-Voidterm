# Herta's Voidterm 1

拿到终端，使用`help`查看命令，发现可以查看环境变量

输入 `env` 后回车就能看到 flag 了

`flag{m4d4m-H3Rta-1s-A_P33RLEsS_gem}` = 黑塔女士举世无双

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/msedge_hV4KgAEGb8.png)

# Herta's Voidterm 2

查看 `/etc/hosts` 可以发现有一条记录

`100.100.100.100 th3-h3rt4.bili33.top`

但是实际上用浏览器访问看不到，提示`DNS_PROBE_FINISHED_NXDOMAIN`，但是 DNS 记录也可以拿来放字符串（TXT类型记录）

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/msedge_2TD0uNgW6H.png)

Windows 下使用 `nslookup -q=txt th3-h3rt4.bili33.top`，Linux 下使用 `dig th3-h3rt4.bili33.top txt` 都可以拿到结果

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/WindowsTerminal_znkakPBdWZ.png)

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/MobaXterm_Fc2E9UoSjp.png)

`flag{MAd@M_HerT@_I5-AN-uNr1v4Lled_gen1US_=w=}` = 黑塔女士聪明绝顶

# Herta's Voidterm 3

在 `/home/herta/Pictures` 目录下发现 `Igiari.png`，结合题目说明 `本站的 cat 命令是经过修改的，如果 cat 到了非纯文本文件会触发下载`，很可能这个文件有用

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/msedge_7yYUpR3Lwx.png)

先用 `cat` 命令下载下来，得到文件，后缀为 `.png`，使用 tweakpng 查看

第一个提示：CRC错误

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/tweakpng_wpulRD0nQ2.png)

第二个提示：文件尾后有垃圾

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/tweakpng_uyKzWlZndT.png)

所以得到两个常考方向：IHDR修改高度隐藏信息、文件尾藏有信息

对于IHDR修改高度，直接随便找个脚本跑就行

> [AabyssZG/Deformed-Image-Restorer: 自动爆破PNG图片宽高并一键修复工具](https://github.com/AabyssZG/Deformed-Image-Restorer)

直接 `python Deformed-Image-Restorer.py -i Igiari.png`，修复完成后可以看到底下有内容：`WE_neD_m0r-HERT@` = 我们需要更多黑塔

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/unhex.png)

文件尾的内容可以直接用 010 Editor 看，也可以直接 strings 提取 `strings .\Igiari.png`

得到信息 `d1D-y0U_Get_iT??` = 开窍了吗（大黑塔普通攻击）

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/WindowsTerminal_ZN2kxcztFl.png)

至此得到两个内容 `WE_neD_m0r-HERT@` 和 `d1D-y0U_Get_iT??`

别忘了刚开始你做第一题的时候，有个 `SECRET=d4303addef085aa30b5b7383f2fbe0e3acce3c222df0b86fa6385f90bcefb4fe4510e66ab4ee45cb02788490a9959381`

`SECRET` 肯定不可能平白无故地给你，而且还特别命名为 `SECRET`

两个提取到的内容长度都是 16，如果都当做加密参数的话，很容易想到 AES（使用 key 和向量 iv 进行加密）

因为不知道哪个是密钥哪个是向量，所以两个都试试

得到当 `d1D-y0U_Get_iT??` 为密钥，`WE_neD_m0r-HERT@` 为向量的时候可以成功解密

得到 flag 为 `flag{M4D@M-H3rTa-IS_@N-iniM1TAbl3-bEAUty-0rz}` = 黑塔女士沉鱼落雁

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/msedge_cuofZY19Bo.png)

![](https://cdn.jsdelivr.net/gh/GDUTMeow/Hertas-Voidterm/Pictures/msedge_ZqumHBLEGT.png)
