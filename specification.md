# PMF semantic markup specification (draft)

## Introduction

PMF semantic markup is a set of HTML class names and hidden elements that make
web pages easier for password managers to parse and interact with. PMF semantic
class names designate the meaning and purpose of human-readable forms and other
elements, while a small number of additional hidden elements supply useful
information that is not normally be available to a password manager.

The PMF standard specifies a machine-readable format for password policies to
allow password managers to generate strong, policy-compliant passwords for
users.

PMF markup only uses existing standards-compliant features of HTML. This makes
it easy for both website authors and password manager developers to adopt.

PMF was first presented at [Passwords 2014][101] and has a website at
[pmfriendly.org][102]. PMF stands for Password-Manager Friendly.

## Status of this document

This document is a draft and all aspects are subject to change.

## Glossary

* "password form" - A `form` element which contains one or more `password`-type
  `input` element where the user types a new or existing password.

## Semantic classes

### Form inputs

#### Username inputs

An `input` element where the user would enter their username must have the
class `pmf-username`.

**Code example:**

	<p>
		Username:
		<input type="text" name="username" class="pmf-username" />
    </p>

A password form which relates to a specific account and username, such as a
password-reset form, must contain a `hidden`-type `input` marked with the
`pmf-username` class and with its `value` attribute set to the username of the
account for the password is being changed or reset.

**Code example:**

	<input type="hidden" class="pmf-username" value="jimbojones" />

**Note:** The `name` of the hidden input should not be set to avoid
unnecessarily sending the username back to the server when the form is
submitted.

#### Password inputs

An `input` element where the user would enter an existing password must have
the class `pmf-password`.

**Code example:**

	<p>
		Password:
		<input type="password" name="password" class="pmf-password" />
	</p>

An `input` element where the user would enter a new password must have the
class `pmf-new-password`.

**Code example:**

	<p>
		New password:
		<input type="password" name="new" class="pmf-new-password" />
	</p>
	<p>
		Confirm new password:
		<input type="password" name="confirm" class="pmf-new-password" />
	</p>

**Note:** If a form contains multiple inputs where the user would enter the
  same new password, all of them must have the class `pmf-new-password`.

#### Stay-signed-in inputs

A `checkbox`-type `input` element which the user would check to stay signed in
for a longer amount of time should have the `pmf-stay-signed-in` class.

**Code example:**

	<p>
		Stay signed in?
		<input type="checkbox" name="persist" class="pmf-stay-signed-in" />
	</p>

### Forms

#### Login forms

A password form where the user would enter their username and password to login
to their account must have the class `pmf-login`.

**Code example:**

	<form action="/login" method="POST" class="pmf-login">
		<p>
			Username:
			<input type="text" name="username" class="pmf-username" />
		</p>
		<p>
			Password:
			<input type="password" name="password" class="pmf-password" />
		</p>
		<p>
			<input type="submit" value="Login" />
		</p>
	</form>

A `pmf-login` form must contain exactly one `pmf-username` input and zero or
one `pmf-password` inputs. A `pmf-login` form may contain up to one
`pmf-stay-signed-in` checkbox.

Examples of login forms with no `pmf-password` input include:

* Login forms which are part of a multi-page login process.

	**Code example:**

		<form action="/login" method="POST" class="pmf-login">
			<p>
				Username:
				<input type="text" name="username" class="pmf-username" />
			</p>
			<p>
				<input type="submit" value="Next" />
			</p>
		</form>

	**Note:** Any `pmf-login` form on a subsequent page must still include
	exactly one `hidden`-type input with the `pmf-username` class and its
	`value` attribute set to the username (entered on a previous page).
  
* Login forms which require a one-time code instead of a password.

	**Code example:**

		<form action="/login" method="POST" class="pmf-login">
			<p>
				Username:
				<input type="text" name="username" class="pmf-username" />
			</p>
			<p>
				Security code:
				<input type="password" name="code" />
			</p>
			<p>
				<input type="submit" value="Login" />
			</p>
		</form>

#### Registration forms

A password form where the user would enter a username, a new password and other
details required to register a new account must have the class `pmf-register`.

**Code example:**

	<form action="/register" method="POST" class="pmf-register">
		<p>
			Username:
			<input type="text" name="username" class="pmf-username" />
		</p>
		<p>
			Email address:
			<input type="email" name="email" />
		</p>
		<p>
			New password:
			<input type="password" name="new" class="pmf-new-password" />
		</p>
		<p>
			Confirm new password:
			<input type="password" name="confirm" class="pmf-new-password" />
		</p>
		<p>
			<input type="submit" value="Register" />
		</p>
	</form>

A `pmf-register` form must contain exactly one `pmf-username` input and one or
more `pmf-new-password` inputs. A `pmf-register` form may contain up to one
`pmf-stay-signed-in` checkbox and up to one `pmf-policy` (see below).

**Note:** Unlike login forms, registration forms may not contain zero password
  inputs. In the case of a multi-page registration process in which the inputs
  where the user would enter their username and new password appear on
  different pages, only the form where they would enter their new password
  should have the `pmf-register` class and that form must contain exactly one
  `hidden`-type input with the `pmf-username` class and its `value` attribute
  set to the new username (entered on a previous page).

#### Password-reset forms

A password form where the user would enter a new password for an existing
account after receiving a password-reset email must have the class
`pmf-reset-password`.

