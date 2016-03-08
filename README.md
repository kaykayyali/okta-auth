# Okta-auth

Okta Authentication for ExpressJS (NodeJS).

## Example

Simple usage:

    const cookieParser = require('cookie-parser');
    const bodyParser = require('body-parser');
    const session = require('express-session');
    const oktaAuth = require('okta-auth');

    app.use(cookieParser());
    app.use(bodyParser());
    app.use(session());

    oktaAuth.initialize(app, {
      oktaIssuer: process.env.OKTA_ISSUER,          // required
      oktaEntryPoint: process.env.OKTA_ENTRY_POINT, // required
      oktaCert: process.env.OKTA_CERT               // required
    });

    app.get('/my-secured-url', oktaAuth.secured, (req, res) => {
      res.render('index');
    });

You can create own page for not logged users:

    // ...

    oktaAuth.initialize(app, {
      // ...
      accessDeniedUrl: accessDeniedUrl, // default: /access-denied
      defaultAccessDeniedPage: false    // default: true
    });

    app.get(accessDeniedUrl, (req, res) => {
      res.render('access-denied');
    });

    // ...

You can configure Okta to pass some information about users.
You have access to it in `req.session.passport.user`

    // ...

    oktaAuth.initialize(app, {
        // ...
        oktaFields: ['email', 'position', 'department']; // default: ['email', 'firstName', 'lastName']
    ));

    app.get('/my-secured-url', oktaAuth.secured, (req, res) => {
      res.render('index', {user: req.session.passport.user});
    });

    // ...
