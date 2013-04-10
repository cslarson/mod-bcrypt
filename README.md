# BCrypt

This module allows passwords to be hashed over the vertx event bus. It is using the ["bcrypt java library"](http://www.mindrot.org/projects/jBCrypt/) by Damien Miller <djm@mindrot.org> 

## Name

The module name is `mod-bcrypt`.

## Configuration

A configuration object can be passed when deploying the module. At the moment it is only possible to change the name of the address. The default address, "bcrypt", is used if no configuration object is passed. Additionally, a default [log_rounds](http://stackoverflow.com/questions/13897169/is-jbcrypts-default-log-rounds-still-appropriate-for-2013) value can be specified (otherwise 10 will be the default value).

    {
        "address": <address>,
        "log_rounds": 11
    }

## Operations

### Hash

To hash a password, send a JSON message to the address given by the main address of the busmod + `.hash`. For example if the main address is `bcrypt`, you send the message to `bcrypt.hash`.

The JSON message should have the following structure:

    {
        "password": <password>,
        "log_rounds": 9            // optionally specify here
    }
    
A reply will be returned:

    {
        "status": "ok",
        "hashed": <hashed password>   
    }

If the password is null or empty the reply will be:

    {
        "status": "error"    
    }
    
### Check

To check a password against a candidate hash, send a JSON message to the address given by the main address of the busmod + `.check`. For example if the main address is `bcrypt`, you send the message to `bcrypt.check`.

The JSON message should have the following structure:

    {
        "password": <password>,
        "hashed": <hashed password> 
    }
 
If the password matches the following reply will be returned:

    {
        "status": "ok",
        "match": true
    } 
    
Otherwise, if the password doesn't match:

    {
        "status": "ok",
        "match": false
    } 

If either the password or hash is null or empty the reply will be:

    {
        "status": "error"    
    }
    
