# Password-Manager Friendly (PMF) semantic markup

## Definitions

* "document" - A HTML document.
* "form" - A HTML form object, a HTML `form` element and its contents.
* "input"/"field" - A HTML `input` element within a form.
* "password input" - An `input` element with its `type` attribute set to
  `password`.
* "password form" - A "form" which includes an "password input".

## Semantic class annotations

### Versions

Mark the `body` element of a document that conform with this specification with
the class `pmf-version-1`.

**Example:**

	<body class="pmf-version-1">

### Form types

There are 4 PMF semantic classes for different types of password form:

1. Mark login forms with the class name `pmf-login`.
2. Mark registration forms with the class name `pmf-register`.
3. Mark change passwords forms with the class name `pmf-change-password`.
4. Mark password reset forms with the class name `pmf-reset-password`.

**Example:**

    <form action="/login" method="POST" class="pmf-login">

### Input types

#### Username inputs

For login and registration forms, mark all inputs where the user would enter
their username with the class name `pmf-username`.

**Example:**

    <form action="/register" method="POST" class="pmf-register">
        ...
		Username:
		<input type="text" name="username" class="pmf-username" />
		...
    </form>

For change password and password reset forms, add a new `hidden`-type input
marked with the `pmf-username` class and with its `value` attribute set to the
username of the account for the password is being changed or reset.

**Example:**

	<form action="/changepassword" method="POST" class="pmf-change-password">
		<input type="hidden" class="pmf-username" value="jimbojones" />
		...
	</form>

#### Password inputs

Mark all inputs where the user would enter an existing password with the class
name `pmf-password`.

**Example:**

	<form action="/login" method="POST" class="pmf-login">
		...
		Password:
		<input type="password" name="password" class="pmf-password" />
		...
	</form>

Mark all inputs where the user would enter a new password with the class name
`pmf-new-password`. Note that `pmf-password` and `pmf-new-password` will
typically appear together in change password forms.

**Example:**

	<form action="/changepassword" method="POST" class="pmf-change-password">
		<input type="hidden" class="pmf-username" value="jimbojones" />
		
		Current password:
		<input type="password" name="current" class="pmf-password" />
		
		New password:
		<input type="password" name="new" class="pmf-new-password" />
		
		Confirm new password:
		<input type="password" name="confirm" class="pmf-new-password" />
	</form>

#### Stay-signed-in inputs

Mark all `checkbox`-type inputs which the user would check to stay signed in
for a longer amount of time with the `pmf-stay-signed-in` class name.

**Example:**

	<form action="/login" method="POST" class="pmf-login">
		...
		Stay signed in?
		<input type="checkbox" name="persist" class="pmf-stay-signed-in" />
		...
	</form>

### Errors

Mark all error messages related to the submission of a password form with the
`pmf-error` class name.

**Example:**

	<p class="pmf-error">
		Incorrect username and/or password.
	</p>

**Example:**

	<p class="pmf-error">
		Invalid password - your password must not contain your username.
	</p>

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
