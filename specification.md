# PMF semantic markup standard

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

PMF was first presented at [Passwords 2014](http://passwords14.item.ntnu.no/)
and has a website at [pmfriendly.org](http://pmfriendly.org). PMF stands for
Password-Manager Friendly.

## Glossary

* "password form" - A `form` element which contains one or more `password`-type
  `input` element where the user types a new or existing password.

## Semantic classes

### Form inputs

#### Username inputs

An `input` element where the user would enter their username must have the
class `pmf-username`.

**Example:**

	<p>
		Username:
		<input type="text" name="username" class="pmf-username" />
    </p>

A password form which relates to a specific account and username, such as a
password-reset form, must contain a `hidden`-type `input` marked with the
`pmf-username` class and with its `value` attribute set to the username of the
account for the password is being changed or reset.

**Note:** The `name` of the hidden input should not be set to avoid
unnecessarily sending the username back to the server when the form is
submitted.

**Code example:**

	<input type="hidden" class="pmf-username" value="jimbojones" />

#### Password inputs

An `input` element where the user would enter an existing password must have
the class `pmf-password`.

**Code example:**

	<p>
		Password:
		<input type="password" name="password" class="pmf-password" />
	</p>

An `input` element where the user would enter a new password must have the
class `pmf-new-password`. Note that `pmf-password` and `pmf-new-password` will
typically appear together in change password forms.

**Code example:**

	<p>
		New password:
		<input type="password" name="new" class="pmf-new-password" />
	</p>
	<p>
		Confirm new password:
		<input type="password" name="confirm" class="pmf-new-password" />
	</p>

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

A password policy is encoded as a JSON object with between zero and four
members:

* `minLen` - minimum number of characters a password can contain.
* `maxLen` - maximum number of characters a password can contain.
* `mayHave` - set of characters a password may contain.
* `mustHave` - set of sets of characters a password must contain.

The values of the `minLen` and `maxLen` members must be non-negative
integers. The following sets of characters are defined for inclusion in
`mayHave` and `mustHave` values:

* `"lower"` - the 26 lowercase ASCII characters.
* `"upper"` - the 26 uppercase ASCII characters.
* `"digit"` - the 10 ASCII digits.
* `"symbol"` - the following 11 symbols: `!` `@` `#` `$` `%` `&` `*` `-` `+`
  `/` `=`
* `"base64"` - the 64 ASCII characters used in OpenPGP base 64 encoding (RFC
  4880)
* `"ascii"` - the 95 printable ASCII characters from 32 (space) to 126 (tilde)

The *Character classes* section below describes how to specify arbitrary other
sets of characters.

**Example:**

"Password must contain between 8 and 16 characters long, consist of printable
ascii characters and contain a lower case letter and a digit."

	{
		minLen: 8,
		maxLen: 16,
		mayHave: "ascii",
		mustHave: ["lower", "digit"]
	}

**Example:**

Policies may omit any of the four members:

"Password must be at least 8 characters long."

	{
		minLen: 8
	}
