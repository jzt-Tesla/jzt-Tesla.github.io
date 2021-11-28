## 遇见的一些问题

1. 启动本地 jekyll 进行测试的时候，先开启本地服务命令 jekyll serve 成功后本地修改需要更新页面，使用命令"ctrl + C"，然后输入"Y"停止服务，再重复 jekyll serve 就成功了。
2. 在博客的 md 文件中，需要插入图片，没用图床怎么办？当然可以参考这篇文章自建图床[GitHub + jsDelivr + PicGo + Imagine 打造稳定快速、高效免费图床](https://blog.csdn.net/qq_39047625/article/details/103048865).当然我觉得用 github 的图床速度还是太慢了，我先到 CSDN 上传文章，成功后就获得了免费的图床信息，再直接复制粘贴 md 文章就搞定了。
3. 在使用网站统计的时候，因为原来的使用方法和代码已经失效了，我是直接到网站上注册的，非常简单，有手就行。[百度网站统计](https://tongji.baidu.com/web/welcome/basic)
4. 在使用 gittalk 的时候有蛮多坑，最全的是这篇文章[Gitalk 评论插件，为你的博客快速集成评论功能](https://www.exception.site/essay/how-to-install-gitalk-on-your-blog)
5. 搭建 jekyll 的过程中，我参考的是这篇文章[搭建 jekyll](https://blog.csdn.net/qq_27032631/article/details/106156088)
6. 配置域名的过程中，阿里云 DNS 解析，最好是自己的 github 域名的地址，譬如我的是这个: ping jzt-tesla.github.io 得到的 ip：185.199.110.153
7. 在参考 qiubaiying 的博客右上角是【home，about，tags】，而我的是【主页，博客，关于我】。内容不变，顺序变化了，只需要在项目里面改变这三个 html 文件的名称顺序，它们是按照项目中名字的顺序排列的。

## 致谢

1. 这个模板是从这里 [qiubaiying](hhttps://github.com/qiubaiying/qiubaiying.github.io) fork 的, 感谢这个作者。 他的博客[BY Blog](http://qiubaiying.github.io)
2. 感谢 Jekyll、Github Pages 和 Bootstrap!

## License

遵循 MIT 许可证。有关详细,请参阅 [LICENSE](https://github.com/qiubaiying/qiubaiying.github.io/blob/master/LICENSE)。
