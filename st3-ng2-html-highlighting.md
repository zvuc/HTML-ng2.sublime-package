## Fix ng2 attributes not being highlighted like other HTML attributes correctly
1. Find Sublime Text 3.app in Applications, View Package Contents
2. Open `Contents/MacOS/Packages/HTML.sublime-package` (Rename to .zip, unpackage it)
3. In `HTML.sublime-syntax` file, add following under `tag-event-attribute`:

  ```YAML
  tag-ng-attribute:
      - match: '\s+(([\[\(][a-zA-Z0-9:-]+[\]\)]|[\*][a-zA-Z0-9:-]+)\s*(=)\s*)'
        captures:
          1: meta.attribute-with-value.html
          2: entity.other.attribute-name.ng.html
          3: punctuation.separator.key-value.html
        push:
          - match: '"'
            scope: punctuation.definition.string.begin.html
            set:
              - meta_scope: meta.attribute-with-value.html string.quoted.double.html
              - match: '"'
                scope: punctuation.definition.string.end.html
                pop: true
              - include: entities
          - match: "'"
            scope: punctuation.definition.string.begin.html
            set:
              - meta_scope: meta.attribute-with-value.html string.quoted.single.html
              - match: "'"
                scope: punctuation.definition.string.end.html
                pop: true
              - include: entities
          - match: '(?:[^\s<>/''"]|/(?!>))+'
            scope: meta.attribute-with-value.html string.unquoted.html
          - match: ''
            pop: true
      - match: '\s+([\[\(][a-zA-Z0-9:-]+[\]\)]|[\*][a-zA-Z0-9:-]+)'
        captures:
          1: entity.other.attribute-name.ng.html
  ```

  This is basically a copy of existing `tag-generic-attribute:` with modified regexp for `match:` to target Angular 2 attributes starting with `*`, enclosed in `[]` or `()`.

4. At the bottom under `tag-stuff:` add:

  ```
  - include: tag-ng-attribute
  ```

5. Save the modified package as `.sublime-package` and replace the original file. You can also rename files to something else if you want to keep the original HTML.sublime-package and have a separate package like HTML-ng2.sublime-package.
 

## To add custom color to your Color Scheme
1. Open up the package for the Color Scheme you're using, find the `*.tmTheme` file.
2. Add following code inside `<array>`:

  ```
  <dict>
  	<key>name</key>
  	<string>Tag ng attribute</string>
  	<key>scope</key>
  	<string>entity.other.attribute-name.ng</string>
  	<key>settings</key>
  	<dict>
  		<key>fontStyle</key>
  		<string></string>
  		<key>foreground</key>
  		<string>#C99E00</string>
  	</dict>
  </dict>
  ```
  Replace the color HEX code inside the last `<string>` to one of the theme colors in your color scheme.
3. Save and replace existing file.