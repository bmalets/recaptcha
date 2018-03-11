# reCAPTCHA

Fork of https://github.com/ambethia/recaptcha
Updates:
- gem does not use your ENV variables
- configure gem from initializers (`Recaptcha.configure { |config| ... }`)

## Rails Installation

[obtain a reCAPTCHA API key](https://www.google.com/recaptcha/admin). Note: Use localhost or 127.0.0.1 in domain if using localhost:3000.

```Ruby
gem 'recaptcha', github: 'bmalets/recaptcha', require: 'recaptcha/rails'
```

Set SITE_KEY and SECRET_KEY:

```Ruby
# config/initializers/recaptcha.rb
Recaptcha.configure do |config|
  config.site_key  = '6Lc6BAAAAAAAAChqRbQZcn_yyyyyyyyyyyyyyyyy'
  config.secret_key = '6Lc6BAAAAAAAAKN3DRm6VA_xxxxxxxxxxxxxxxxx'
  # Uncomment the following line if you are using a proxy server:
  # config.proxy = 'http://myproxy.com.au:8080'
end
```

Add `recaptcha_tags` to the forms you want to protect.

```Erb
<%= form_for @foo do |f| %>
  # ... other tags
  <%= recaptcha_tags %>
  # ... other tags
<% end %>
```

And, add `verify_recaptcha` logic to each form action that you've protected.

```Ruby
# app/controllers/users_controller.rb
@user = User.new(params[:user].permit(:name))
if verify_recaptcha(model: @user) && @user.save
  redirect_to @user
else
  render 'new'
end
```
