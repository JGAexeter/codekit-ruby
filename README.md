# Att::Codekit

Library for easy access to AT&T's cloud services.

## Requirements

Ruby 1.9.3 or higher (Ruby 2.1 recommended)

## Installation
### Easy install

Add the server host of the gem file:

    $ gem sources --add https://lprod.code-api-att.com/gemserver/

Install the gem:

    $ gem install att-codekit

### Manual install
Compile a gem using:

    $ rake build

Then install it locally:

    $ gem install -l pkg/att-codekit-version.gem

You can install per user by issuing:

    $ gem install --user-install -l pkg/att-codekit-version.gem

Substituting version for the compiled version to install

## Documentation

The Codekit contains inline documentation, which can be generated using
yard doc via the yard command. Install by running

    $ gem install yard

For example, to generate the documentation which can then be viewed via browser
run:
  
    $ yard server

Then goto the specified address in your preferred web browser, usually:

    http://localhost:8808

## Usage

###Require the library
    
    require 'att/codekit'

###Include the namespace (optional)
    
    include Att::Codekit::Auth
    include Att::Codekit::Service

###Create oauth service object

You will need to use AuthCode or ClientCred according to which api you wish to use.
These immutable objects will perform all steps required for using oauth services.

    # Create an authcode oauth service 
    authcode = AuthCode.new(fqdn, client_id, client_secret)

    # Create a client credential oauth service
    client = ClientCred.new(fqdn, client_id, client_secret)

###Create token object

Now that we have an oauth service object we can use it to generate a token.

    # Authorization code must first redirect the user to grant authorization
    redirect authcode.consentFlow(:redirect => 'http://localhost/auth')
    
    # After consent flow the user will be redirected to the specified url with CODE
    authToken = authcode.createToken(CODE)
    
    # Generate a client credential token with SCOPE
    clientToken = client.createToken(SCOPE)

###Create api service 

Now to create an api service we just pass the oauth token to the api we want to use:
    
    immn = IMMNService.new(fqdn, authToken)
    sms = SMSService.new(fqdn, clientToken)

