---
layout: post
title: ActiveRecord Enum实战总结
categories: [Tech]
tags: [rails, active-record, enum]
---

![](/images/Bing_711.JPG)

## 基本使用方法
案例说明: 给已经存在的 Company 增加一个 size 属性, 属性包括 large, medium, small 三个选项

从 Rails4.1 开始，可以通过 `ActiveRecord::Enum` 来快速实现这样的功能

{% highlight ruby %}
class Company < ActiveRecord::Base
  enum size: [:large, :medium, :small]
end
{% endhighlight %}

定义了 `enum` 后，Rails会自动产生下面这些方法

{% highlight ruby %}
Company.sizes
# => {"large"=>0, "medium"=>1, "small"=>2}

# Scope methods
Company.large
Company.medium
Company.small

# Query methods
company.large?
company.medium?

# Action methods
company.large!
company.small!
{% endhighlight %}

## Migration

`enum` 的实现是基于该字段是一个 integer 类型的。所以添加这个新的字段我们需要下面的 migration

{% highlight ruby %}
class AddSizeToCompanies < ActiveRecord::Migration
  def change
    add_column :companies, :size, :integer, null: false, default: 0
    add_index  :companies, :size
  end
end
{% endhighlight %}

## 赋值操作

通过上面 Company.sizes 方法，我们看到 Rails 默认是从0开始对应的。所以上面的 migration 中默认值是0。当新建一个Company时，默认 company 的 size 是 large

{% highlight ruby %}
company = Company.new
company.size # => "large"

# 多种赋值操作
company.size = :medium
company.medium? # => true

company.size = 2
company.small? # => true

company.large = 'large'
company.large? # => true
{% endhighlight %}

`enum` 会自动添加一些验证，如果给 size 属性赋错误的值，Rails会抛出异常

{% highlight ruby %}
company.size = 5
# => ArgumentError: 5 is not a valid billing_category
company.size = :bala
# => ArgumentError: 'bala' is not a valid billing_category
{% endhighlight %}

## Form下拉菜单的填充

当在前端的Form中填充到下拉列表中，可以这样做

{% highlight ruby %}
f.input :size, collection: Company.sizes.keys.map {|s| [s.titleize, s]}, prompt: "Select a size"
{% endhighlight %}

## 需要注意的地方:

由于存到数据库的仍然是 number，所以如果有别的应用也使用同样的数据库，那么该应用需要知道对应关系。

Rails4.1 及之后的版本中，由于 `enum` 会自动产生一些方法，所以要特别注意选项的命名问题，尽量用明确的命名。 另外如果你还需要不同的属性，拥有相同的选项，那么你可以考虑这个 gem [activerecord-enum-without-methods](https://rubygems.org/gems/activerecord-enum-without-methods/versions/1.0.0)

同时在 Rails5 中，就可以[使用`_prefix`和`_postfix`选项](https://github.com/rails/rails/pull/19813)来避免相应的问题。·

## 更好的Migration方法
上面的migration方法对于产品环境已经有数据的情况下，可能会产生问题。

对于Mysql 数据库，新加字段的时候对于已有的记录，mysql 会自动设置 `NOT NULL` 的已有记录为 default
但是对于 postgreSQL 就会出现下面的错误：

> PG::NotNullViolation: ERROR: column "size" contains null values

这时可以先`add_column`添加字段，不要加 NOT NULL 的限制，然后更新已有数据，然后再通过`change_column_null`来添加 `NOT NULL` 限制。
