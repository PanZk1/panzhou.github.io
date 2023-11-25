+++
title = "More goodies"
hascode = true
rss = "A short description of the page which would serve as **blurb** in a `RSS` feed; you can use basic markdown here but the whole description string must be a single line (not a multiline string). Like this one for instance. Keep in mind that styling is minimal in RSS so for instance don't expect maths or fancy styling to work; images should be ok though: ![](https://upload.wikimedia.org/wikipedia/en/b/b0/Rick_and_Morty_characters.jpg)"
rss_title = "More goodies"
rss_pubdate = Date(2019, 5, 1)

tags = ["syntax", "code", "image"]
+++

# More goodies

\toc

## More markdown support

The Julia Markdown parser in Julia's stdlib is not exactly complete and Franklin strives to bring useful extensions that are either defined in standard specs such as Common Mark or that just seem like useful extensions.

* indirect references for instance [like so]

[like so]: http://existentialcomics.com/

or also for images

![][some image]

some people find that useful as it allows referring multiple times to the same link for instance.

[some image]: https://upload.wikimedia.org/wikipedia/commons/9/90/Krul.svg

* un-qualified code blocks are allowed and are julia by default, indented code blocks are not supported by default (and there support will disappear completely in later version)

```
a = 1
b = a+1
```

you can specify the default language with `@def lang = "julia"`.
If you actually want a "plain" code block, qualify it as `plaintext` like

```plaintext
so this is plain-text stuff.
```

## A bit more highlighting

Extension of highlighting for `pkg` an `shell` mode in Julia:

```julia-repl
(v1.4) pkg> add Franklin
shell> blah
julia> 1+1
(Sandbox) pkg> resolve
```

you can tune the colouring in the CSS etc via the following classes:

* `.hljs-meta` (for `julia>`)
* `.hljs-metas` (for `shell>`)
* `.hljs-metap` (for `...pkg>`)

## More customisation

Franklin, by design, gives you a lot of flexibility to define how you want stuff be done, this includes doing your own parsing/processing and your own HTML generation using Julia code.

In order to do this, you can define two types of functions in a `utils.jl` file which will complement your `config.md` file:

* `hfun_*` allow you to plug custom-generated HTML somewhere
* `lx_*` allow you to do custom parsing of markdown and generation of HTML

The former (`hfun_*`) is most likely to be useful.

### Custom "hfun"

If you define a function `hfun_bar` in the `utils.jl` then you have access to a new template function `{{bar ...}}`. The parameters are passed as a list of strings, for instance variable names but it  could just be strings as well.

For instance:

```julia
function hfun_bar(vname)
  val = Meta.parse(vname[1])
  return round(sqrt(val), digits=2)
end
```

~~~
.hf {background-color:black;color:white;font-weight:bold;}
~~~

Can be called with `{{bar 4}}`: **{{bar 4}}**.

Usually you will want to pass variable name (either local or global) and collect their value via one of `locvar`, `globvar` or `pagevar` depending on your use case.
Let's have another toy example:

```julia
function hfun_m1fill(vname)
  var = vname[1]
  return pagevar("menu1", var)
end
```

Which you can use like this `{{m1fill title}}`: **{{m1fill title}}**. Of course  in this specific case you could also have used `{{fill title menu1}}`: **{{fill title menu1}}**.

