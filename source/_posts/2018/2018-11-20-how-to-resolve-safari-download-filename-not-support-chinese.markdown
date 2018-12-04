---
layout: post
title: "How to resolve Safari download filename not support Chinese"
date: 2018-11-20T13:58:28+08:00
comments: true
external-url:
categories:
---

Found from [stackoverflow](https://stackoverflow.com/questions/51482811/rails-send-file-filename-utf-8-broken-in-internet-explorer), which I think should including in send_data acturally....


```ruby
def make_and_send_pdf(pdf_name, options = {})
  options = { :disposition => 'attachment' }.merge(options)
  file_name = "#{pdf_name}.pdf"
  send_data(
    make_pdf(options),
    filename: ERB::Util.url_encode(file_name),
    type: 'application/pdf',
    disposition: "#{options[:disposition]}; filename*= UTF-8''#{ERB::Util.url_encode(file_name)}"
  )
end
```
