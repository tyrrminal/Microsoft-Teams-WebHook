# SYNOPSIS

Sample usage to post notifications using Microsoft::Teams::WebHook

# DESCRIPTION

Microsoft::Teams::WebHook

Set of helpers to send plain or AdaptiveCard notifications

# Constructor attributes

## url \[required\]

The backend `url` for your Teams webhook

## json \[optional\]

This is optional and allow you to provide an alternate JSON object
to format the output sent to post queries.

One JSON::MaybeXS with the flavor of your choice.
By default `utf8 = 0, pretty = 1`.

## auto\_detect\_utf8 \[default=true\] \[optional\]

You can provide a boolean to automatically try to detect utf8 strings
and enable the utf8 flag.

This is on by default but you can disable it by using

    my $hook = Slack::WebHook->new( ..., auto_detect_utf8 => 0 );

# Methods

## new( \[url => "https://..." \] )

This is the constructor for [Microsoft::Teams::WebHook](https://metacpan.org/pod/Microsoft%3A%3ATeams%3A%3AWebHook). You should provide the `url` for your webhook.
You should visit the [official Microsoft documentation page](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook?tabs=dotnet)
for information on how to create this URL.

## post( $message )

The [post](https://metacpan.org/pod/post) method allows you to post a single message without applying any formatting.
The return value is the return of [HTTP::Tiny::post](https://metacpan.org/pod/HTTP%3A%3ATiny%3A%3Apost) which is one `Hash Ref`.
The `success` field will be true if the status code is 2xx.

The other `post_*` methods format the message contents in [AdaptiveCards](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/connectors-using?tabs=cURL#send-adaptive-cards-using-an-incoming-webhook)
whereas this method allows you to send a simple, unformatted plaintext message 
or provide a `HashRef` structure (which will be converted to a JSON string) for
custom `AdaptiveCards`.

## post\_ok( $message, \[ @list \])

Post a message to the Teams WebHook URL. There are two methods of calling a `post_*` method.

You may either pass a simple string argument to the function

    Microsoft::Teams::WebHook->new(url => ...)->post_ok( q{posting a simple "ok" text} );

or you can pass a hash (not a hashref!) with a required `text` key

    Microsoft::Teams::WebHook->new(url => ...)->post_ok(text => q{your notification message});

Using the latter form, you may also optionally add `title` and `text_color` (for `post_ok` only!) keys
to set those additional parameters in the resulting AdaptiveCard. If `text_color` is specified,
it must be one of the allowed AdaptiveCard colors (see [documentation](https://adaptivecards.io/explorer/TextRun.html)) 
which as of this writing are: `default`, `dark`, `light`, `accent`, `good`, `warning`, and `attention`.

    Microsoft::Teams::WebHook->new(url => ...)->post_ok(
      title      => q{YOUR NOTIFICATION TITLE},
      text       => q{your notification message},
      text_color => 'light'
    );

`post_ok` defaults to `good` text color if none is given.

The return value of the `post_*` method is one [HTTP::Tiny](https://metacpan.org/pod/HTTP%3A%3ATiny) response, a `HashRef`
containing the `success` field, which is true on success.

## post\_warning( $message, \[ @list \])

Similar to [post\_ok](https://metacpan.org/pod/post_ok), but the color used to display the message is `warning` and
cannot be overridden.

## post\_info( $message, \[ @list \])

Similar to [post\_ok](https://metacpan.org/pod/post_ok), but the color used to display the message is `accent` and
cannot be overridden.

## post\_error( $message, \[ @list \])

Similar to [post\_ok](https://metacpan.org/pod/post_ok), but the color used to display the message is `attention` and
cannot be overridden.

## post\_start( $message, \[ @list \])

Similar to [post\_ok](https://metacpan.org/pod/post_ok), but the color used to display the message is `accent` and
cannot be overridden. Additionally, this method starts a timer, which is used by 
[post\_end](https://metacpan.org/pod/post_end).

## post\_warning( $message, \[ @list \])

Similar to [post\_ok](https://metacpan.org/pod/post_ok), but the default color used to display the message is `good`. 
An additional italicized, `default`-colored line is appended to the AdaptiveCard
which displays a human-readable format of the elapsed time since `post_start`
was called.

If `post_start` was not previously called (or not called since the last `post_end`)
the timer is considered to have no value and the elapsed time section will not 
be added.

# CREDITS

This module is heavily based on the excellent work done on [Slack::WebHook](https://metacpan.org/pod/Slack%3A%3AWebHook) 
(and as such intended to be largely drop-in compatible)

# AUTHOR

Mark Tyrrell <mark@tyrrminal.dev>

# LICENSE

Copyright (c) 2023

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

# DISCLAIMER OF WARRANTY

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
