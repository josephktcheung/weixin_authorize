# WeixinAuthorize

## Installation

Add this line to your application's Gemfile:

  `gem 'weixin_authorize'`

  Or

  `gem 'weixin_authorize', git: "https://github.com/lanrion/weixin_authorize.git"

And then execute:

  `$ bundle`

Or install it yourself as:

  `$ gem install weixin_authorize`

## Usage

### Init a `client`

```ruby

$client ||= WeixinAuthorize.configure do |config|
  config.app_id     = ENV["APPID"]
  config.app_secret = ENV["APPSECRET"]
  config.expired_at = Time.now.to_i
end

# Or

$client ||= WeixinAuthorize::Client.new(ENV["APPID"], ENV["APPSECRET"])

```

### 获取用户管理信息

* [获取用户基本信息](http://mp.weixin.qq.com/wiki/index.php?title=获取用户基本信息)

  `user_info = $client.user(ENV["OPENID"])`

* [获取关注者列表](http://mp.weixin.qq.com/wiki/index.php?title=获取关注者列表)

  `followers = $client.followers`


### [分组管理接口](http://mp.weixin.qq.com/wiki/index.php?title=分组管理接口)

* 创建分组:

  `group = $client.create_group("test")`

* 查询所有分组:

  `groups = $client.groups`
* 查询用户所在分组:

  `group = $client.get_group_for(ENV["OPENID"])`

* 修改分组名:

  `group = $client.update_group_name(ENV["OPENID"], "new_group_name")`

* 移动用户分组:

  `group = $client.update_group_for_openid(ENV["OPENID"], "to_groupid")`

### 自定义菜单

* [自定义菜单创建接口](http://mp.weixin.qq.com/wiki/index.php?title=自定义菜单创建接口)

  `response = $client.create_menu(menu) # Hash or Json`

* [自定义菜单查询接口](http://mp.weixin.qq.com/wiki/index.php?title=自定义菜单查询接口)

  `response = $client.menu`

* [自定义菜单删除接口](http://mp.weixin.qq.com/wiki/index.php?title=自定义菜单删除接口)

  `response = $client.delete_menu`

### [发送客服信息](http://mp.weixin.qq.com/wiki/index.php?title=发送客服信息)

* 发送文本信息:

  `$client.send_text_custom(to_user, content)`

* 发送图片信息:

  `$client.send_image_custom(to_user, media_id)`

* 发送语音消息:

  `$client.send_voice_custom(to_user, media_id)`

* 发送视频消息:

  `$client.send_video_custom(to_user, media_id, options)`

* 发送音乐消息:

  `$client.send_music_custom(to_user, media_id, musicurl, hqmusicurl, options)`

* 发送图文消息:

  `$client.send_news_custom(to_user, *articles)`

## How to test

Go to https://github.com/lanrion/weixin_authorize/issues/2, apply a weixin sandbox test account and follow this account, then add them to your `~/.bash_profile`

```
export APPID="your test account weixin app_id"
export APPSECRET="your test account weixin appsecret"
export OPENID="your weixin openid"
```
Last, you have to **open a new terminal tag (Reload bash_profile)** , and run `rspec .`

## 多用户微信营销平台的对接

> 对于多用户微信营销平台的对接，需要把每次的expired_at, access_token保存在Redis中,每次使用，则可以从Redis中获取expired_at和access_token, 即 `@client = WeixinAuthorize::Client.new(appid, appsecret, expired_at, access_token)`, 获取access_token，则仍然是：`@client.get_access_token`来获取.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
