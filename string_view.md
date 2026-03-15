# StringView (`string_view.zc`)

`StringView` is a stack allocated string type. It is an opaquely aliased `Slice<char>` and does not ensure null-termination like `String` does.

It is primarily used for text parsing.

## Usage

```zc
import "./string_view.zc"

fn main() {
    let sv = StringView::from("Hello World");

    // first_word points to the start of "Hello World" and will have a length of 5
    // This cannot change the original string so it won't null terminate first_word
    let first_word = sv.chop_by_delim(' ');
    println "{first_word}\n{sv}"
}
```

## Struct Definition

```zc
opaque alias StringView = Slice<char>;
```

## Methods

| Method | Signature | Description |
| :--- | :--- | :--- |
| **from** | `StringView::from(s: char*) -> StringView` | Returns a StringView from a C string primitive
| **from_parts** | `StringView::from(s: char*, len: usize) -> StringView` | Returns a `StringView` from its needed parts, pointer and length
| **starts_with** | `starts_with(self, prefix: StringView) -> bool` | Checks if the string starts with the given prefix |
| **ends_with** | `ends_with(self, postfix: StringView) -> bool` | Checks if the string ends with the given postfix |
| **trim_left** | `trim_left(self)` | Trims any ascii spaces from the start of the `StringView` |
| **trim_right** | `trim_right(self)` | Trims any ascii spaces from the end of the `StringView` |
| **trim** | `trim(self)` | Trims any ascii spaces from both the start and the end of the `StringView` |
| **chop_left** | `chop_left(self, n: usize) -> StringView` | Chops `n` bytes from the start and returns the start of the original |
| **chop_right** | `chop_right(self, n: usize) -> StringView` | Chops `n` bytes from the end and returns the start of the original |
| **chop_starts_with** | `chop_starts_with(self, prefix: StringView) -> bool` | Checks if the string starts with the given prefix. If it is, it chops the prefix |
| **chop_by_delim** | `chop_by_delim(self, delim: char) -> StringView` | Chops until the first instance of delim. Returns chopped. Self does not start with delim after this call unless there were two or more contiguous delim bytes |
| **chop_by_line** | `chop_by_line(self) -> StringView` | Chops until the first line end. Returns chopped. Self does not start with the line end after this call unless there were two or more contiguous line ends |
| **get_line_iterator** | `get_line_iterator(self) -> StringViewLineIter` | Returns a line iterator to iterate by lines. It uses a copy of the pointer and length, not a pointer/reference |
| **_digit_val** | `StringView::_digit_val(c: int) -> int` | Returns the value of the character given when converted to a digit. It is used internally in `parse_s64()` |
| **parse_s64** | `parse_s64(self) -> (i64, bool)` | Trims the start and parses an integer. Returns (parsed integer, found_integer). Chops the parsed integer from self |

## Operators

| Operator | Method | Description |
| :--- | :--- | :--- |
| `==` | **eq** | `sv1 == sv2`. Structural equality check. |