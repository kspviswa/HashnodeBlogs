---
title: "go-தமிழ்"
datePublished: Mon Apr 12 2021 03:55:39 GMT+0000 (Coordinated Universal Time)
cuid: ckzeyvups0a6z96s10gcag1bi
slug: go-e0aea4e0aeaee0aebfe0aeb4e0af8d-tamil-transliteration-tool-in-golang-for-fun-learning-a59eb87d9b7f
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681452161479/46a9c6e8-1217-48b6-86ed-53d3bd0a13a3.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1681452009097/4fedee09-e985-4281-9a73-08536c7d3dfc.png
tags: go

---

In this post, I’m gonna talk about my new project, **go-தமிழ் — Tamil transliteration tool in golang for fun & learning** .

தமிழ் (Thamizh not Tamil) is my mother tongue. It is absolutely fantastic to see thamizh letters in internet. Due to recent developments in typographic / indic technologies, it is now very easy to type & view in native languages.

At a very basic level, native languages are represented as **Unicode, UTF-8, UTF-16, UTF-32** special characters. This way, computers can make sense of every char of every possible language as just an integer.

Although handling **UTF-8** strings is definitely a pain, `golang` seems to support this out of the box. Especially their `[unicode/utf8](https://golang.org/pkg/unicode/utf8/)` [package](https://golang.org/pkg/unicode/utf8/) is worth a read.

Having known that `golang` can support தமிழ் natively & having learnt the basics of `golang`, why not develop a english -&gt; தமிழ் transileration tool ??

### Basics of `go-தமிழ்`

`தமிழ்` can be largely categorized as உயிர் ( Primary), மெய் (Secondary) & உயிர்மெய் (Vowels).

For example `தமிழ்` letter `க` is derived from `க்` ( which is `மெய்`) and `அ` ( which is `உயிர்`).  
i.e `க் + அ = க`. Similalry `மி = ம் + இ`.

However in unicode world, the vowels appear as special character. They appear in ், ா, ி form only. So in unicode world, inorder to get `மி`, we should concatenate `ம` & \` ி \` i.e `மி = ம + ி` .

So it turns out that, generating tamil characters is quite challenging & interesting. Upon receiving a english transileration text say `vanakkam`, we need first find the pattern of difference between printing a `உயிர்` & `உயிர்மெய்`.

For instance, `vaa` can be interpreted as `வஅ` or `வா`. So it is quite clear that, we need a mechanism to identify whether the user wants to pronounce vowel sounds, or they want to get the actual letter here. In-order to solve this problem, I resorted to have my own encoding scheme for `go-தமிழ்`.

### Architecture of `go-தமிழ்`

Having decided that, I need to come up with my own encoding rules ( heck this is my own new encoding tool for fun! ), I then started to lay out basic grammer for my own `tanglish` language.

You can take look at the grammar for `go-தமிழ்` in the `help` page of the webpage that gets served as part of `go-தமிழ்` daemon mode.

To give you some glimpse…

உயிர் | Primaryதமிழ்Englishஅaஆ2aஇiஈ2iஉuஊ2uஎeஏ2eஐ3iஒoஓ2o

For complete details on `go-தமிழ்` encoding rules, please [this page](https://github.com/kspviswa/gothamizh/blob/master/help.html).

### Algo

* Get the input text and split it based on space delimiter, resulting in `slice` of input tokens.
    
* Now iterate over each token and perceive every letter of input token as in-turn a `slice`.
    
* By using [Golang slicing of the slice](https://blog.golang.org/go-slices-usage-and-internals) technique, iterate from `0` to `len(token)`.
    
* Match the new slice with either uyir, mei or vowels pattern.
    
* If found, then increment both start & end indices.
    
* If not, then increment only end and re-slice the slice from `start:end` pattern.
    
* Loop & repeat till exit.
    

### Deployment

After the main logic got working, now it is just a matter of how to present & package the tool. Usablity is the key aspect here.

Next, inorder to spice up the meal, I decided to have 2 modes of operation — *Console* mode & *Daemon* mode.

**Console** mode will mimic a `go-தமிழ் >>` shell, which takes in english input and return `தமிழ்` text in the terminal out ( if terminal support is there for UTF-8).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1644375777215/YtQNkqVIb.png align="left")

**Daemon** mode will run a webserver at port 8080 and it will serve **transliteration as a service** . For this, I shamelessly copied [Golang playground CSS](https://play.golang.org/) and re-used to my theme. I have to say, it perfectly fitted to my design and I’m kinda proud of it :-)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1644375779524/wbPFQC8k_.png align="left")

### Final thoughts..

Just before saying Cya

Looking back, I now realize that, I was able to take a crazy notion of `go-தமிழ்` to a prototype which actually works. Though this is no where near to production grade deployment, I’m kinda convinced that it has ample potential. But then wait, this is supposed to be a fun project, just to get myself familiarized with `golang`.

Both `lsgo` & `go-தமிழ்` projects, clearly convince me that, `golang` is a fun & intutive language. Thoughts just flow through and the language doesn’t prohibit by its syntactial structure or lexical grammars. I just love programming in `golang`.

If you happen to like my project, feel free to let me know your thoughts and I would love to hear them. Cya in my next blogpost..

* A learning Gopher…