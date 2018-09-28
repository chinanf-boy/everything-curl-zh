
## API兼容性

libcurl保证API稳定,并保证您今天编写的程序将继续工作.我们不会破坏兼容性.

随着时间的推移,我们向API添加特性、新选项和新函数,但不会以不兼容的方式更改行为或删除函数.

上次我们以不兼容的方式更改API是2006的7.16.0,我们计划再也不做了.

### 版本号

cURL和libcurl是单独版本,但它们大多是相互紧接着的.

版本编号总是使用相同的系统建立的:

```
X.Y.Z
```

-   X是主版本号
-   Y是释放号
-   Z是斑块数

### 颠簸数

这些X.Y.Z数字中的一个将在每一个新版本中出现颠簸.一个颠簸的数字右边的数字将被重置为零.

主版本号X在*真的?*世界发生了巨大的冲突.当执行改变或添加事物/特征时,释放号Y被颠簸.当变化仅仅是修正时,补丁号Z被颠簸.

这意味着,在版本1.2.3之后,如果进行了非常大的改变,我们可以发布2.0.0;如果没有进行那么大的更改,可以发布1.3.0;如果大多数bug都已修复,则可以发布1.2.4.

颠簸,就像用1增加数字一样,无条件地只影响其中一个数字(右边的数字被设置为零).1变成2, 3变成4, 9变成10, 88变成89,99变成100.因此,1.2.9之后是1.2.10.3.93.3后,可能会出现3.100.0.

所有原始的curl源发布归档文件都是根据libcurl版本(而不是根据前面提到的curl客户端版本)命名的.

### 哪一种曲解版

作为对任何可能希望支持新的libcurl特性,同时仍然能够使用旧版本构建的应用程序的服务,所有版本都具有存储在`curl/curlver.h`使用可用于比较的静态编号方案来进行文件.版本号定义为:

```
#define LIBCURL_VERSION_NUM 0xXXYYZZ
```

其中XX、YY和ZZ是主版本,十六进制中的版本号和补丁号.所有三个数字字段总是用两个数字表示(每个八位).1.2.0将显示为"0x010200",而版本91.7显示为"0x090B07".

在最近发布的版本中,这个6位十六进制数总是更大的数字.它与大于或小于的工作进行比较.

这个数字也可作为三个独立的定义:`LIBCURL_VERSION_MAJOR`,`LIBCURL_VERSION_MINOR`和`LIBCURL_VERSION_PATCH`.

当然,这些定义只适用于确定版本号.*刚才*他们不会帮助你找出三年后在运行时使用的LyCURL版本.

### 哪一个libcurl版本运行

找出应用程序使用的LIcCURL版本*马上*,`curl_version_info()`有你吗?

应用程序应该使用这个函数来判断是否可以这样做,而不是使用编译时检查,因为动态/DLL库可以独立于应用程序而改变.

curl_version_info()返回指向一个结构的指针,其中包含版本号和各种特性以及libcurl的运行版本的信息.你通过给它一个特殊的年龄计数器来称呼它,这样libcurl就知道它的"年龄".年龄是一个定义`CURLVERSION_NOW`是在cURL发育过程中以不规则的间隔增加的计数器.年龄号告诉LbCURL它可以返回什么样的结构.

你这样调用函数:

CurLyVulnOnInFixDATA\*VER=CurLyVeluoSnIn信息(CurrVeluon现在);

然后,数据将指向具有至少可以具有以下布局的结构:

```
struct {
  CURLversion age;          /* see description below */

  /* when 'age' is 0 or higher, the members below also exist: */
  const char *version;      /* human readable string */
  unsigned int version_num; /* numeric representation */
  const char *host;         /* human readable string */
  int features;             /* bitmask, see below */
  char *ssl_version;        /* human readable string */
  long ssl_version_num;     /* not used, always zero */
  const char *libz_version; /* human readable string */
  const char * const *protocols; /* protocols */

  /* when 'age' is 1 or higher, the members below also exist: */
  const char *ares;         /* human readable string */
  int ares_num;             /* number */

  /* when 'age' is 2 or higher, the member below also exists: */
  const char *libidn;       /* human readable string */

  /* when 'age' is 3 or higher (7.16.1 or later), the members below also
     exist  */
  int iconv_ver_num;       /* '_libiconv_version' if iconv support enabled */

  const char *libssh_version; /* human readable string */

} curl_version_info_data;
```
