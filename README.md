
# KWARGSVERIFY a verifier and sanitizer for python kwargs parameters

The kwargsverify package simplifies validation & sanitation of python kwargs parameter, your can define most validations declaratively, in a map.
See the following example, where the validator definitions are passed to the ```kwchecker.KwArgsChecker``` constructor.

```
        def func_to_test(**kwargs):
            checker = kwchecker.KwArgsChecker(required={
                    "first_name": (
                        str,
                        kwchecker.no_regex_validator("^\s*$", "Error: empty first name"),
                        kwchecker.strip_leading_trailing_space(),
                        kwchecker.capitalize_first_letter(),
                    ),
                    "last_name": (
                        str,
                        kwchecker.no_regex_validator(r"^\s*$", "Error: empty last name"),
                        kwchecker.strip_leading_trailing_space(),
                        kwchecker.capitalize_first_letter(),
                    ),

                    "title" : str,

                    "email" : kwchecker.email_validator(),

                    "phone" : (
                        kwchecker.regex_validator(r"^[0-9\+\-,\ \(\)#]*$", "not a vald phone number")
                        )
                }, opt={
                    "mood": kwchecker.int_range_validator(0, 10),
                    "plan": str
                })

            checker.validate(kwargs)

```
The constructor has two arguments ```required``` receives the definition for mandatory parameters passed via kwargs, ```opt``` receives definitions of optional parameters.
An errror is triggered, if an actual parameter in kwargs does not appear in any of these definitions.]

Each allowed parameter name appears as a key in the dictionary arguments. The value of the entry defines the actual validation/sanitation.
A type value will check, if the supplied argument is of the requested type

```"title" : str``` - this will check if the kwargs parameter ```title``` is of type string.

A funcion value will be either a validation function, or a sanitier function.

```"email" : kwchecker.email_validator()``` - this validates that the ```email``` parameter in kwargs is a valid email address. Note that function ```email_validator``` is being called here, as it returns a closure.

You may pass a tuple of values that are either types or functions. see

```
"last_name": (
    str,
    kwchecker.no_regex_validator(r"^\s*$", "Error: empty last name"),
    kwchecker.strip_leading_trailing_space(),
    kwchecker.capitalize_first_letter(),
)
```
The action for the kwargs ```last_name``` parameter will first verify that parameter to be of type string, then it will verify, that the value is not an empty string ```kwchecker.no_regex_validator(r"^\s*$", "Error: empty last name")```, then it will strip the leading and trailing whitespaces ```kwchecker.strip_leading_trailing_space()```, and capitalize the first letter of the value ```kwchecker.capitalize_first_letter()```








