---
layout: post
title: "Rails api backed with jwt"
date: 2015-06-23 21:23
comments: true
categories: [Rails]
tags: [rails, jwt]
---

Stand-alone client side applications, REST, API, JSON Web Token is all over the web. Last week I sat with one of my friend (Front-End Developer) to hack an Javascript MVC app with Rails-Api.

An awesome api, in the minds have following characterstics:

  - Excellent docs with success and failure sample response
  - A standard authorization mechanism
  - Consistent response body and codes

However, being an api developer in addition to that, I would like to have an automated tool that would help me to make sure that docs are always upto date with the api and I would like to use the latest and adopted authentication mechanism.

I would like to share my story of api development with Rails-api, Rspec, Rspec-api-documentation and JSON Web Token(JWT).

This is not a long tutorial about getting started with rails-devise-jwt, there is already lots of articles about what and why JWT and using JWT with ruby and rails. This is a blueprint of an end-to-end solution I adopted for development of an api.

The sample app is available [here at github](https://github.com/RohitRox/Rails-API-and-JWT).

The [commit](https://github.com/RohitRox/Rails-API-and-JWT/commits/master) in the speaks itself for steps that were done.

<!-- more -->

The first seven commits are all about setting up an rails-api app, getting required gems, creating CRUD resource, devise user installation. I used jbuilder for building json responses and kaminari for pagination.

I used Rspec and Rspec-api-documentation for testing. Setting up Rspec-api-documentation is quite straight forward and I didn't feel any difficulty in folowing their readme. I really like this gem, works very well, the dsl it provides is decent, the page it generates looks good and it can generate in various formats. The strategy I took, was, to have controller and model specs as it would be, but put the api-documentation specs in a special folder 'specs/acceptance'. The api-documentation specs would assert only for response codes and have examples for requests with valid as well invalid params. This way, my usual controller and model specs make sure the api are up and good and api-documentation will make sure that the docs have the up-to-date params, request body and response body specifications.

To make the response body consistent, I have followed following schema:

``` js
// POST /api/posts
{
  "data": {
    "id": 1,
    "title": "My Title",
    "content": "Some long long awesome content."
  },
  "response": {
    "code": 201
  },
  "links": {
    "self": "http://example.org/api/posts/1"
  }
}
```

``` js
// GET /api/posts
{
  "data": [
    {
      "id": 2,
      "title": "Vel sit in molestias iste voluptas nam ad.",
      "content": "Facilis rem quia quas repudiandae non. Dignissimos dolores rerum aperiam inventore non doloremque laborum. Ex aut autem deserunt molestiae quae repellendus consequatur."
    },
    {
      "id": 1,
      "title": "Aliquid atque distinctio ab quaerat qui adipisci.",
      "content": "Facilis rem quia quas repudiandae non. Dignissimos dolores rerum aperiam inventore non doloremque laborum. Ex aut autem deserunt molestiae quae repellendus consequatur."
    }
  ],
  "meta": {
    "current_page": 1,
    "next_page": null,
    "prev_page": null,
    "total_pages": 1,
    "total_count": 2,
    "sort": "created_at",
    "order": "desc"
  },
  "response": {
    "code": 200
  }
}
```

I would not say this is the standard json response structure for a REST Api, but my front-end developer friend found it pretty neat.

Finally, devise, I do not have any specific reason why I was using devise, I mean the solution I was doing could be done without devise but somewhere in the future a couple of devise modules may be useful. I have a [commit](https://github.com/RohitRox/Rails-API-and-JWT/commit/10a17cb0c5a8e24015a5f2f77607b0e390f6acca) for using devise and overiding devise controller for signing-in and signing-up users. At that time, I looked at different approaches and decided to go with that. Basically we hijack devise controller actions for signing-up and signing-in, this is necessary because later we need to generate and inject auth-token there.

Next step is to use 'jwt' gem to generate auth-tokens and decode the tokens to extract claims and verify its authenticty.
This [article](http://nebulab.it/blog/authentication-with-rails-jwt-and-react) describes really well. The steps are pretty straight forward, we generate JSON Web token and send them in response at sign-in or sign-up actions, client requests resource with token set in Authorization headers, we decode the token and extract the information we had set. The commits [here](https://github.com/RohitRox/Rails-API-and-JWT/commit/548a1268c943a8895d3f67ad33358150e45449ec) and [this](https://github.com/RohitRox/Rails-API-and-JWT/commit/a7714cf68f62e4b0342b155bc3308ca20fa2ae2b) is where I have integrated the JWT authentication mechanisms.

This is what the code looks like:

``` ruby
#lib/json_web_token.rb
require 'jwt'

class JsonWebToken
  def self.encode(payload, expiration = 24.hours.from_now)
    payload = payload.dup
    payload['exp'] = expiration.to_i
    JWT.encode(payload, Rails.application.secrets.json_web_token_secret)
  end

  def self.decode(token)
    JWT.decode(token, Rails.application.secrets.json_web_token_secret)
  end
end
```

``` ruby
#app/controllers/api/base_controller.rb
class Api::BaseController < ApplicationController
include ActionController::ImplicitRender
respond_to :json

before_filter :authenticate_user_from_token!

protected

  def authenticate_user_from_token!
    if claims and user = User.find_by(email: claims[0]['user'])
      @current_user = user
    else
      return render_unauthorized errors: { unauthorized: ["You are not authorized perform this action."] }
    end
  end

  def claims
    auth_header = request.headers['Authorization'] and
      token = auth_header.split(' ').last and
      ::JsonWebToken.decode(token)
  rescue
    nil
  end

  def jwt_token user
    JsonWebToken.encode('user' => user.email)
  end

  def render_unauthorized(payload)
    render json: payload.merge(response: { code: 401 })
  end

end
```

I have the sample app running at heroku and docs at [https://rails-api-jwt.herokuapp.com/api/docs/](https://rails-api-jwt.herokuapp.com/api/docs/).

I have drawn a lot of inspiration and some code from following blog posts to create this.

  - [http://nebulab.it/blog/authentication-with-rails-jwt-and-react](http://nebulab.it/blog/authentication-with-rails-jwt-and-react)
  - [http://adamalbrecht.com/2014/12/04/add-json-web-token-authentication-to-your-angular-rails-app/](http://adamalbrecht.com/2014/12/04/add-json-web-token-authentication-to-your-angular-rails-app/)
  - [http://blog.moove-it.com/token-based-authentication-json-web-tokenjwt/](http://blog.moove-it.com/token-based-authentication-json-web-tokenjwt/)
  - [http://zacstewart.com/2015/05/14/using-json-web-tokens-to-authenticate-javascript-front-ends-on-rails.html](http://zacstewart.com/2015/05/14/using-json-web-tokens-to-authenticate-javascript-front-ends-on-rails.html)

Note that there is no error handling when decoding the token in the sample app. These as well as other algorithm information are well described in the ['ruby-jwt' gem github page](https://github.com/progrium/ruby-jwt).
