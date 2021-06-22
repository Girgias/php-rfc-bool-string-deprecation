# PHP RFC: Deprecate boolean to string coercion

  * Version: 0.1
  * Date: 2021-06-22
  * Author: George Peter Banyard <girgias@php.net>, Ilija Tovilo <tovilo.ilija@gmail.com>
  * Status: Under Discussion
  * Target Version: PHP 8.1
  * Implementation: https://github.com/php/php-src/pull/9999999
  * First Published at: http://wiki.php.net/rfc/deprecate-boolean-string-coercion
  * GitHub mirror: https://github.com/Girgias/deprecate-boolean-string-coercion

## Introduction

PHP is a dynamically typed language and as such implicit type coercion naturally arises, most of these are harmless and rather convenient.
However, the conversion from boolean to string is asymmetric, as `false` gets coerced to an empty string `''` and `true` to the string `'1'`, and is a rare occurrence which most likely hints to a bug.

## Proposal

Emit an `E_DEPRECATED` deprecation diagnostic for implicit coercion of a boolean to a string.

Raise this deprecation diagnostic to a TypeError in the next major version (PHP 9.0).

## Rationale

A code audit of the impact of this change was realized back in 2015 for the "Coercive Types for Function Arguments" RFC [1] and any occurrence of this happening was a symptom of a bug rather then desirable behaviour.

PHP's boolean conversion to string is also unique, databases will convert `true`/`false` into `'1'`/`'0'`, and other programming languages if they support such a conversion usually convert it to `'true'`/`'false'`. Moreover, the fact that `false` gets converted to an empty string `''` means that as of PHP 8.0, it does not produce a valid numeric string, resulting in a broken chain of implicit coercion if the string is used in a numeric context later on.


## Backward Incompatible Changes

The following operations will now emit an E_DEPRECATED if a boolean is used:

 - Concatenation operator `.`
 - Assignment to a typed property of type `string` in coercive typing mode
 - Argument for a parameter of type `string` for both internal and userland functions in coercive typing mode
 - Returning such a value for userland functions declared with a return type of `string` in coercive typing mode
 

## Proposed PHP Version

Next minor version, i.e. PHP 8.1.

## Unaffected PHP Functionality

 - Manual casting to string will not emit a deprecation diagnostic.
 - String to boolean implicit coercions are not affected.
 - Strict type behaviour is unaffected.

## Future scope

 - Deprecating casting boolean to strings
 - Changing the output of `false` being casted to string

## Proposed Voting Choices

As per the voting RFC a yes/no vote with a 2/3 majority is needed for this proposal to be accepted.

## Patches and Tests
Links to any external patches and tests go here.

If there is no patch, make it clear who will create a patch, or whether a volunteer to help with implementation is needed.

Make it clear if the patch is intended to be the final patch, or is just a prototype.

For changes affecting the core language, you should also provide a patch for the language specification.

## Implementation
After the project is implemented, this section should contain
  - the version(s) it was merged into
  - a link to the git commit(s)
  - a link to the PHP manual entry for the feature
  - a link to the language specification section (if any)

## References

[1]: PHP RFC: Coercive Types for Function Arguments <https://wiki.php.net/rfc/coercive_sth#changes_to_internal_functions>
