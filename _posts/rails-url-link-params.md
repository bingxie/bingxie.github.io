##get the current URL.
request.original_url

def original_url
  base_url + original_fullpath
end

request.request_uri //only returns the relative URL


http://#{request.host}:#{request.port+request.fullpath}


link_to and url_for generate absolute URLs by default in templates
If you use the _url suffix, the generated URL is absolute. Use _path to get a relative URL.

##url_for


url_for 用于产生任何应用内的url，即使你没有获取到model的实例。比如用在产生的email中。

举例：
如果在prodocts的controller中，
= url_for(action: :index, only_path: false)
生成： http://localhost:3000/products
only_path: fasle，指定生成的URL要带有域名

如果不是products controller，也可以通过controller参数指定
url_for(action: :index, controller: 'products')

还可以通过anchor参数，设定anchor
= url_for(action: :index, anchor: 'price')

通过指定一个model的实例，可以产生访问model展示页面的url
= url_for(@product)

具体的实现是通过调用model的to_param得到的返回值来拼装url
所以可以通过重写to_param方法，来自定义url，比如实现friendly URL

url_for(:back)

如果request.env["HTTP_REFERER"]有设置值，那么就跳转到该url
如果没有，就会生成javascript:history.back()

##link_to
link_to(name = nil, options = nil, html_options = nil, &block) public
link_to中间的options参数，实际是传递给url_for,用于生成URL
html_options参数，就用于指定html相关的属性值。
带有block的使用方法，可以用于连接的文字生成比较复杂的情况


link_to “Back”, :back

常见的一些html_options:
id: 指定id属性
class: 指定css
:date 用于指定自定义的data属性
:method  生成表单，点击连接提交的方法，包括：:post, :delete, :patch, :put
remote: true, 通过js来发送ajax请求
confirm: 'question?'  点击后弹出确认框
:disable_with 设定当点击按钮提交时，disabled的提交按钮显示的内容，比如“提交中...”

link_to can also produce links with anchors or query strings:
link_to "Comment wall", profile_path(@profile, anchor: "wall")
# => <a href="/profiles/1#wall">Comment wall</a>

link_to "Ruby on Rails search", controller: "searches", query: "ruby on rails"
# => <a href="/searches?query=ruby+on+rails">Ruby on Rails search</a>

link_to "Nonsense search", searches_path(foo: "bar", baz: "quux")
# => <a href="/searches?foo=bar&baz=quux">Nonsense search</a>

如果没有传入任何的path参数，当前的params就会作为options参数。
下面的实例是最常见的添加或者替换参数值的方法
<%= link_to 'lowest prices', params.merge(sort: 'end_date') %>
<%= link_to 'relevance', params.merge(sort: 'default') %>
如果参数值传入nil，那么该参数就会被删除，这个就可以作为默认的参数

link_to some url with current params
link_to "some text", users_path(:params => params, :more_params => "more params")


##Performance
如果你担心性能，可以把path的生成方法直接用字符串拼装，并且封装在一个方法中。
# In PostDecorator
def path
  "/blog/posts/#{model.to_param}"
 end

http://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to
