---
layout: post
title: "Postback other input controls value in rails jquery-ujs data-remote request"
date: 2014-04-27T23:30:34+08:00
comments: true
external-url:
categories: [Rails, Javascript]
---

[The *Unobtrusive JavaScript* feature introduce from Rails 3.1](http://yacodeblog.wordpress.com/2011/07/06/how-ajax-works-in-rails-3-1-i-think-revised/) and the [Rails Guide](http://guides.rubyonrails.org/working_with_javascript_in_rails.html#unobtrusive-javascript) have a whole chapter to say about how to use, but the [jquery-ujs](https://github.com/rails/jquery-ujs) much more feature not talked in official Rails Guide.

[Except the AJAX forms and some link_to feature](http://robots.thoughtbot.com/a-tour-of-rails-jquery-ujs), one of my favorite features is AJAX HTTP request back to server when one of the forms input control content changed by user and lost focus. Such feature is very similar to [ASP.NET AutoPostBack](http://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.textbox.autopostback(v=vs.110).aspx), which is very handy when you need user fill some content first before you can help them auto fill the rest.

<!--more-->

The rails_ujs is simple to use, mainly three steps:

### Declare text_field as remote.

```erb
<%= f.text_field :order_id, class: 'form-control', 'data-remote' => true, \
	'data-type' => 'script', 'data-url' => url_for(controller: 'order', action: 'fetch_order_detail', format: 'js') %>
```

### Prepare the data in Rails server side

```ruby
def fetch_order_detail
  @order = Order.where(:order_id => params[:order][:order_id]).first
  respond_to do |format|
    if @order.present?
      format.js { render :layout => false }
    else
      format.js { render :layout => false, :status => 406  }
    end
  end
end
```

### Render the javascript.

```erb
$('input[id$="client_name"]').val("<%= @order.client.name -%>");
$('input[id$="place_date"]').val("<%= @order.place_date -%>");
```

Although it's very easy, but some time, you need additional field value to do the data preparation, but the jquery-ujs only can send back the [triggered control value to server](https://github.com/rails/jquery-ujs/blob/master/src/rails.js#L115), then you have to add the custom events for data-remote request, here is how:

### Attach additional

```javascript
$('#order_id').on('ajax:beforeSend', function(event, xhr, settings) {
  settings.url += '&unit_id=' + $('#unit_unit_id').val();
});
```

Then you can, from controller get additional field value via `params[:unit_id]`.

The hack method not elegant as original jquery-ujs, so if you have more good methods, just comments.
