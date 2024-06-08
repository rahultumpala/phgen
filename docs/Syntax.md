# Syntax Guide

phgen uses a simple and powerful scripting syntax.

### Variables
```
// ---------------
// to set variables
// ---------------

#phgen_set_var_global{
    string : value
}
```

### Listeners
```
// ---------------
// to create listeners
// ---------------

#phgen_serve{
    method : HTTP_METHOD
    scheme: string
    host : string
    port : int
    path : string
    $query_params: @query_param_1
    $headers: @headers1
}
```

### Static Data
```
// ----------------
// to define static data
// ----------------

@headers1 {
    // JSON compliant body or empty
}

@query_param_1{
    // JSON compliant body or empty
}
```

### Generator
```
// ----------------
// to generate http requests to a location
// ----------------


#phgen_generate{
    method : HTTP_METHOD
    scheme: string
    host : string
    port : int
    path : string
    $query_params: @query_param_1
    $headers: @headers1
    $body: @body1
    __max_concurrent_reqs: [1-10000] // 1 by default
    __synchronize : [true|false] // false by default
}

@body1{
    "__text" : ""
    // to use raw text. if this param is set other params will be ignored and this text will be used as body.
}
```

### Include other phgen scripts
```
// ----------------
// Import and invoke another phgen script
// ----------------

#phgen_compose{
    // order will be preserved
    "1" : "path_to_script1.phgen"
    "2" : "path_to_script2.phgen"
}

```

### Note
- Fields prefixed with dollar, $ are optional
- Fields prefixed with double underscore, __ are internal Fields
- All other non prefixed fields are mandatory

# Syntax Grammar

EBNF Notation of phgen syntax is as follows

### Phgen Script

A phgen script is made up of blocks
```
phgen = block {block};
```

### Blocks
All the above listed examples are separated in their own blocks.

```
block = run_block | data_block ;

run_block = hash, block_name, "{",  block_definition, "}";
data_block = at_sign, block_name, "{", data_definition, "}";

hash = "#";
at_sign = "@";

(* Block name is defined at the end *)
```

### Block Definitions

All blocks need to follow the following block definition grammar, however internal and optional params are allowed only in run_blocks. This is verified before the script is executed.

```
block_definition = (block_key, ":", value, newline)*;
data_definition = (data_key, ":", value, newline)*;


block_key = [dollar | double_underscore], string;
data_key = quote, letter, {string}, quote;
value = (quote, string, quote)
      | number
      | boolean
      | JSON
      | at_sign, block_name;

quote = """; (* A single double quote character *)
double_underscore = "__";
newline = "\n";
dollar = "$";

(* String can contain all printable characters *)
(* letter includes all upper and lowercase alphabets *)
(* number follows float64 *)
(* boolean is true and false *)
(* JSON is a valid JSON defintion *)
```

Each block is identified using a name. Block names are restricted to having only letters, digits and underscores. Block names must begin with a letter.

```
block_name = letter, {block_name_term};
block_name_term = letter
                | digit
                | underscore;

underscore = "_";
```

### Comments

All lines starting with double forward slashes will be treated as comments and will be ignored.

```
comment = "//", string, newline; (* This is a phgen script comment *)
```

### Reserved Block Names

The following run block names are reserved by default, there are handlers implemented for these run blocks.
- phgen_set_var_global
- phgen_generate
- phgen_serve
- phgen_compose

A text document containing the entire EBNF notation is available here: [phgen EBNF](./phgen_ebnf.txt). It does not contain the entire EBNF for JSON schema, I assume the reader has knowledge of JSON syntax.