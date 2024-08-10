# Language highlighting

Docsify uses [Prism](https://prismjs.com) to highlight code blocks in your pages. Prism supports the following languages by default:

* Markup - `markup`, `html`, `xml`, `svg`, `mathml`, `ssml`, `atom`, `rss`
* CSS - `css`
* C-like - `clike`
* JavaScript - `javascript`, `js`

Support for [additional languages](https://prismjs.com/#supported-languages) is available by loading the language-specific [grammar files](https://cdn.jsdelivr.net/npm/prismjs@1/components/) via CDN:

```html
<script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-bash.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-php.min.js"></script>
```

To enable syntax highlighting, wrap each code block in triple backticks with the [language](https://prismjs.com/#supported-languages) specified on the first line:

````
```html
<p>This is a paragraph</p>
<a href="//docsify.js.org/">Docsify</a>
```

```bash
echo "hello"
```

```php
function getAdder(int $x): int 
{
    return 123;
}
```
````

The above markdown will be rendered as:

```html
<p>This is a paragraph</p>
<a href="//docsify.js.org/">Docsify</a>
```

```bash
echo "hello"
```

```php
function getAdder(int $x): int 
{
    return 123;
}
```

## Highlighting Dynamic Content
Code blocks [dynamically created from javascript](https://docsify.js.org/#/configuration?id=executescript) can be highlighted using the method `Prism.highlightElement` like so:

```javascript
var code = document.createElement("code");
code.innerHTML = "console.log('Hello World!')";
code.setAttribute("class", "lang-javascript");
Prism.highlightElement(code);
```

```html
Languang

- Translations
    - [:cn: 中文简体](/zh-cn/)
    - [:uk: English](/)
    - [:de: Deutsch](/de-de/)
    - [:es: Español](/es/)
    - [:ru: Русский](/ru-ru/)
```

The C# code will be highlighted __after__ the rest of this document.

```csharp
public class Foo : IBar<int> {
	public string Baz { get; set; } = "foo";
}
```

The CSS code will be highlighted with this document because CSS has already been loaded.

```css
a:hover {
	color: green !important;
}
```

rust

```rust
/// 注册

mod account;
mod processor;
mod state;
mod error;
mod transaction;
mod keypair;


/// 整数计算溢出
fn wrapping_add() {
    println!("############ wrapping_add ############");

    // u32位无符号整数最大值
    let a: u32 = 4_294_967_295;
    let b: u32 = 1;
    // 使用 wrapping_* 方法在所有模式下都按照补码循环溢出规则处理，如果一个值超过最大值，它会从0开始重新计数。
    // 返回 0
    let result_wrapping = a.wrapping_add(b);
    println!("Original: {}", a);
    println!("Add 1: {}", b);
    println!("Wrapping a + b Result: {}", result_wrapping);

    // saturating_* 在整数溢出时返回相应类型的最大值，如果是下溢，则返回最小值。
    let result_saturating = a.saturating_add(b);
    // 返回 4294967295
    println!("Saturating Result: {}", result_saturating);

    // checked_* 返回值为Option，溢出时为 None 值，否则为Some(result)。可以检查操作是否导致了溢出。
    let result_checked = a.checked_add(b);
    // 返回 None
    match result_checked {
        Some(result) => println!("Checked Result: {}", result),
        None => println!("Checked Result: Overflow!"),
    }

    // overflowing_* 返回值为(result, overflowed)，结果值和一个指示是否存在溢出的布尔值
    let (result_overflowing, overflowed) = a.overflowing_add(b);
    // 返回 (0, true)
    if overflowed {
        println!("Overflowing Result: Overflow!");
    } else {
        println!("Overflowing Result: {}", result_overflowing);
    }
}

#[cfg(test)]
mod tests {
    use log::{debug, info};
    use socala_common::logs_factory::initialization;

    #[test]
    fn it_works() {
        // init 日志
        initialization(Some("../log4rs.yaml"));
        info!("program lib it_works info");
        debug!("program lib it_works debug");

        assert_eq!(2 + 2, 4);
    }
}

```

## HTML

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
