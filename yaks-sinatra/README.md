Provide basic integration for using Yaks in sinatra. It gives you a top level `configure_yaks` method, and a `yaks` helper for use in routes.

This will register all media types known to Yaks, make sure the right one is picked based on the `Accept` header, and it will put the correct `Content-Type` header on the response.


``` ruby
require 'yaks-sinatra'

configure_yaks do
  # Yaks configuration as used in Yaks.new do ; ... ; end
end

class RootMapper < Yaks::Mapper
  link :self, '/'
  link :posts, '/posts'
end

class PostMapper < Yaks::Mapper
  attributes :title, :body, :date
  has_one :author
end

get '/' do
  yaks nil, mapper: RootMapper
end

get '/posts' do
  yaks Post.all
end
```
