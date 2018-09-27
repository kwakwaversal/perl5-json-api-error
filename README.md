# NAME

JSON::API::Error - JSON API-style error objects

# SYNOPSIS

    use JSON::API::Error;
    use Mojo::JSON qw/encode_json/;

    # A JSON API error representing bad submission data
    my $err = JSON::API::Error->new({
        source => {pointer => '/forename'},
        status => '400',
        title  => 'Field required',
    });

    # Field required
    say $err->title;
    # /forename: Field required
    say "$err";

    # {
    #   "source": {
    #     "pointer": "/forename"
    #   },
    #   "status": "400",
    #   "title": "Field required"
    # }
    say encode_json $err;
    say encode_json $err->TO_JSON;

    # A JSON API error representing a missing resource
    my $err = JSON::API::Error->new({
        status => '404',
        title  => 'Not Found',
    });

    # {
    #   "status": "404",
    #   "title": "Not Found"
    # }
    say encode_json $err;
    say encode_json $err->TO_JSON;

# DESCRIPTION

[JSON::API::Error](https://metacpan.org/pod/JSON::API::Error) provides a [JSON API error object](http://jsonapi.org/format/#error-objects).
It is intended to provide a consistent error interface that can be digested by
front and backend software.

The front end will receive an `ARRAY` of these objects when there is an error.
It should contain enough information to be able to add custom errors to specific
form elements.

# ATTRIBUTES

[JSON::API::Error](https://metacpan.org/pod/JSON::API::Error) implements the following attributes.

## code

An application-specific error code, expressed as a string value.

## detail

A human-readable explanation specific to this occurrence of the problem. Like
`title`, this field's value can be localized.

## id

A unique identifier for this particular occurrence of the problem.

## links

A [links object](http://jsonapi.org/format/#document-links) containing the
following members:

**about**: a link that leads to further details about this particular occurrence
of the problem.

## meta

    my $err = JSON::API::Error->new(
        {
            meta => {
                length => 5,
                detail => "Field length is 5, should be at least 30"
            },
            source => {pointer => "/forename"},
            status => '400',
            title  => "Field length",
        }
    );

A meta object containing non-standard meta-information about the error. Can be
used to include more detail about the error.

## source

An object containing references to the source of the error, optionally including
any of the following members:

**pointer**: a JSON Pointer \[RFC6901\] to the associated entity in the request
document \[e.g. "/data" for a primary data object, or "/data/attributes/title"
for a specific attribute\].

**parameter**: a string indicating which URI query parameter caused the error.

## status

The HTTP status code applicable to this problem, expressed as a string value.

## title

A short, human-readable summary of the problem that SHOULD NOT change from
occurrence to occurrence of the problem, except for purposes of localization.

# METHODS

[JSON::API::Error](https://metacpan.org/pod/JSON::API::Error) implements the following methods.

## TO\_JSON

Returns the object instance as a `HASH` reference, suitable for encoding to
`JSON`.

# OPERATORS

[JSON::API::Error](https://metacpan.org/pod/JSON::API::Error) overloads the following operators.

## bool

    my $bool = !!$err;

Always true.

## stringify

    my $str = "$err";

Alias for `to_string`.

# AUTHOR

Paul Williams <kwakwa@cpan.org>

# COPYRIGHT

Copyright 2018- Paul Williams

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO

[https://metacpan.org/pod/Mojolicious::Plugin::OpenAPI](https://metacpan.org/pod/Mojolicious::Plugin::OpenAPI),
[http://jsonapi.org/](http://jsonapi.org/),
[http://jsonapi.org/format/#errors](http://jsonapi.org/format/#errors).