**Code example:**

	<form action="/reset" method="POST" class="pmf-reset-password">
		<input type="hidden" class="pmf-username" value="jimbojones" />
		<p>
			New password:
			<input type="password" name="new" class="pmf-new-password" />
		</p>
		<p>
			Confirm new password:
			<input type="password" name="confirm" class="pmf-new-password" />
		</p>
		<p>
			<input type="submit" value="Reset password" />
		</p>
	</form>

#### Change-password forms

A password form where the user would enter a new password for an existing
account, for which they have the current password must have the class
`pmf-change-password`.

**Code example:**

	<form action="/change" method="POST" class="pmf-change-password">
		<input type="hidden" class="pmf-username" value="jimbojones" />
		<p>
			Current password:
			<input type="password" name="current" class="pmf-password" />
		</p>
		<p>
			New password:
			<input type="password" name="new" class="pmf-new-password" />
		</p>
		<p>
			Confirm new password:
			<input type="password" name="confirm" class="pmf-new-password" />
		</p>
		<p>
			<input type="submit" value="Change password" />
		</p>
	</form>

### Errors

All error messages related to the submission of a password form, within a HTML
document, must have the `pmf-error` class.

**Code example:**

	<p class="pmf-error">
		Incorrect username and/or password.
	</p>

**Code example:**

	<p class="pmf-error">
		Invalid password - your password must not contain your username.
	</p>
	
### Versioning

The `body` element in a HTML document that conform with this specification has
the class `pmf-version-1`.

**Code example:**

	<!DOCTYPE HTML>
	<html>
		<head>
			<title>Login</title>
		</head>
		<body class="pmf-version-1">
			<h1>Login</h1>
			<form action="/login" method="POST" class="pmf-login">
				<p>
					Username:
					<input type="text" name="username" class="pmf-username" />
				</p>
				<p>
					Password:
					<input type="password" name="password" class="pmf-password" />
				</p>
				<p>
					<input type="submit" value="Login" />
				</p>
			</form>
		</body>
	</html>

## Password policies

### Policy encoding

A password policy is encoded as a [JSON][1] object with any of the following
members:

* `minLen` is the minimum number of characters a password can contain.
* `maxLen` is the maximum number of characters a password can contain.
* `mayHave` is a list of characters a password may contain.
* `mustHave` is a list of lists of characters a password must contain.

The values of the `minLen` and `maxLen` members must be non-negative
integers. The following sets of characters are defined for inclusion in
`mayHave` and `mustHave` values:

* `"lower"` expands to a list of the 26 lowercase ASCII characters.
* `"upper"` expands to a list of the 26 uppercase ASCII characters.
* `"digit"` expands to a list of the 10 ASCII digits.
* `"symbol"` expands to:
  `["!", "@", "#", "$", "%", "&", "*", "-", "+", "/", "="]`
* `"base64"` expands to a list of the 64 ASCII characters used in OpenPGP base
  64 encoding (RFC 4880)
* `"ascii"` expands to a list of the 95 printable ASCII characters from 32
  (space) to 126 (tilde)

**Note:** A future version of this document will include a section which
describes how to specify arbitrary other sets of characters as outlined by
[Stajano *et al*][2].

**Example:**

*Policy:*

"Password must contain between 8 and 16 characters long, consist of printable
ascii characters and contain a lower case letter and a digit."

*Code:*

	{
		minLen: 8,
		maxLen: 16,
		mayHave: "ascii",
		mustHave: ["lower", "digit"]
	}

**Example:**

*Policy:*

"Password must be at least 8 characters long."

*Code:*

	{
		minLen: 8
	}

### Embedding policies ###

A PMF password policy is embedded within a password form as the `value` of the
`hidden`-type `input` marked with the `pmf-policy` class.

**Code example:**

	<form action="/register" method="POST" class="pmf-register">
		<p>
			Username:
			<input type="text" name="username" class="pmf-username" />
		</p>
		<p>
			Email address:
			<input type="email" name="email" />
		</p>
		<p>
			New password:
			<input type="password" name="new" class="pmf-new-password" />
		</p>
		<p>
			Confirm new password:
			<input type="password" name="confirm" class="pmf-new-password" />
		</p>
		<p>
			Your new password must contain between 8 and 16 characters long, consist of printable ascii characters and contain a lower case	letter and a digit.
			<input
				type="hidden"
				class="pmf-policy"
				value='{
					minLen: 8,
					maxLen: 16,
					mayHave: "ascii",
					mustHave: ["lower", "digit"]
				}' />
		</p>
		<p>
			<input type="submit" value="Register" />
		</p>
	</form>

Any password form should contain a `pmf-policy` hidden input.

### Limitations ###

Useful policy rules which currently cannot be expressed in a PMF policy include
blacklists of known weak passwords and stateful checks such as "you can't use
your username or a variation of it". However, such violations are very unlikely
to occur if the password is generated randomly.

## Bibliography

1. *ECMA-404, The JSON Data Interchange Standard, 1st edition (October 2013)*,
   [http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf][1].
2. Stajano, F., Spencer, M., Jenkinson, G.: *Password-manager friendly (PMF):
Semantic annotations to improve the effectiveness of password managers*,
Pre-proceedings of Passwords 2014, Springer-Verlag,
[http://pmfriendly.org/static/documents/2014-StaSpeJen-pmf.pdf][2]

[1]: http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf "The JSON Data Interchange Standard"
[2]: http://pmfriendly.org/static/documents/2014-StaSpeJen-pmf.pdf "Password-manager friendly (PMF): Semantic annotations to improve the effectiveness of password managers"

[101]: http://passwords14.item.ntnu.no/ "Passwords 2014"
[102]: http://pmfriendly.org "pmfriendly.org"
