## My first Post on blog##
  
(　ﾟдﾟ)虽然早就计划弄个blog了却一直没有行动  
总之，俺终于有自己的blog了！  
>好像有很多感想又不知道该写点啥...  
>稍微写下这个GitHub Pages搭建过程.md好了  
>(顺便练习下markdown语法...   
>(结果一下午就这么过去了... 

### 准备材料：

```
1.一台能上网的电脑
2.GitHub帐号一个
```
### 副标题好难写啊ˊ_>ˋ
作为一个GitHub Pages版的博客，github很贴心的提供了运行的服务器。  
>大致胡扯一下**原理**：  
>Github服务器给叫`***.github.io`的repo配了[Jekyll](https://github.com/jekyll/jekyll)，一个静态页面生成器。  
>简单来说就是按照[特定目录结构](https://jekyllrb.com/docs/structure/)存东西(博文.md,图片.png啥的)，再修改一些[配置](https://jekyllrb.com/docs/configuration/)文件，Jekyll就会把这些东西揉成一个静态站点了。  
>而GitHub负责存储这些静态资源和提供Jekyll服务。  

#### 所以我们要做的只有  

* (可选,但强烈推荐)安装一大堆[东西](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/);(如果想在本地编辑文件预览效果的话)
* 上面说的很[简单](http://jmcglone.com/guides/github-pages/)(也可以很[复杂](https://help.github.com/articles/adding-a-jekyll-theme-to-your-github-pages-site/)...)的配置;  
* 用[markdown]()写博文页面;  

#### 当然，也许可以简化(真的是简化...)一下

* `mkdir YOUR_GITHUB_ID.github.io`, `git init`;
* 找到一个你比较喜欢的别人的GitHub Page页面，进repo [fork](https://help.github.com/articles/fork-a-repo/)之; 
* `git clone YOUR_FORKED_REPO_URL`到上面新建的目录后进行"个性化的"编辑;
* 用[markdown]()写博文页面, 写好了`git commit -a`;  
* 编辑和博文写完了`git commit -m '我做了些啥'`
* `git push -u origin master`
* 访问`https://YOUR_GITHUB_ID.github.io`看看效果  

直到写这个搭建过程的时候我才知道这个简单的步骤...感觉浪费了好多时间。  
***
写完这篇发现自己整理知识能力还是太差了，这破文看得懂的人大概也轮不到看我这篇来学Github Pages，看不懂的人看完了还是不知道要弄个GitHub Pages博客需要干啥干啥...只能给自己看看当个笔记—对！
>“写这个搭建过程的时候其实是在回忆自己昨天干了些啥，打个比方的话相当于重新做了一次作业，巩固了昨天所学的知识！”  

"这么想就会感觉自己没浪费这一天了"，放学回家路上，我如此安慰着自己...
