# Fix ng2 attributes not being highlighted like other HTML attributes correctly

## Quick Apply
Download either [HTML-ng2.sublime-package](/HTML-ng2.sublime-package) or [HTML.sublime-package](HTML.sublime-package) depending on if you want to keep it separate from stock HTML syntax or not. (Applying as HTML.sublime-package will override default HTML package.) Copy the downloaded package to `Library/Application Support/Sublime Text 3/Installed Packages`. (Tip: You can open this directory quickly by bringing up Command Palette (Cmd+Shift+P) and typing 'Preferences: Browse Packages').

Sublime Text 3 will probably show an error message about HTML syntax not being able to load, then you'll be fine. If not, try quitting the app and relaunching it.


## Manual Edit

ADD: You can also install [PackageResourceViewer](https://packagecontrol.io/packages/PackageResourceViewer) from Package Control to open up and modify the package contents directly without having to manually rename, unpackage and copy.

1. Find Sublime Text 3.app in Applications, View Package Contents
2. Open `Contents/MacOS/Packages/HTML.sublime-package` (Rename to .zip, unpackage it)
3. In `HTML.sublime-syntax` file, add following under `tag-event-attribute`:

  ```YAML
  tag-ng2-attribute:
    - match: '\s+(([\[\(][a-zA-Z0-9.:-]+[\]\)]|(\[\()[a-zA-Z0-9.:-]+(\)\])|[\*#][a-zA-Z0-9.:-]+)\s*(=)\s*)'
      captures:
        1: meta.attribute-with-value.html
        2: entity.other.attribute-name.ng2.html
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
    - match: '\s+([\[\(][a-zA-Z0-9.:-]+[\]\)]|(\[\()[a-zA-Z0-9.:-]+(\)\])|[\*#][a-zA-Z0-9._:-]+)'
      captures:
        1: entity.other.attribute-name.ng2.html
  ```

  This is basically a copy of existing `tag-generic-attribute:` with modified regexp for `match:` to target Angular 2 attributes starting with `*` or `#`, enclosed in `[]` or `()` or `[()]`.

4. At the bottom under `tag-stuff:` add:

  ```YAML
  - include: tag-ng2-attribute
  ```

5. Save the modified package as `.sublime-package` and copy to `Library/Application Support/Sublime Text 3/Installed Packages`. If you want to keep the HTML-ng2 package separate from stock HTML package, rename `HTML.sublime-syntax` file to something different like `HTML-ng2` so it won't overwrite the default one. 
 

## To add custom color to your Color Scheme
1. Open up the package for the Color Scheme you're using, find the `*.tmTheme` file.
2. Add following code inside `<array>`:

  ```XML
  <dict>
  	<key>name</key>
  	<string>Tag ng2 attribute</string>
  	<key>scope</key>
  	<string>entity.other.attribute-name.ng2</string>
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

## Result

![screenshot](http://i.imgur.com/qh8FyTD.png)
