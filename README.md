# Language Name

This plugin provides a new scripting function `$language_name()` that allows the user to retrieve the full name for a three-character ISO 639-3 language code. The function is available to both user scripts and file naming scripts.

## Usage

The function is used as `$language_name(code)`, where `code` is the three-character language code. It would typically be used to expand the `%language%` and `%_releaselanguage%` tags.

For example, `$language_name(%language%)` will return '**English**' if the language code is '**eng**', or '**Gwich´in**' if the language code is '**gwi**', or even '**Klingon / tlhIngan-Hol**' if the code is '**tlh**'. (Yes, Klingon is actually officially recognized in the ISO 639-3 list of languages.)

If an unknown code is entered, the function will return '**unknown**'. If there is no code entered, '**missing**' will be returned.

### Tutorial

The following usage tutorial has been graciously provided by ***hiccup***:

You will need to use a script to tag your files with the full language names that this plugins makes available. A basic script to e.g. populate the 'language' tag with the full language name of the work could be:

```taggerscript
$set(language,$language_name(%language%))
```

Or if you want to write the release language, you could use:

```taggerscript
$set(release language,$language_name(%_releaselanguage%))
```

By default, when a release has no language code attached to it, the plugin will populate a tag with the text "**missing**". If for these cases you don't want a tag created at all, you could use a script like this:

```taggerscript
$set(language_temp,$language_name(%language%))
$if($in(%language_temp%,missing),,$set(language,%language_temp%))
$delete(language_temp)
```

When a release has the code "**zxx**", the plugin will write "**no linguistic content**". While technically correct, you might prefer to have "**Instrumental**" in your tags instead. Integrated with the previous example script, you then could use:

```taggerscript
$set(language_temp,$language_name(%language%))
$if($in(%language_temp%,missing),,$if($in(%language_temp%,no linguistic content),$set(language,Instrumental),$set(language,%language_temp%)))
$delete(language_temp)
```

If you are using a language other than English, you should change "no linguistic content" accordingly. E.g. for Dutch you should use:

```taggerscript
$set(language_temp,$language_name(%language%))
$if($in(%language_temp%,missing),,$if($in(%language_temp%,geen taalkundige inhoud),$set(language,Instrumentaal),$set(language,%language_temp%)))
$delete(language_temp)
```

## Supported Languages for Return Values

English is the output language used for all user interface languages not currently supported with a translation.

Supported translations of the language names from English are based on the translations entered into [Weblate](https://translations.metabrainz.org/projects/picard-plugins/#components) under the "Language Name" component of the "Picard Plugins" project. Any help correcting / completing the current translations, or translation to other languages would be greatly appreciated.

## Technical Notes

This function uses a subset of the ISO 639-3 language codes as found on the [Wikipedia](https://en.wikipedia.org/wiki/List_of_ISO_639-3_codes) website.
