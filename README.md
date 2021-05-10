Reference => https://www.pluralsight.com/guides/token-based-authentication-with-ruby-on-rails-5-api

rails new JWT_Auth --api
Ruby 2.6.3
Rails 6.0.3.7
rvm gemset create JWT_Auth
rvm gemset use JWT_Auth
Bundler version 1.17.2

rails g model User name email password_digest
rails db:migrate

The method has_secure_password must be added to the model to make sure the password is properly encrypted into the database: has_secure_password is part of the bcrypt gem, so we have to install it first. Add it to the gemfile.

gem 'bcrypt'

At the end
-> Run seed file

-> curl -H "Content-Type: application/json" -X POST -d '{"email":"example@mail.com","password":"123123123"}' http://localhost:3000/authenticate
  -> Response: {"auth_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJleHAiOjE0NjA2NTgxODZ9.xsSwcPC22IR71OBv6bU_OGCSyfE89DvEzWfDU0iybMA"} 

-> curl http://localhost:3000/items
  -> Response: {"error":"Not Authorized"}
  
-> curl -H "Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJleHAiOjE0NjA2NTgxODZ9.xsSwcPC22IR71OBv6bU_OGCSyfE89DvEzWfDU0iybMA" http://localhost:3000/items
  -> Response: [{"id":1,"name":"Test item 0","description":"Test description 0.","created_at":"2021-05-07T13:05:15.392Z","updated_at":"2021-05-07T13:05:15.392Z"},{"id":2,"name":"Test item 1","description":"Test description 1.","created_at":"2021-05-07T13:05:15.399Z","updated_at":"2021-05-07T13:05:15.399Z"},{"id":3,"name":"Test item 2","description":"Test description 2.","created_at":"2021-05-07T13:05:15.405Z","updated_at":"2021-05-07T13:05:15.405Z"},{"id":4,"name":"Test item 3","description":"Test description 3.","created_at":"2021-05-07T13:05:15.411Z","updated_at":"2021-05-07T13:05:15.411Z"},{"id":5,"name":"Test item 4","description":"Test description 4.","created_at":"2021-05-07T13:05:15.417Z","updated_at":"2021-05-07T13:05:15.417Z"}]


=> FLOW

1. for authentication
- submit email and password
- goes to AuthenticationController#authenticate -> AuthenticateUser#call -> JsonWebToken#encode ::::: Response: { "auth_token": value } ONLY IF CREDENTIALS ARE VALID