Of course these examples are not very useful, in practice you might want to use it to generate actual HTML in a specific way using Julia code.
For instance you can use it to customise how [tag pages look like](/menu3/#customising_tag_pages).

A nice example of what you can do is in the [SymbolicUtils.jl manual](https://juliasymbolics.github.io/SymbolicUtils.jl/api/) where they use a `hfun_` to generate HTML encapsulating the content of code docstrings, in a way doing something similar to what Documenter does. See [how they defined it](https://github.com/JuliaSymbolics/SymbolicUtils.jl/blob/website/utils.jl).

**Note**: the  output **will not** be reprocessed by Franklin, if you want to generate markdown which should be processed by Franklin, then use `return fd2html(markdown, internal=true)` at the end.

### Custom "lx"

These commands will look the same as latex commands but what they do with their content is now entirely controlled by your code.
You can use this to do your own parsing of specific chunks of your content if you so desire.

The definition of `lx_*` commands **must** look like this:

```julia
function lx_baz(com, _)
  # keep this first line
  brace_content = Franklin.content(com.braces[1]) # input string
  # do whatever you want here
  return uppercase(brace_content)
end
```

You can call the above with `\baz{some string}`: \baz{some string}.

**Note**: the output **will be** reprocessed by Franklin, if you want to avoid this, then escape the output by using `return "~~~" * s * "~~~"` and it will be plugged  in as is in the HTML.

## 学习区

\toc

### 算法区
计算基态能量和基态波函数的算法
1. MordernSC 里面的算法【lanzcos 自动微分等】写note和作业
2. tensor network 
  1. TRG
  2. genericTN
  3. DMRG/CTMRG
  4. 【谢志远老师slides和综述】
   [note 要包括基本物理图像，可以应用的系统，]
3. 量子蒙卡算法【note SSE VMC】
4. 解析延拓算法【note】
5. simulated annealing /quantum annealing / parallel tempering【note】
6. spin-glass 统计物理和机器学习的联系【note】
7. 变分量子算法【note】
8. 限制波尔兹曼机【note】
   

### 凝聚态物理区
1. topo insulator [note + code]
   1. berry phase
   2. chern number QHE
   3. QSH effect
   4. QAH effect
   5. weyl semimetal...nodal line ,majorlana zero mode 
   6. 分数量子霍尔效应**
   7. 拓扑超导
2. 重整化群流 [note + code] 重整化方法**
   1. renormalization group
   2. Kadanoff block spin
   3. Wilson block spin
   4. 重整化群流
   5. 量子相变
3. superconductivity [note + code]
   1. BCS theory
   2. Ginzburg-Landau theory 超导*序参量**
   3. Bogoliubov-de Gennes equation
   4. 格林函数**/隶玻色子
   5. 非常规超导 【cuprate】
   6. 超导相变
4. quantum spin liquid  [note + code]
   1. RVB/spinon
   
5. 安德森局域化 [note + code]

### 量子信息区
1. toric code [note + code] anyon
2. 密度矩阵和纠缠熵，量子系综 [note + code] 

### 数学区
1. 高等代数 ，矩阵分析**
   tensor network [note + code]
2. 群论 [note + code]【李群李代数***】【纤维丛】【点群】
3. 微分几何 [note + code]
   1. 流形
   2. 流形上的张量场
   3. 流形上的微分形式
   4. 流形上的度量
   5. 流形上的联络
   6. 流形上的曲率
4. 复分析** 泛函分析
5. 偏微分方程** 数值技巧【数学物理方程】
6. 范畴论
7. 辛几何与哈密顿雅可比方程

## 项目区
### topo-insulator project
1. 边复现python代码 边按综述写note，边写代码
2. 提出可能的项目方向【wTe 二维转角材料】 

### universal hamiltonian project
1. 复现SSE on Rydberg atom array [quantum spin liquid ]
2. QMC sign problem 【调研+note+读孟子杨老师组避免sign problem 论文】
3. 对此论文idea 进行推进 Quantum optimization within lattice gauge theory model on a quantum simulator 【用GTN 简化模型】
4. 研究哈密顿量的UMA-complete性质 有效哈密顿量【探究是否能模拟超导量子体系-强关联，量子长程纠缠系统，经典长程纠缠，拓扑保护体系spin liquid ,spin glass, topo体系】**
5. 里德保原子阵列作为模拟器


universal H 在Rydberg体系下模拟平衡态系统，模拟平衡态系统的动力学演化，格林函数可以表示含时关联响应，也可描述定态微扰响应（定态有效哈密顿量） 经典哈密顿量演化（自旋玻璃态）基态简并性质【从一个基态到另一个基态跃迁中的拓扑保护效应（QA，SA，SSE，QMC）
