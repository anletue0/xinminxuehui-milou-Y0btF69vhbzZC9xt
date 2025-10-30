# 线性同余方程

### 定义

线性同余方程就是形如 ax≡b(modm)ax\equiv b\pmod max≡b(modm) 其中 a,b,ma,b,ma,b,m 是给定的整数。

### 解法

由同余的性质可知 m∣ax−bm\mid ax-bm∣ax−b 即 ax−b=kmax-b=kmax−b=km 其中 k∈Zk\in \mathbb{Z}k∈Z。

如果我们设 Bézout 定理。

由**Bézout 定理**我们可以得到，这个同余方程有解当且仅当 gcd⁡(a,m)∣b\gcd(a,m)\mid bgcd(a,m)∣b。

我们考虑在有解的情况下使用扩展欧几里得算法先求解出 ax+my=gcd⁡(a,m)ax+my=\gcd(a,m)ax+my=gcd(a,m) 的一组特解 {x=x0y=y0\left\{\begin{matrix}x=x\_0 \\y=y\_0\end{matrix}\right.{x=x0​y=y0​​，然后呢，我们就可以得到 x=x0×bgcd⁡(a,m)x=\frac{x\_0\times b}{\gcd(a,m)}x=gcd(a,m)x0​×b​ 就是原方程的一组解。

#### 关于扩展欧几里得算法的说明

##### 内容

我们考虑不定方程 ax+by=gcd⁡(a,b)ax+by=\gcd(a,b)ax+by=gcd(a,b) 的一组特解，我们可以采用递归的方法来求解，实际上这也就是扩展欧几里得算法：

1. 显然当 b=0b=0b=0 时，有 {x=1y=0\left\{\begin{matrix}x=1\\y=0\end{matrix}\right.{x=1y=0​ 满足条件。
2. 当 b≠0b\ne 0b=0 时，我们根据欧几里得算法有 gcd⁡(a,b)=gcd⁡(b,a mod b)\gcd(a,b)=\gcd(b,a\bmod b)gcd(a,b)=gcd(b,amodb) 于是，我们就有 (a mod b)y+bx=gcd⁡(b,a mod b)(a\bmod b)y+bx=\gcd(b,a\bmod b)(amodb)y+bx=gcd(b,amodb) 又由于 (a mod b)y+bx=(a−b×⌊ab⌋)×y+bx(a\bmod b)y+bx=\left(a-b\times\lfloor\frac{a}{b}\rfloor\right)\times y+bx(amodb)y+bx=(a−b×⌊ba​⌋)×y+bx 将 RHS⁡\operatorname{RHS}RHS 展开，合并同类项后有 (a mod b)y+bx=ay+b×(x−⌊ab⌋y)(a\bmod b)y+bx=ay+b\times \left(x-\lfloor\frac{a}{b}\rfloor y\right)(amodb)y+bx=ay+b×(x−⌊ba​⌋y) 于是，我们令 x0=y,y0=x−⌊ab⌋yx\_0=y,y\_0=x-\lfloor \frac{a}{b}\rfloor yx0​=y,y0​=x−⌊ba​⌋y 就有 ax0+by0=gcd⁡(a,b)ax\_0+by\_0=\gcd(a,b)ax0​+by0​=gcd(a,b)。

##### 代码实现

根据上述内容，我们可以打出扩展欧几里得算法的代码：

```
int exgcd(int a,int b,int&x,int&y){
	if(b==0){
		x=1;
		y=0;
		return a;
	}
	int d=exgcd(b,a%b,x,y);
	int z=x;
	x=y;
	y=z-z*y;
	return d;
}
```

##### 功能介绍

以上函数的返回值为 gcd⁡(a,b)\gcd(a,b)gcd(a,b)，注意到参数 x,yx,yx,y 均在前面加上了取地址符，表示在函数中可以改变 `x` 与 `y` 的值，而函数运行完成后 `x` 与 `y` 所保存的值就是 ax+by=gcd⁡(a,b)ax+by=\gcd(a,b)ax+by=gcd(a,b) 的一组特解。

### 一道模板题

[洛谷 P1082](https://github.com)：

这道题目就是模板题，方程可以写成 ax+by=1ax+by=1ax+by=1 的形式，于是我们使用扩展欧几里得算法，可以求出特解 x0x\_0x0​ 然后 x0 mod bx\_0\bmod bx0​modb 就是原方程的最小正整数解了。

```
#include
#define int long long
using namespace std;
int a,b;
int exgcd(int a,int b,int&x,int&y){
	if(b==0){
		x=1;
		y=0;
		return a;
	}
	int d=exgcd(b,a%b,x,y);
	int z=x;
	x=y;
	y=z-(a/b)*y;
	return d;
}
signed main(){
	cin>>a>>b;
	int x,y;
	exgcd(a,b,x,y);
	cout<<(x%b+b)%b;
	return 0;
}
```

# 线性同余方程组

作者太懒了，这里先讲解更加宽泛的扩展中国剩余定理吧，等以后再讲解特殊的中国剩余定理，顺便宣传一下博客：[link](https://github.com)。

### 问题简述

给定一个 kkk 个方程的线性同余方程组：

{x≡a1(modm1)x≡a2(modm2)⋮x≡ak(modmk)\left\{\begin{matrix}
x\equiv a\_1\pmod {m\_1}\\x\equiv a\_2\pmod {m\_2}
\\\vdots
\\x\equiv a\_k\pmod {m\_k}
\end{matrix}\right.⎩⎨⎧​x≡a1​(modm1​)x≡a2​(modm2​)⋮x≡ak​(modmk​)​

其中 m1,m2,…,mkm\_1,m\_2,\dots,m\_km1​,m2​,…,mk​ 不一定两两互质。

### 解题方法

我们的大致解题思路为将 222 个方程合并为一个新的方程，以此类推，最终我们会得到一个 x≡y(modz)x\equiv y\pmod zx≡y(modz) 的一个方程，易见上面的方程组的最小正整数解就是 yyy。

接下来我们来解决合并方程的问题，我们考虑如下两个方程：

{x≡a1(modm1)x≡a2(modm2)\left\{\begin{matrix}
x\equiv a\_1\pmod {m\_1}\\x\equiv a\_2\pmod {m\_2}
\end{matrix}\right.{x≡a1​(modm1​)x≡a2​(modm2​)​

我们根据第一个式子可以写出 xxx 的通解 x=a1+m1×kx=a\_1+m\_1\times kx=a1​+m1​×k 其中 kkk 为任意整数，我们将这个通解带入第二个式子就可以得到 a1+m1×k≡a2(modm2)a\_1+m\_1\times k\equiv a\_2\pmod {m\_2}a1​+m1​×k≡a2​(modm2​) 我们移一下项就可以得到 m1×k≡a2−a1(modm2)m\_1\times k\equiv a\_2-a\_1\pmod {m\_2}m1​×k≡a2​−a1​(modm2​)，这就是上面的方程组合并后的结果。

而这个方程有解的充要条件是 gcd⁡(m1,m2)∣a2−a1\gcd(m\_1,m\_2)\mid a\_2-a\_1gcd(m1​,m2​)∣a2​−a1​，这个其实就是裴蜀定理，这里不再概述。

我们继续讲，我们得到这个充要条件后我们可以判断这个方程是否有解，如果有解我们就继续进行接下来的操作。

我们设 d=gcd⁡(m1,m2)d=\gcd(m\_1,m\_2)d=gcd(m1​,m2​)，然后将我们合并的方程变换一下就是：

m1×kd≡a2−a1d(modm2d)\frac{m\_1\times k}{d}\equiv \frac{a\_2-a\_1}{d}\pmod {\frac{m\_2}{d}}
dm1​×k​≡da2​−a1​​(moddm2​​)

然后，我们设 m1′=m1d,c=a2−a1d,m2′=m2dm\_1'=\frac{m\_1}{d},c=\frac{a\_2-a\_1}{d},m\_2'=\frac{m\_2}{d}m1′​=dm1​​,c=da2​−a1​​,m2′​=dm2​​ 于是我们就有：

m1′×k≡c(modm2′)m\_1'\times k\equiv c\pmod {m\_2'}
m1′​×k≡c(modm2′​)

注意到此时 m1′,m2′m\_1',m\_2'm1′​,m2′​ 互质，所以 m1′m\_1'm1′​ 在模 m2′m\_2'm2′​ 的意义下存在乘法逆元，我们可以使用扩展欧几里得算法来求出逆元，即求出整数 invinvinv 使得 m1′×inv≡1(modm2′)m\_1'\times inv\equiv 1\pmod {m\_2'}m1′​×inv≡1(modm2′​)，所以我们继续将这个方程变换就变成了：

k≡c×inv(modm2′)k\equiv c\times inv\pmod {m\_2'}
k≡c×inv(modm2′​)

如果我们记 k0=c×invk\_0=c\times invk0​=c×inv 则 kkk 的通解为 k0+m2′×tk\_0+m\_2'\times tk0​+m2′​×t 其中 ttt 为任意整数。

然后我们将这个 kkk 带回一开始的式子就可以得出：

x=a1+m1×(k0+m2′×t)=(a1+m1×k0)+(m1×m2′)×t=(a1+m1×k0)+m1×m2d×t=(a1+m1×k0)+lcm(m1,m2)×t\begin{aligned}
x&=a\_1+m\_1\times(k\_0+m\_2'\times t)\\
&=(a\_1+m\_1\times k\_0)+(m\_1\times m\_2')\times t\\
&=(a\_1+m\_1\times k\_0)+\frac{m\_1\times m\_2}{d}\times t\\
&=(a\_1+m\_1\times k\_0)+\mathrm{lcm}(m\_1,m\_2)\times t\end{aligned}x​=a1​+m1​×(k0​+m2′​×t)=(a1​+m1​×k0​)+(m1​×m2′​)×t=(a1​+m1​×k0​)+dm1​×m2​​×t=(a1​+m1​×k0​)+lcm(m1​,m2​)×t​

我们设 x0=a1+m1×k0,L=lcm(m1,m2)x\_0=a\_1+m\_1\times k\_0,L=\mathrm{lcm}(m\_1,m\_2)x0​=a1​+m1​×k0​,L=lcm(m1​,m2​) 所以我们就愉快地得出了：

{x≡a1(modm1)x≡a2(modm2)⟺x≡x0(modL)\left\{\begin{matrix}
x\equiv a\_1\pmod {m\_1}\\x\equiv a\_2\pmod {m\_2}
\end{matrix}\right.\Longleftrightarrow x\equiv x\_0\pmod L{x≡a1​(modm1​)x≡a2​(modm2​)​⟺x≡x0​(modL)

于是，我们完成了合并方程的使命！

最后其实就是一个递推的过程我们一次合并前 222 个方程，最后就能得到答案！

### 代码实现

```
#include
#define LL __int128
#define R register
using namespace std;
namespace fastIO{char *p1,*p2,buf[100000];
	#define nc() (p1==p2&&(p2=(p1=buf)+fread(buf,1,100000,stdin),p1==p2)?EOF:*p1++)
	inline void read(LL&n){LL x=0,f=1;char ch=nc();while(ch<48||ch>57){if(ch=='-'){f=-1;}ch=nc();}while(ch>=48&&ch<=57){x=(x<<3)+(x<<1)+(ch^48),ch=nc();}n=x*f;}inline void read(string&s){s="";char ch=nc();while(ch==' '||ch=='\n'){ch=nc();}while(ch!=' '&&ch!='\n'){s+=ch,ch=nc();}}inline void read(char&ch){ch=nc();while(ch==' '||ch=='\n'){ch=nc();}}inline void write(LL x){if(x<0){putchar('-'),x=-x;}if(x>9){write(x/10);}putchar(x%10+'0');return;}inline void write(const string&s){for(R LL i=0;i<(int)s.size();i++){putchar(s[i]);}}inline void write(const char&c){putchar(c);}
}using namespace fastIO;
inline LL mul(LL a,LL b,const LL&mod){
    a=(a%mod+mod)%mod; 
    b=(b%mod+mod)%mod;
    LL res=0;
    while(b){
        if(b&1)res=(res+a)%mod;
        a=(a+a)%mod;
        b>>=1;
    }
    return res;
}
void exgcd(LL a,LL b,LL&x,LL&y){
    if(b==0){
        x=1;
        y=0;
    }
	else{
        exgcd(b,a%b,y,x);
        y-=x*(a/b);
    }
}
LL inv_mod(LL a,LL m){
    LL x,y;
    exgcd(a,m,x,y);
    return (x%m+m)%m;
}
LL gcd(LL a,LL b){
    return b?gcd(b,a%b):a;
}
LL n,a[100005],b[100005];
signed main(){
    read(n);
    for(int i=0;i<n;i++){
    	read(a[i]);
    	read(b[i]);
    }
    LL a0=a[0];
    LL b0=(b[0]%a0+a0)%a0;
    for(int i=1;i<n;i++){
        LL ai=a[i];
        LL bi=(b[i]%ai+ai)%ai;
        LL d=gcd(a0,ai);
        LL dif=bi-b0;
        LL a0_=a0/d;
        LL ai_=ai/d;
        LL dif_=dif/d;
        LL c=(dif_%ai_+ai_)%ai_;
        LL inv=inv_mod(a0_,ai_);
        LL t0=mul(inv,c,ai_);
        LL a0__=(a0/d)*ai;
        LL mod__=a0__;
        LL p=mul(a0,t0,mod__);
        LL b0__=(b0+p)%mod__;
        a0=mod__;
        b0=b0__;
    }
	write(b0);
    return 0;
}
```

# 一些例题

如果有不会的可以回复作者！

* [洛谷 P5655](https://github.com):[樱花宇宙机场](https://enhouse.org)。
* [洛谷 P2480](https://github.com)。
