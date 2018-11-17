# trailofbits/twa [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 小小网络，审查员(bash) 」

[中文](./readme.md) | [english](https://github.com/trailofbits/twa)

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'trailofbits/twa' -->
<!-- commit = 'dc4151f9b679fa42f09636911a8693329dfee737' -->
<!-- time = '2018-11-10' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2018-11-10 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/trailofbits/twa.svg
[commit]: https://github.com/trailofbits/twa/tree/dc4151f9b679fa42f09636911a8693329dfee737

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

# twa

[![Build Status](https://travis-ci.org/trailofbits/twa.svg?branch=master)](https://travis-ci.org/trailofbits/twa)
![Docker Build Status](https://img.shields.io/docker/build/trailofbits/twa.svg)

**小**审计，大脾气 (网络)

> A **t**iny **w**eb **a**uditor ==> twa

## 用法

### 依赖

你需要`bash`4,`curl`,`dig`,和`nc`,以及一个还行的 POSIX 系统.

### 审计

```bash
# 审核网站。
$ twa google.com
> FAIL(google.com): TWA-0102: HTTP redirects to HTTP (not secure)
> FAIL(google.com): TWA-0205: Strict-Transport-Security missing
> MEH(google.com): TWA-0206: X-Frame-Options is 'sameorigin', consider 'deny'
> FAIL(google.com): TWA-0209: X-Content-Type-Options missing
> PASS(google.com): X-XSS-Protection specifies mode=block
> FAIL(google.com): TWA-0214: Referrer-Policy missing
> FAIL(google.com): TWA-0219: Content-Security-Policy missing
> FAIL(google.com): TWA-0220: Feature-Policy missing
> PASS(google.com): Site sends 'Server', but probably only a vendor ID: gws
> PASS(google.com): Site doesn't send 'X-Powered-By'
> PASS(google.com): Site doesn't send 'Via'
> PASS(google.com): Site doesn't send 'X-AspNet-Version'
> PASS(google.com): Site doesn't send 'X-AspNetMvc-Version'
> PASS(google.com): No SCM repository at: http://google.com/.git/HEAD
> PASS(google.com): No SCM repository at: http://google.com/.hg/store/00manifest.i
> PASS(google.com): No SCM repository at: http://google.com/.svn/entries
> PASS(google.com): No environment file at: http://google.com/.env
> PASS(google.com): No environment file at: http://google.com/.dockerenv

# 审核网站，并且详细。
$ twa -v google.com

# 审核站点及其www子域。
$ twa -w google.com
```

`twa`一次只搞一个域,并且在使用`-w`时，多个域只审核一次。如果您需要审核多个域,请多次运行它.

每个结果行包含一个测试结果,如下所示:

```
TYPE(domain): explanation
```

这里的`TYPE`是`PASS`,`MEH`,`FAIL`,`UNK`,`SKIP`,和`FATAL`中的一个:

- `PASS`:测试通过，带有绚丽的色彩.
- `MEH`:测试通过了,但有一件或多件事情可以改进.
- `FAIL`:测试失败,应该修复.
- `UNK`:服务器给了我们一些我们不理解的东西.
- `SKIP`:服务器给了我们一些我们理解的东西,但我们还没有处理.
- `FATAL`:一个非常重要的测试失败了,应立即修复.

如果`TYPE`是不好的(即`MEH`,`FAIL`, 要么`FATAL`)，解释以格式参考代码`TWA-XXYY`为前缀，这里的`XX`是结果发生的阶段，和`YY`是结果的唯一标识符.

### 评分

`twa`可以和`tscore`一起使用，它提供了一个基本的评分机制:

```bash
$ twa google.com | tscore
> 35 9 1 6 0 0 0
```

分数格式逐个的意思是`score npasses nmehs nfailures nunknowns nskips totally_screwed`,所以你可以这样做:

```bash
$ read -r score npasses nmehs nfailures nunknowns nskips totally_screwed < <(twa google.com | tscore)
$ echo "score: ${score}"
```

像`twa`,这个`tscore`是可选的。您可以通过编辑，来更改其选项(即其分数权重).

### Docker

`twa`可以使用轻型(29MB)Alpine Docker 容器.

要从 Docker Hub 安装它:

```bash
$ docker pull trailofbits/twa
```

或者,手动构建它:

```bash
$ git clone https://github.com/trailofbits/twa.git
$ cd twa
$ docker build -t twa .
$ docker run -it twa:latest -vw google.com
```

## 贡献

看看[贡献指南](https://github.com/trailofbits/twa/blob/master/CONTRIBUTING.md).
