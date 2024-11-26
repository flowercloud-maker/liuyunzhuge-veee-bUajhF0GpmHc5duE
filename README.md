[合集 \- 算法与数据结构(1\)](https://github.com)1\.算法与数据结构 1 \- 模拟11\-25收起
### 模拟


#### 介绍


正如名称所说，模拟是信息学学生最早接触，也是难度跨度最大的知识点。简单如[《A\+B 问题》](https://github.com "《A+B 问题》")[《校门外的树》](https://github.com "《校门外的树》")开门见山，没有任何铺垫和掩饰；困难如[《猪国杀》](https://github.com "《猪国杀》")[《乱西星上的空战》](https://github.com "《乱西星上的空战》"):[蓝猫机场](https://fenfang.org)同样开门见山，但谁做谁头疼。
因此，本文选择了模拟作为《算法与数据结构》的第一章。


#### 引入


正如名字所表示的，模拟的核心思想就是“复制粘贴”：根据题目提供的信息，将该题目的解决过程模拟出来就能够得到最终的结果。下面以[《\[CSP\-S 2023] 密码锁》](https://github.com "《[CSP-S 2023] 密码锁》")为例进行介绍：



> 小 Y 有一把五个拨圈的密码锁。如图所示，每个拨圈上是从 0 到 9 的数字。每个拨圈都是从 0 到 9 的循环，即 9 拨动一个位置后可以变成 0 或 8，
> ![](https://cdn.luogu.com.cn/upload/image_hosting/aku4duog.png)
> 因为校园里比较安全，小 Y 采用的锁车方式是：从正确密码开始，随机转动密码锁仅一次；每次都是以某个幅度仅转动一个拨圈或者同时转动两个相邻的拨圈。
> 当小 Y 选择同时转动两个相邻拨圈时，两个拨圈转动的幅度相同，即小 Y 可以将密码锁从 00115 转成 11115，但不会转成 12115。
> 时间久了，小 Y 也担心这么锁车的安全性，所以小 Y 记下了自己锁车后密码锁的 n 个状态，注意这 n 个状态都不是正确密码。
> 为了检验这么锁车的安全性，小 Y 有多少种可能的正确密码，使得每个正确密码都能够按照他所采用的锁车方式产生锁车后密码锁的全部 n 个状态。


不难发现题目中的密码锁只有 5 位数，所以可以直接对每一个可能的密码 x（x∈Z 且 00000≤x≤99999）进行判断。判断时分别检查 n 个状态和可能密码不同的位。若只有 1 位不同则显然可行；若有相邻 2 位不同，但两位与状态的差值固定（即拨动了相同的幅度）也是可行的。然后直接统计即可。



```


|  | #include |
| --- | --- |
|  | using namespace std; |
|  | int n; |
|  | int a[10][10]; |
|  | int ans = 0; |
|  | int b[10]; |
|  | bool cmp(int x) { |
|  | int cnt = 0, pos; |
|  | for (int i = 1; i <= 5; i++) { |
|  | if (b[i] != a[x][i]) { |
|  | cnt++; |
|  | pos = i; |
|  | } |
|  | if (cnt > 2) return true; // 不同的位太多 |
|  | } |
|  | if (cnt == 0) return true; // 没动 |
|  | if (cnt == 1) return false; // 只拨动了一位 |
|  | if (b[pos - 1] == a[x][pos - 1]) return true; // 拨动的两位不相邻 |
|  | // 计算两位的幅度 |
|  | int f = (b[pos] + 10 - a[x][pos]) % 10; |
|  | int g = (b[pos - 1] + 10 - a[x][pos - 1]) % 10; |
|  | return f != g; |
|  | } |
|  | bool test() { |
|  | for (int i = 1; i <= n; i++) { |
|  | if (cmp(i)) { |
|  | return false; |
|  | } |
|  | } |
|  | return true; |
|  | } |
|  | int main() { |
|  | scanf("%d", &n); |
|  | for (int i = 1; i <= n; i++) { |
|  | for (int j = 1; j <= 5; j++) { |
|  | scanf("%d", &a[i][j]); |
|  | } |
|  | } |
|  | for (int i = 0; i <= 9; i++) { |
|  | b[1] = i; |
|  | for (int j = 0; j <= 9; j++) { |
|  | b[2] = j; |
|  | for (int k = 0; k <= 9; k++) { |
|  | b[3] = k; |
|  | for (int l = 0; l <= 9; l++) { |
|  | b[4] = l; |
|  | for (int m = 0; m <= 9; m++) { |
|  | b[5] = m; |
|  | if (test()) { |
|  | ans++; // 直接统计 |
|  | } |
|  | } |
|  | } |
|  | } |
|  | } |
|  | } |
|  | printf("%d\n", ans); |
|  | return 0; |
|  | } |


```

如你所见，代码简单直白，不带任何掩饰。


#### 进阶


难度升级，但也没升多少。只要大胆开写，小心调试，一切皆有可能！
下面有请：[《德州扑克牌》（Texas Hold'em）](https://github.com "《德州扑克牌》（Texas Hold'em）")！



> 一副扑克牌除去大小王，你和对手分别有 2 张已知手牌，桌面上现在有 3 张已知公共牌和 2 张未知公共牌。
> 翻开 2 张未知公共牌，现在有 5 张公共牌，你和对手分别拥有 2 张手牌，共 9 张牌。你不可以选择对手的手牌，对手也不可以选择你的手牌。在你自己的手牌和公共牌中，选取 5 张组成牌型。对手同理。双方牌型可以有重复的公共牌。
> 你和对手都要选择最佳组合，问在两张未知牌的所有可能情况中，你的获胜概率是多少。平局不属于获胜。
> 牌型的比较方法参考下表（上小下大）：
> ![image](https://img2024.cnblogs.com/blog/3516270/202411/3516270-20241125221820189-469594807.png)


题目的本意就是让我们计算概率。众所周知，枚举每种可能的情况后，用可行方案数除以总方案数就是概率。所以直接枚举剩下的 2 张公共牌就行了。剩下的时间交给写代码和调试！



```


|  | // 代码中的注释充满 “Chinglish” 和拼音（因为之前那台机器不支持中文输入法），先凑合着看吧。 |
| --- | --- |
|  | #include |
|  | using namespace std; |
|  | struct card { |
|  | int c, n; |
|  | /* |
|  | c: |
|  | 0 -> Spades |
|  | 1 -> Hearts |
|  | 2 -> Diamonds |
|  | 3 -> Clubs |
|  | n: |
|  | A -> 14 / 1 |
|  | K -> 13 |
|  | Q -> 12 |
|  | J -> 11 |
|  | T -> 10 |
|  | 9-2 |
|  | */ |
|  | }; |
|  | map<int, bool> vis; |
|  | bool operator == (card &x, card &y) { |
|  | return x.c == y.c && x.n == y.n; |
|  | } |
|  | card me[2], en[2], pu[9]; // me, enemy, public(last 2 for users) |
|  | card a[5] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0}, b[5] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0}, c[5] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0}; // compare a and b. a->me, b->enemy; c for the best choice |
|  | int s1, s2, s3; // judge kind of card |
|  | /* |
|  | 9 -> huang jia tong hua shun |
|  | 8 -> tong hua shun |
|  | 7 -> si tiao |
|  | 6 -> mam tang hong |
|  | 5 -> tong hua |
|  | 4 -> shun zi |
|  | 3 -> san tiao |
|  | 2 -> liang dui |
|  | 1 -> yi dui |
|  | 0 -> gao pai |
|  | */ |
|  | bool ok(int i, int j, int k, int l) { |
|  | return !vis[i*20+ j] && !vis[k*20+l] && i * 20 + j != k * 20 + l; |
|  | } |
|  | bool cmp(card &x, card &y) { |
|  | return (x.n != y.n) ? (x.n > y.n) : (x.c < y.c); |
|  | } |
|  | bool cmpa() { |
|  | /* |
|  | return value: |
|  | 1 -> win |
|  | 0 -> lost / draw |
|  | */ |
|  | // diff kind |
|  | if (s1 != s3) return s1 > s3; |
|  | if (s1 == 9) return 0; |
|  | if (s1 == 8) { |
|  | return a[0].n > c[0].n; |
|  | } |
|  | if (s1 == 7) { |
|  | if (a[0].n != c[0].n) return a[0].n > c[0].n; |
|  | return a[4].n > c[4].n; |
|  | } |
|  | if (s1 == 6) { |
|  | if (a[0].n != c[0].n) return a[0].n > c[0].n; |
|  | return a[4].n > c[4].n; |
|  | } |
|  | if (s1 == 5) { |
|  | for (int i = 0; i < 5; i++) { |
|  | if (a[i].n != c[i].n) return a[i].n > c[i].n; |
|  | } |
|  | return 0; |
|  | } |
|  | if (s1 == 4) { |
|  | return a[0].n > c[0].n; |
|  | } |
|  | if (s1 == 3) { |
|  | if (a[0].n != c[0].n) return a[0].n > c[0].n; |
|  | if (a[3].n != c[3].n) return a[3].n > c[3].n; |
|  | return a[4].n > c[4].n; |
|  | } |
|  | if (s1 == 2) { |
|  | if (a[0].n != c[0].n) return a[0].n > c[0].n; |
|  | if (a[3].n != c[3].n) return a[3].n > c[3].n; |
|  | return a[4].n > c[4].n; |
|  | } |
|  | if (s1 == 1) { |
|  | if (a[0].n != c[0].n) return a[0].n > c[0].n; |
|  | for (int i = 2; i < 5; i++) { |
|  | if (a[i].n != c[i].n) return a[i].n > c[i].n; |
|  | } |
|  | return 0; |
|  | } |
|  | for (int i = 0; i < 5; i++) { |
|  | if (a[i].n != c[i].n) return a[i].n > c[i].n; |
|  | } |
|  | return 0; // default |
|  | } |
|  | bool cmpb() { |
|  | /* |
|  | return value: |
|  | 1 -> win |
|  | 0 -> lost / draw |
|  | */ |
|  | // diff kind |
|  | if (s2 != s3) return s2 > s3; |
|  | if (s2 == 9) return 0; |
|  | if (s2 == 8) { |
|  | return b[0].n > c[0].n; |
|  | } |
|  | if (s2 == 7) { |
|  | if (b[0].n != c[0].n) return b[0].n > c[0].n; |
|  | return b[4].n > c[4].n; |
|  | } |
|  | if (s2 == 6) { |
|  | if (b[0].n != c[0].n) return b[0].n > c[0].n; |
|  | return b[4].n > c[4].n; |
|  | } |
|  | if (s2 == 5) { |
|  | for (int i = 0; i < 5; i++) { |
|  | if (b[i].n != c[i].n) return b[i].n > c[i].n; |
|  | } |
|  | return 0; |
|  | } |
|  | if (s2 == 4) { |
|  | return b[0].n > c[0].n; |
|  | } |
|  | if (s2 == 3) { |
|  | if (b[0].n != c[0].n) return b[0].n > c[0].n; |
|  | if (b[3].n != c[3].n) return b[3].n > c[3].n; |
|  | return b[4].n > c[4].n; |
|  | } |
|  | if (s2 == 2) { |
|  | if (b[0].n != c[0].n) return b[0].n > c[0].n; |
|  | if (b[3].n != c[3].n) return b[3].n > c[3].n; |
|  | return b[4].n > c[4].n; |
|  | } |
|  | if (s2 == 1) { |
|  | if (b[0].n != c[0].n) return b[0].n > c[0].n; |
|  | for (int i = 2; i < 5; i++) { |
|  | if (b[i].n != c[i].n) return b[i].n > c[i].n; |
|  | } |
|  | return 0; |
|  | } |
|  | for (int i = 0; i < 5; i++) { |
|  | if (b[i].n != c[i].n) return b[i].n > c[i].n; |
|  | } |
|  | return 0; // default |
|  | } |
|  | bool check() { |
|  | /* |
|  | return value: |
|  | 1 -> win |
|  | 0 -> lost / draw |
|  | */ |
|  | // diff kind |
|  | if (s1 != s2) return s1 > s2; |
|  | if (s1 == 9) return 0; |
|  | if (s1 == 8) { |
|  | return a[0].n > b[0].n; |
|  | } |
|  | if (s1 == 7) { |
|  | if (a[0].n != b[0].n) return a[0].n > b[0].n; |
|  | return a[4].n > b[4].n; |
|  | } |
|  | if (s1 == 6) { |
|  | if (a[0].n != b[0].n) return a[0].n > b[0].n; |
|  | return a[4].n > b[4].n; |
|  | } |
|  | if (s1 == 5) { |
|  | for (int i = 0; i < 5; i++) { |
|  | if (a[i].n != b[i].n) return a[i].n > b[i].n; |
|  | } |
|  | return 0; |
|  | } |
|  | if (s1 == 4) { |
|  | return a[0].n > b[0].n; |
|  | } |
|  | if (s1 == 3) { |
|  | if (a[0].n != b[0].n) return a[0].n > b[0].n; |
|  | if (a[3].n != b[3].n) return a[3].n > b[3].n; |
|  | return a[4].n > b[4].n; |
|  | } |
|  | if (s1 == 2) { |
|  | if (a[0].n != b[0].n) return a[0].n > b[0].n; |
|  | if (a[3].n != b[3].n) return a[3].n > b[3].n; |
|  | return a[4].n > b[4].n; |
|  | } |
|  | if (s1 == 1) { |
|  | if (a[0].n != b[0].n) return a[0].n > b[0].n; |
|  | for (int i = 2; i < 5; i++) { |
|  | if (a[i].n != b[i].n) return a[i].n > b[i].n; |
|  | } |
|  | return 0; |
|  | } |
|  | for (int i = 0; i < 5; i++) { |
|  | if (a[i].n != b[i].n) return a[i].n > b[i].n; |
|  | } |
|  | return 0; // default |
|  | } |
|  | void judgec() { |
|  | // (huang jia) tong hua shun |
|  | if (c[0].c == c[1].c && c[1].c == c[2].c && c[2].c == c[3].c && c[3].c == c[4].c) { |
|  | if (c[0].n == 14 && c[1].n == 13 && c[2].n == 12 && c[3].n == 11 && c[4].n == 10) { // AKQJT |
|  | s3 = 9; |
|  | return; |
|  | } |
|  | if (c[0].n == c[1].n + 1 && c[1].n == c[2].n + 1 && c[2].n == c[3].n + 1 && c[3].n == c[4].n + 1) { // shun zi |
|  | s3 = 8; |
|  | return; |
|  | } |
|  | if (c[0].n == 14 && c[1].n == 5 && c[2].n == 4 && c[3].n == 3 && c[4].n == 2) { // A2345 |
|  | swap(c[0], c[1]); |
|  | swap(c[1], c[2]); |
|  | swap(c[2], c[3]); |
|  | swap(c[3], c[4]); |
|  | c[4].n = 1; |
|  | s3 = 8; |
|  | return; |
|  | } |
|  | } |
|  | // 4 tiao |
|  | if (c[0].n == c[1].n && c[1].n == c[2].n && c[2].n == c[3].n) { |
|  | s3 = 7; |
|  | return; |
|  | } |
|  | if (c[1].n == c[2].n && c[2].n == c[3].n && c[3].n == c[4].n) { |
|  | swap(c[0], c[4]); |
|  | s3 = 7; |
|  | return; |
|  | } |
|  | // 3 dai 2 |
|  | if (c[0].n == c[1].n && c[1].n == c[2].n && c[3].n == c[4].n) { |
|  | s3 = 6; |
|  | return; |
|  | } |
|  | if (c[0].n == c[1].n && c[2].n == c[3].n && c[3].n == c[4].n) { |
|  | s3 = 6; |
|  | // tiao zheng pai de shun xu |
|  | swap(c[0], c[4]); |
|  | swap(c[1], c[3]); |
|  | return; |
|  | } |
|  | // tong hua |
|  | if (c[0].c == c[1].c && c[1].c == c[2].c && c[2].c == c[3].c && c[3].c == c[4].c) { |
|  | s3 = 5; |
|  | return; |
|  | } |
|  | // shun zi |
|  | if (c[0].n == c[1].n + 1 && c[1].n == c[2].n + 1 && c[2].n == c[3].n + 1 && c[3].n == c[4].n + 1) { // shun zi |
|  | s3 = 4; |
|  | return; |
|  | } |
|  | if (c[0].n == 14 && c[1].n == 5 && c[2].n == 4 && c[3].n == 3 && c[4].n == 2) { // A2345 |
|  | s3 = 4; |
|  | c[0].n = 1; |
|  | swap(c[0], c[1]); |
|  | swap(c[1], c[2]); |
|  | swap(c[2], c[3]); |
|  | swap(c[3], c[4]); |
|  | return; |
|  | } |
|  | // 3 tiao |
|  | if (c[0].n == c[1].n && c[1].n == c[2].n) { |
|  | s3 = 3; |
|  | return; |
|  | } |
|  | if (c[1].n == c[2].n && c[2].n == c[3].n) { |
|  | s3 = 3; |
|  | c[3] = c[0]; |
|  | c[0] = c[1]; |
|  | if (c[3].n < c[4].n) swap(c[3], c[4]); |
|  | return; |
|  | } |
|  | if (c[2].n == c[3].n && c[3].n == c[4].n) { |
|  | s3 = 3; |
|  | swap(c[1], c[2]); |
|  | swap(c[2], c[3]); |
|  | swap(c[3], c[4]); |
|  | swap(c[0], c[1]); |
|  | swap(c[1], c[2]); |
|  | swap(c[2], c[3]); |
|  | return; |
|  | } |
|  | // 2 dui |
|  | if (c[0].n == c[1].n) { |
|  | if (c[2].n == c[3].n) { |
|  | s3 = 2; |
|  | return; |
|  | } |
|  | if (c[3].n == c[4].n) { |
|  | s3 = 2; |
|  | swap(c[2], c[3]); |
|  | swap(c[3], c[4]); |
|  | return; |
|  | } |
|  | } |
|  | if (c[1].n == c[2].n) { |
|  | if (c[3].n == c[4].n) { |
|  | s3 = 2; |
|  | swap(c[0], c[1]); |
|  | swap(c[1], c[2]); |
|  | swap(c[2], c[3]); |
|  | swap(c[3], c[4]); |
|  | return; |
|  | } |
|  | } |
|  | // 1 dui |
|  | if (c[0].n == c[1].n) { |
|  | s3 = 1; |
|  | return; |
|  | } |
|  | if (c[1].n == c[2].n) { |
|  | s3 = 1; |
|  | swap(c[2], c[0]); |
|  | return; |
|  | } |
|  | if (c[2].n == c[3].n) { |
|  | s3 = 1; |
|  | swap(c[0], c[2]); |
|  | swap(c[1], c[3]); |
|  | return; |
|  | } |
|  | if (c[3].n == c[4].n) { |
|  | s3 = 1; |
|  | swap(c[0], c[3]); |
|  | swap(c[1], c[4]); |
|  | swap(c[2], c[3]); |
|  | swap(c[3], c[4]); |
|  | return; |
|  | } |
|  | // san pai |
|  | s3 = 0; |
|  | return; |
|  | } |
|  | int main() { |
|  | string s; |
|  | while (cin >> s) { |
|  | if (s == "#") break; |
|  | { |
|  | int num, cd; |
|  | if (s[0] == 'S') cd = 0; |
|  | if (s[0] == 'H') cd = 1; |
|  | if (s[0] == 'D') cd = 2; |
|  | if (s[0] == 'C') cd = 3; |
|  | if (s[1] == 'A') num = 14; |
|  | else if (s[1] == 'T') num = 10; |
|  | else if (s[1] == 'J') num = 11; |
|  | else if (s[1] == 'Q') num = 12; |
|  | else if (s[1] == 'K') num = 13; |
|  | else num = s[1] - '0'; |
|  | me[0] = {cd, num}; |
|  | cin >> s; |
|  | if (s[0] == 'S') cd = 0; |
|  | if (s[0] == 'H') cd = 1; |
|  | if (s[0] == 'D') cd = 2; |
|  | if (s[0] == 'C') cd = 3; |
|  | if (s[1] == 'A') num = 14; |
|  | else if (s[1] == 'T') num = 10; |
|  | else if (s[1] == 'J') num = 11; |
|  | else if (s[1] == 'Q') num = 12; |
|  | else if (s[1] == 'K') num = 13; |
|  | else num = s[1] - '0'; |
|  | me[1] = {cd, num}; |
|  | cin >> s; |
|  | if (s[0] == 'S') cd = 0; |
|  | if (s[0] == 'H') cd = 1; |
|  | if (s[0] == 'D') cd = 2; |
|  | if (s[0] == 'C') cd = 3; |
|  | if (s[1] == 'A') num = 14; |
|  | else if (s[1] == 'T') num = 10; |
|  | else if (s[1] == 'J') num = 11; |
|  | else if (s[1] == 'Q') num = 12; |
|  | else if (s[1] == 'K') num = 13; |
|  | else num = s[1] - '0'; |
|  | en[0] = {cd, num}; |
|  | cin >> s; |
|  | if (s[0] == 'S') cd = 0; |
|  | if (s[0] == 'H') cd = 1; |
|  | if (s[0] == 'D') cd = 2; |
|  | if (s[0] == 'C') cd = 3; |
|  | if (s[1] == 'A') num = 14; |
|  | else if (s[1] == 'T') num = 10; |
|  | else if (s[1] == 'J') num = 11; |
|  | else if (s[1] == 'Q') num = 12; |
|  | else if (s[1] == 'K') num = 13; |
|  | else num = s[1] - '0'; |
|  | en[1] = {cd, num}; |
|  | cin >> s; |
|  | if (s[0] == 'S') cd = 0; |
|  | if (s[0] == 'H') cd = 1; |
|  | if (s[0] == 'D') cd = 2; |
|  | if (s[0] == 'C') cd = 3; |
|  | if (s[1] == 'A') num = 14; |
|  | else if (s[1] == 'T') num = 10; |
|  | else if (s[1] == 'J') num = 11; |
|  | else if (s[1] == 'Q') num = 12; |
|  | else if (s[1] == 'K') num = 13; |
|  | else num = s[1] - '0'; |
|  | pu[0] = {cd, num}; |
|  | cin >> s; |
|  | if (s[0] == 'S') cd = 0; |
|  | if (s[0] == 'H') cd = 1; |
|  | if (s[0] == 'D') cd = 2; |
|  | if (s[0] == 'C') cd = 3; |
|  | if (s[1] == 'A') num = 14; |
|  | else if (s[1] == 'T') num = 10; |
|  | else if (s[1] == 'J') num = 11; |
|  | else if (s[1] == 'Q') num = 12; |
|  | else if (s[1] == 'K') num = 13; |
|  | else num = s[1] - '0'; |
|  | pu[1] = {cd, num}; |
|  | cin >> s; |
|  | if (s[0] == 'S') cd = 0; |
|  | if (s[0] == 'H') cd = 1; |
|  | if (s[0] == 'D') cd = 2; |
|  | if (s[0] == 'C') cd = 3; |
|  | if (s[1] == 'A') num = 14; |
|  | else if (s[1] == 'T') num = 10; |
|  | else if (s[1] == 'J') num = 11; |
|  | else if (s[1] == 'Q') num = 12; |
|  | else if (s[1] == 'K') num = 13; |
|  | else num = s[1] - '0'; |
|  | pu[2] = {cd, num}; |
|  | } // input |
|  | vis[me[0].c * 20 + me[0].n] = vis[me[1].c * 20 + me[1].n] = vis[en[0].c * 20 + en[0].n] = vis[en[1].c * 20 + en[1].n] = vis[pu[0].c * 20 + pu[0].n] = vis[pu[1].c * 20 + pu[1].n] = vis[pu[2].c * 20 + pu[2].n] = 1; |
|  | int ans = 0, cnt = 0; |
|  | for (int i = 0; i < 4; i++) { |
|  | for (int j = 2; j <= 14; j++) { |
|  | for (int k = 0; k < 4; k++) { |
|  | for (int l = 2; l <= 14; l++) { |
|  | pu[3] = {i, j}; |
|  | pu[4] = {k, l}; |
|  | if (!ok(i, j, k, l)) continue; |
|  | memset(a, 0, sizeof(a)); |
|  | memset(b, 0, sizeof(b)); |
|  | memset(c, 0, sizeof(c)); |
|  | s3 = s1 = s2 = 0; |
|  | card t = {0, 0}; |
|  | for (int i = 5; i < 9; i++) pu[i] = t; |
|  | // select card |
|  | for (int m = 0; m < 7; m++) { |
|  | for (int n = m + 1; n < 7; n++) { |
|  | for (int o = n + 1; o < 7; o++) { |
|  | for (int p = o + 1; p < 7; p++) { |
|  | for (int q = p + 1; q < 7; q++) { |
|  | pu[5] = me[0]; |
|  | pu[6] = me[1]; |
|  | c[0] = pu[m]; |
|  | c[1] = pu[n]; |
|  | c[2] = pu[o]; |
|  | c[3] = pu[p]; |
|  | c[4] = pu[q]; |
|  | sort(c, c + 5, cmp); |
|  | judgec(); |
|  | if (!cmpa()) { |
|  | for (int i = 0; i < 5; i++) a[i] = c[i]; |
|  | s1 = s3; |
|  | } |
|  | pu[5] = en[0]; |
|  | pu[6] = en[1]; |
|  | c[0] = pu[m]; |
|  | c[1] = pu[n]; |
|  | c[2] = pu[o]; |
|  | c[3] = pu[p]; |
|  | c[4] = pu[q]; |
|  | sort(c, c + 5, cmp); |
|  | judgec(); |
|  | if (!cmpb()) { |
|  | for (int i = 0; i < 5; i++) b[i] = c[i]; |
|  | s2 = s3; |
|  | } |
|  | } |
|  | } |
|  | } |
|  | } |
|  | } |
|  | // cnt win |
|  | ans += check(); |
|  | cnt++; |
|  | } |
|  | } |
|  | } |
|  | } |
|  | double pwin = (double)ans / cnt; |
|  | cout << fixed << setprecision(20) << pwin << endl; |
|  | vis[me[0].c * 20 + me[0].n] = vis[me[1].c * 20 + me[1].n] = vis[en[0].c * 20 + en[0].n] = vis[en[1].c * 20 + en[1].n] = vis[pu[0].c * 20 + pu[0].n] = vis[pu[1].c * 20 + pu[1].n] = vis[pu[2].c * 20 + pu[2].n] = 0; |
|  | } |
|  | return 0; |
|  | } |


```

可以看出，模拟题想出来正解并不难，主要困扰我们的是难以将思想转化为代码。
只要多加练习，再难的模拟题都能做出来！


#### 作业


1. [\[CSP\-S 2023] 结构体](https://github.com "[CSP-S 2023] 结构体")
2. [\[THUPC 2023 初赛] 乱西星上的空战](https://github.com "[THUPC 2023 初赛] 乱西星上的空战")


