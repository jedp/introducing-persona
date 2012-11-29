## Mozilla Persona
### For Good and For Awesome

---------------------------------------------------

**Jed Parsons**, Engineer, Mozilla

- `jparsons@mozilla.com`
- `@drainmice`

DevCon5, San Francisco, 29 Nov 2012

**Presentation Source**: [https://github.com/jedp/introducing-persona](https://github.com/jedp/introducing-persona)

## tl;dr

**Mozilla Persona** is a decentralized and secure authentication
system for the web that simplifies the problem of authentication for
users as well as for developers.

This presentation will discuss the problems in security and privacy
that Persona seeks to remedy, describe how the system works, and show
how you can use it to make your login experience easier for your users
and simpler for you.

## Goals

- Make Identity Simple
- Make Identity Unbroken

Or: 

- Make Identity Awesome
- Make Identity Good

## Identity Today

1. Go to site
2. **Click login**
3. You have to register, first
4. Choose a unique username
5. Try to make up a password, dealing with random bulls**t rules
6. Suppress the urge to smash something
7. Security Questions!!?? WTF???
8. **Enter an email address**
9. **Retrieve email to complete registration**
10. Go back to site and login

## Forgot Your Password?

- Return to the site
- **Identify Yourself by your Email Address**
- **Retrieve your email to reset your password**

## What Inevitably Happens

- People use the same crummy passwords
- People have to reset their passwords all the time
  (this should tell us that there's something broken about passwords)
- People open new accounts and let the old ones rot
- Memorizable vs Safe passwords - conflicting instructions from "experts"
- People use "1Password" solutions
- People just give up.  Leave the door unlocked and hope for the best.
- Security gets worse, time gets wasted, kittens die

## &lt;Facepalm>

![passwords](img/passwordscloud.png)

Source: [http://xato.net/passwords/more-top-worst-passwords/](http://xato.net/passwords/more-top-worst-passwords/)

## What About Third-Party Logins?

- Centralized authority
- One stop

Sounds good!

- You overshare your life.  They track everything you do.
- What if service goes down?  Site becomes irrelevant?
- Lock-in and brittleness for developers

Sounds not so good :-/

## Logins Reconsidered Pragmatically

On login or signup:

- Provide an email address

To complete signup:

- Retrieve email and click the link

To reset your password:

- Retrieve email and click the link

*Hmm...*

## Mozilla Persona Flow

First time flow:

1. Click login
2. **Enter an email address**
3. **Authenticate with your email provider**
4. Done

Second time flow:

1. Click login, if you're not automatically logged in
2. **Select an email address**
3. Done

## Live Demo Time

![live demo](img/montparnasse.400.jpg)

## Test Drive

- http://crossword.thetimes.co.uk/

- https://www.voo.st/events

- http://myfavoritebeer.org/

- http://123done.org/

## Why Email Addresses

- People know them
- They make sense: name@place
- Natural for capturing contexts: me@work, me@home, etc.
- Not limited to a single identity
- Provide developers a direct means of contacting users
- Most sites want email anyway; one less step for signup

## Benefits for Developers

- There are **no passwords**
- Storing passwords is a liability
- Conversion rate
- No lock-in; Email addresses are universal

## Testimony from voo.st

> You can't run a FB-only login system without alienating a
   significant chunk of most audiences. ... Now that Persona and FB
   auth are on equal footings, we have yet to have anyone complain
   about the signup process.

Source: [http://news.ycombinator.com/item?id=4599881](http://news.ycombinator.com/item?id=4599881)

## Ease of Implementation

> A complete Persona-based auth system is a question of hours, not days.

Source: [http://news.ycombinator.com/item?id=4232505](http://news.ycombinator.com/item?id=4232505)

## Ease of Implementation

> Done!

Phone call with: [http://everfi.com/](http://everfi.com/) and Anson County School District

## Ease of Implementation

> That was actually simpler than I thought it would be.

Sean Moffatt of [Joomla!](http://www.joomla.org/), writing a Persona plugin for the Joomla! CMS.

## Summary of Key Properties

1. Email is identity.

     Users identify.  No new infrastructure.  No lock-in

2. Decentralized

     Privacy-protecting.  Anyone can play.  IdPs authenticate as they like.

3. Ownership-based authority

     No passwords.

4. Forward compatible

     Until native support, bootstrapped with html5 shim and Mozilla Persona IdP fallback.

## How Does It Work?

A Walk Through The Protocol with Joan, Francine, and Fluffball.

For more details:

- [https://developer.mozilla.org/en-US/docs/Persona/Protocol_Overview](https://developer.mozilla.org/en-US/docs/Persona/Protocol_Overview)

- [http://lloyd.io/how-browserid-works](http://lloyd.io/how-browserid-works)

If this makes your eyes glaze over, until the next three slides are passed, you can visit:

[http://ponyplace.ajf.me](http://ponyplace.ajf.me)

## Actors in our Little Drama

- **Francine** is our **User**

- **Fluffball** runs the **Identity Provider**, `fluffball.com`

- **Joan** runs `HatsByJoan.com`, the **Relying Party**

Plot: Francine wants to login to `HatsByJoan.com` as `francine@fluffball.com`.

If she can convince Joan of her identity, Joan will let her buy hats.

## 1. Identity Provisioning

1. Francine wants to login to RP `HatsByJoan.com` as `francine@fluffball.com`
2. Francine must prove she is actually `francine@fluffball.com`
3. Francine goes to her IdP, `fluffball.com`
4. IdP asks Francine to authenticate
5. IdP then asks Francine to generate a public and private key
6. Francine sends the public key to the IdP
7. IdP makes a JWT with email address, public key, and expiry date
8. IdP signs the JWT with its own private key
9. IdP returns this **signed certificate** to Francine
10. Francine stores this as proof of her `francine@fluffball.com` identity
11. Francine stores the **private key** she generated along with the certificate

## 2. Assertion Generation

1. Francine makes a JWT containing her email, Joan's address, and a short expiry
2. Francine signs this JWT with her private key
3. This is a **signed assertion** of identity
4. She pins a copy of her `fluffball.com` certificate to her assertion
5. This is a **backed assertion** (backed by the certificate)
6. Francine hands the backed assertion to Joan

## 3. Assertion Verification

1. Joan transmits the assertion down to her servers for handling
2. On the server, she checks both expiry dates
3. Joan extracts the hostname from Fluffball's email address (`fluffball.com`)
4. Joan asks IdP `fluffball.com` for its public key
5. Joan uses the key to verify the signature on the certificate
6. With Francine's public key from the cert, Joan verifies the assertion signature
7. She now knows that Francine is indeed `francine@fluffball.com`
8. She opens the doors to her shop and offers to sell Francine a hat 

## Step 1: Include JS

In your web page:

    <script src="https://login.persona.org/include.js"></script>

We recommend you not host the `include.js` file yourself.

We're still in Beta, and things might still change.

## Step 2: Call `watch`

    navigator.id.watch({
      loggedInUser: #{ theUserYouBelieveIsLoggedIn },

      onlogin: function(assertion) {
        $.post('/login',
         {assertion: assertion},
         function(data) { window.location = '/home' });
      },

      onlogout: function() {
        $.post('/logout',
          function() { window.location = '/logout' });
      }
    });

## Step 3: Hook it up

    $('#login').click(function() {
      navigator.id.request();
    });

    $('#logout').click(function() {
      navigator.id.logout();
    });

## Step 4: Verify assertions

    var body = qs.stringify({
      assertion: assertion,
      audience: yourSite
    });
    var options = {
      uri: 'https://verifier.login.persona.org/verify',
      headers: {
        'content-type': 'application/x-www-form-urlencoded',
        'content-length': body.length
      }
    };
    request.post(options, function(err, res, body) {
      if (err) return  callback(err);
      return callback(null, JSON.parse(body));
    });

## Links for You

### Persona

https://developer.mozilla.org/en-US/docs/Persona

https://developer.mozilla.org/en-US/docs/Persona/Libraries_and_plugins

https://developer.mozilla.org/en-US/docs/persona/branding

https://wiki.mozilla.org/Identity/MobileSDK

### FirefoxOS

https://wiki.mozilla.org/B2G

https://hacks.mozilla.org/2012/10/r2d2b2g-an-experimental-prototype-firefox-os-test-environment/
