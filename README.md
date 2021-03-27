# maven-note

## [Using Mirrors for Repositories](https://maven.apache.org/guides/mini/guide-mirror-settings.html#using-mirrors-for-repositories)

With Repositories you specify from which locations you want to download certain artifacts, such as dependencies and maven-plugins.

设置仓库是为了下载构件，如依赖和插件。

Note that there can be at most one mirror for a given repository. In other words, you cannot map a single repository to a group of mirrors that all define the same <mirrorOf> value. Maven will not aggregate the mirrors but simply picks the first match. If you want to provide a combined view of several repositories, use a repository manager instead.
  
对于一个仓库，最多只能为其配置一个镜像。

You can force Maven to use a single repository by having it mirror all repository requests. The repository must contain all of the desired artifacts, or be able to proxy the requests to other repositories. This setting is most useful when using an internal company repository with a Maven Repository Manager to proxy external requests.

通过镜像所有的仓库请求，我们可以强迫 Maven 只使用一个仓库。前提是这个镜像包含所有所需的构件，或者能够代理请求到其他仓库。以下是相关的配置。

```xml
<settings>
  ...
  <mirrors>
    <mirror>
      <id>internal-repository</id>
      <name>Maven Repository Manager running on repo.mycompany.com</name>
      <url>http://repo.mycompany.com/proxy</url>
      <mirrorOf>*</mirrorOf>
    </mirror>
  </mirrors>
  ...
</settings>
```

A single mirror can handle multiple repositories. 

镜像和仓库之间是一个单向**1对多**的关系。


Be careful not to include extra whitespace around identifiers or wildcards in comma separated lists. For example, a mirror with <mirrorOf> set to !repo1, * will not mirror anything while !repo1,* will mirror everything but repo1.
  
要设置镜像时，要注意不能再通配符或仓库ID周围使用空白。

When you use the advanced syntax and configure multiple mirrors, keep in mind that their declaration order matters. When Maven looks for a mirror of some repository, it first checks for a mirror whose <mirrorOf> exactly matches the repository identifier. If no direct match is found, Maven picks the first mirror declaration that matches according to the rules above (if any). Hence, you may influence match order by changing the order of the definitions in the settings.xml
  
存在多个镜像时，它们的声明顺序是非常重要的。
