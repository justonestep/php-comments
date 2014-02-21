Comments Presentation
=====================

Given an [h-entry](http://indiewebcamp.com/h-entry), returns author info as well as truncated post text suitable for display.

Installation
------------

Install via composer:

```json
{
  "indieweb/comments": "0.1.*",
}
```

Or just require the one file:

```php
require_once('src/indieweb/comments.php');
```


Usage
-----

The function accepts a PHP array in the format returned by the [microformats2 parser](https://github.com/indieweb/php-mf2)
and returns a new array that looks like the following:

Original HTML:
```html
<div class="h-entry">
  <div class="p-author h-card">
    <img src="http://aaronparecki.com/images/aaronpk.png" class="u-photo">
    <a href="http://aaronparecki.com">Aaron Parecki</a>
  </div>
  <h3 class="p-name">Example Note</h3>
  <p class="e-content">this text is displayed as the comment</p>
  <time class="dt-published" datetime="2014-02-16T18:48:17-0800">Feb 16, 6:48pm</time>
</div>
```

Parsed Microformats:
```json
{
    "type": [
        "h-entry"
    ],
    "properties": {
        "author": [
            {
                "type": [
                    "h-card"
                ],
                "properties": {
                    "photo": [
                        "http:\/\/aaronparecki.com\/images\/aaronpk.png"
                    ],
                    "name": [
                        "Aaron Parecki"
                    ],
                    "url": [
                        "http:\/\/aaronparecki.com"
                    ]
                },
                "value": "Aaron Parecki"
            }
        ],
        "name": [
            "Example Note"
        ],
        "published": [
            "2014-02-16T18:48:17-0800"
        ],
        "content": [
            {
                "html": "this text is displayed as the comment",
                "value": "this text is displayed as the comment"
            }
        ]
    }
}
```

Parse for comment display:

```php
$result = IndieWeb\comments\parse($input, $maxLength);
```

Resulting PHP array:

```php
  $result = array(
    'author' => array(
      'name' => 'Aaron Parecki',
      'photo' => 'http://aaronparecki.com/images/aaronpk.png',
      'url' => 'http://aaronparecki.com/'
    ),
    'published' => '2014-02-16T18:48:17-0800',
    'text' => 'this text is displayed as the comment',
    'url' => 'http://aaronparecki.com/post/1'
  )
```

This function will return an array with all of the keys above. One or more values may 
be empty depending on what information was available in the post, such as author name/photo.

The `text` property will always be within your maximum desired length as passed to the `parse()` function.

The function follows the algorithm described at [comments-presentation](http://indiewebcamp.com/comments-presentation#How_to_display)
for deciding whether to show the `p-name`, `p-summary` or `e-content` properties and truncating appropriately.



Tests
-----

Please see the [tests](tests/BasicTest.php) for more complete examples of parsing different posts.


License
-------

Copyright 2014 by Aaron Parecki

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
