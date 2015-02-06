# NAME

Dancer::Plugin::UUID - Maintain unique identifiers for each visitor of the application.

# VERSION

version 0.004

# SYNOPSIS

    use Dancer;
    use Dancer::Plugin::UUID;
      
    get ‘/someroute’ => sub {
       my $uid = uuid;
    };

# DESCRIPTION 

This plugin takes care of dropping a cookie with a unique user identifier on
each visitor of the web application. The ID follows the
[UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier) spec and is
generated randomly on the second visit of the user.

Identifiers are generated with [Data::UUID](https://metacpan.org/pod/Data::UUID).

The very first visit is used to drop a _test cookie_ to see if the client
accepts cookies. If not, then no cookie will be droped and the UUID for that
client will be `undef`.

This plugin is useful if you wish to track your users on your application. 

This plugin honors the _Do not track_ policy and won’t drop any cookie when
this option is enabled in the client’s browser.

# KEYWORDS

The plugin exports the following keywords.

## `uuid`

Returns the value of the UUID for the current user. If the browser refuses
cookies, or has the Do Not Track setting enabled, returns undef.

The cookie droped will expire in 3 years (which is almost the end of time for a
device in the Internet world).

# HOOKS

When the plugin is loaded, it declares a `before` hook that will be responsible
for dropping the UUID cookie. As it's impossible to know if the browser accepts
cookies without droping a cookie first, a test cookie is used on the first
visit.

If on the second visit, the test cookie is sent back by the browser, with a
legitimate value, it is assumed that the browser accepts cookies. The real UUID
cookie is then droped.

That means that the very first visits of the users won't be tracked (all users
are identified as "undef" users on their first visit).

# AUTHOR

Alexis Sukrieh <sukria@gmail.com>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2015 by Alexis Sukrieh.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
