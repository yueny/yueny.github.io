# Redis

![Why is Redis so fast?](https://assets.ng-tech.icu/item/20230622211042.png)

- Redis 是一个基于 RAM 的数据库。RAM 访问比随机磁盘访问至少快 1000 倍。

- Redis 利用 IO 复用和单线程执行循环来提高执行效率。

- Redis 利用了几个高效的底层数据结构。

## Links

- https://zhuanlan.zhihu.com/p/68694458
- https://zhuanlan.zhihu.com/p/65013389q
- https://zhuanlan.zhihu.com/p/65013389
- https://mp.weixin.qq.com/s/o_7u5CpP-683I_eaLPAj6g
- https://mp.weixin.qq.com/s/8_xuX1GJXo-DWTma_2z9mw?from=groupmessage&isappinstalled=0 Redis 中设置了过期时间的 Key，那么你还要知道些什么？
- 为什么 Redis 集群有 16384 个槽


<!-- prettier-ignore -->
```html
<script>
  window.$docsify = {
    search: 'auto', // default

    search: [
      '/',            // => /README.md
      '/guide',       // => /guide.md
      '/get-started', // => /get-started.md
      '/zh-cn/',      // => /zh-cn/README.md
    ],

    // complete configuration parameters
    search: {
      maxAge: 86400000, // Expiration time, the default one day
      paths: [], // or 'auto'
      placeholder: 'Type to search',

      // Localization
      placeholder: {
        '/zh-cn/': '搜索',
        '/': 'Type to search',
      },

      noData: 'No Results!',

      // Localization
      noData: {
        '/zh-cn/': '找不到结果',
        '/': 'No Results',
      },

      // Headline depth, 1 - 6
      depth: 2,

      hideOtherSidebarContent: false, // whether or not to hide other sidebar content

      // To avoid search index collision
      // between multiple websites under the same domain
      namespace: 'website-1',

      // Use different indexes for path prefixes (namespaces).
      // NOTE: Only works in 'auto' mode.
      //
      // When initialiazing an index, we look for the first path from the sidebar.
      // If it matches the prefix from the list, we switch to the corresponding index.
      pathNamespaces: ['/zh-cn', '/ru-ru', '/ru-ru/v1'],

      // You can provide a regexp to match prefixes. In this case,
      // the matching substring will be used to identify the index
      pathNamespaces: /^(\/(zh-cn|ru-ru))?(\/(v1|v2))?/,
    },
  };
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
```
