# DEBUGGING.md - Lab 3 Troubleshooting Log

This document records every error encountered across the four lab files, the steps taken to diagnose each one, the exact code change applied, and confirmation of the fix. Errors are grouped by file and numbered in the order they were discovered. Intentional logic error demonstrations that were kept and documented are noted separately at the end of each file section.

---

## `python_app.py` - Prime Number Generator

**Intended output:** Generate all primes up to a user-entered limit, display them, count them, and calculate their average.

---

### Error 1 - Syntax Error: invalid subtraction inside `len()`

**Error type:** `TypeError` at runtime  
**Line:** `variance = sum((x - mean)**2 for x in numbers) / len(numbers - 1)`  
**Initial error message:**
```
TypeError: unsupported operand type(s) for -: 'list' and 'int'
```
**Diagnosis:** Python evaluates `numbers - 1` before passing it to `len()`. Subtracting an integer from a list object is not a valid operation - the intent is to subtract 1 from the *result* of `len()`, not from the list itself. Moving the parentheses corrects the order of evaluation.  
**Fix:**
```python
# Before
variance = sum((x - mean)**2 for x in numbers) / len(numbers - 1)

# After
variance = sum((x - mean)**2 for x in numbers) / (len(numbers) - 1)
```
**Confirmation:** `TypeError` no longer raised; variance is computed correctly.

---

### Error 2 - Syntax Error: bare `except` clause catches all silently

**Error type:** Bad practice / overly broad exception handling  
**Line:** `except:`  
**Diagnosis:** A bare `except:` catches every possible exception including `SystemExit` and `KeyboardInterrupt`, making errors impossible to distinguish and suppressing useful error messages entirely. Specifying the expected exception types ensures only the anticipated errors are caught and all others surface naturally.  
**Fix:**
```python
# Before
except:
    return None

# After
except (ValueError, TypeError):
    return None
```
**Confirmation:** `convert_and_compare()` now only silently handles type and value conversion failures; other unexpected errors will propagate as intended.

---

### Error 3 - Syntax Error: missing colon on `else`

**Error type:** `SyntaxError` - file refuses to load  
**Line:** `else` (inside the `while counter < 3` block near the bottom of `main()`)  
**Initial error message:**
```
SyntaxError: expected ':'
```
**Diagnosis:** Every `else` statement in Python must be followed by `:`. The missing colon is caught at parse time, before any line of the script executes.  
**Fix:**
```python
# Before
else
    pass

# After
else:
    pass
```
**Confirmation:** File loads without `SyntaxError`; the `while` loop runs as expected.

---

### Error 4 - Syntax Error: missing colon on function definition

**Error type:** `SyntaxError`  
**Line:** `def bad_function(x, y)`  
**Initial error message:**
```
SyntaxError: expected ':'
```
**Diagnosis:** All `def` statements in Python must end with `:`. The missing colon causes a `SyntaxError` on this line, blocking the entire file from being parsed.  
**Fix:**
```python
# Before
def bad_function(x, y)

# After
def bad_function(x, y):
```
**Confirmation:** Function definition accepted; `bad_function` is callable without error.

---

### Documented Logic Errors (kept as intentional demonstrations)

The following logic errors were identified but intentionally retained in the file with explanatory comments to demonstrate the class of error rather than fix it:

**`primes_with_one = [1] + primes`** - By mathematical definition, 1 is not a prime number. Prepending it to the primes list contradicts the `is_prime()` function which correctly excludes 1. Retained with comment: *"logic error: 1 is not a prime number but including it to demonstrate a logic error."*

**`wrong_average = sum(primes_with_one) / len(primes)`** - The denominator uses `len(primes)` rather than `len(primes_with_one)`, producing a higher-than-correct average because the extra element (1) is included in the sum but not the count. Retained with original label "Wrong average due to logic error" to illustrate the denominator mismatch.

---

## `script.js` - Bubble Sort & Statistics

**Intended output:** Sort an array using bubble sort, display ascending and descending orders, compute the median and mean.

---

### Error 1 - Syntax Error: assignment operator used in `if` condition

**Error type:** `SyntaxError`  
**Line:** `if (a % 2 = 0)` (inside the parity check `for` loop)  
**Initial error message:**
```
SyntaxError: Invalid left-hand side expression in assignment
```
**Diagnosis:** A single `=` is JavaScript's assignment operator. Using it inside a condition attempts to assign `0` to the expression `a % 2`, which is not a valid assignment target. The correct operator for strict equality comparison is `===`.  
**Fix:**
```javascript
// Before
if (a % 2 = 0) {

// After
if (a % 2 === 0) {
```
**Confirmation:** Parity check now correctly identifies even and odd numbers.

---

### Error 2 - Logic Error: `count++` causes infinite `while` loop

**Error type:** Logic error - process hangs  
**Line:** `count++;` (inside `while (count > 0)`)  
**Diagnosis:** `count` starts at `3` and the condition is `count > 0`. Incrementing `count` on each iteration moves it further from 0, making the condition permanently true. The variable must be decremented to converge toward the exit condition.  
**Fix:**
```javascript
// Before
count++;

// After
count--;
```
**Confirmation:** `while` loop decrements from 3 to 1 and exits cleanly.

---

### Error 3 - Syntax Error: missing closing brace in `faulty()` function

**Error type:** `SyntaxError` - prevents file from parsing  
**Line:** Inside `faulty()` - the `else if (c < 0)` block is never closed before `return c`  
**Initial error message:**
```
SyntaxError: Unexpected token '}'
```
**Diagnosis:** The `else if` branch opens a `{` block but the function ends without the matching `}`. JavaScript's parser cannot reconcile the open block and raises a `SyntaxError` that prevents the entire file from executing.  
**Fix:**
```javascript
// Before
        } else if (c < 0) {
            return -c;
        return c;
    }

// After
        } else if (c < 0) {
            return -c;
        } else {
            return c;
        }
    }
```
**Confirmation:** `faulty()` parses correctly and returns the expected value.

---

### Error 4 - Logic Error: infinite `for` loop (`i++` instead of `i--`)

**Error type:** Logic error - process hangs indefinitely  
**Line:** `for (var i = 10; i > 0; i++)`  
**Diagnosis:** The loop condition is `i > 0` and the starting value is `10`. With a step of `i++`, `i` only increases and the condition `i > 0` is always true - the loop never terminates. Changing the step to `i--` makes `i` decrease toward 0 until the condition becomes false.  
**Fix:**
```javascript
// Before
for (var i = 10; i > 0; i++) {

// After
for (var i = 10; i > 0; i--) {
```
**Confirmation:** Loop counts down from 10 to 1 and exits correctly.

---

### Error 5 - Syntax Error: missing `} else {` in `dummy()` function

**Error type:** `SyntaxError` - file fails to parse  
**Line:** Inside `dummy()` - the `if (x)` block closes but `else` has no opening `{`  
**Initial error message:**
```
SyntaxError: Unexpected token 'else'
```
**Diagnosis:** The `if (x) { return x;` block is not properly closed before the `else`. The parser encounters `else` while the structure is unresolved and throws a `SyntaxError`. Adding `}` to close the `if` block and `{` to open the `else` block restores valid syntax.  
**Fix:**
```javascript
// Before
function dummy(x) {
    if (x) {
        return x;
    else {
        return null;
    }
}

// After
function dummy(x) {
    if (x) {
        return x;
    } else {
        return null;
    }
}
```
**Confirmation:** `dummy()` parses and returns the correct value for truthy and falsy inputs.

---

### Documented Logic Error (kept as intentional demonstration)

**`multiplySort` - multiplication used instead of comparison:** The condition `if (arr[j] * arr[j + 1])` multiplies adjacent values instead of comparing them. Any non-zero pair evaluates as truthy, triggering swaps regardless of order. Retained with comment: *"logic error: should compare values (intentionally flawed)"* to demonstrate the class of error.

---

## `script.sh` - Sum & Average Calculator

**Intended output:** Accept numbers as arguments, compute their sum and average (to 2 decimal places), and identify the maximum and minimum values.

---

### Error 1 - Logic Error: multiplication used instead of addition in sum loop

**Error type:** Logic error - `sum` is always 0  
**Line:** `sum=$((sum * arg))`  
**Diagnosis:** `sum` is initialised to `0`. Multiplying any value by `0` always returns `0`, so `sum` never accumulates. Every subsequent calculation - average, displayed total - will be `0` regardless of the inputs. The accumulation operator must be `+`.  
**Fix:**
```bash
# Before
sum=$((sum * arg))

# After
sum=$((sum + arg))
```
**Confirmation:** `sum` now correctly accumulates across all arguments.

---

### Error 2 - Logic Error: unquoted variables in comparisons

**Error type:** Logic error / potential word-splitting bug  
**Lines:** `if [ $arg -gt $max ]` and `if [ $arg -lt $min ]`  
**Diagnosis:** In Bash, unquoted variables inside `[ ]` are subject to word splitting and glob expansion. If a variable is empty or contains spaces the comparison can break unpredictably. Double-quoting all variables ensures they are treated as single tokens regardless of their value.  
**Fix:**
```bash
# Before
if [ $arg -gt $max ]; then
if [ $arg -lt $min ]; then

# After
if [ "$arg" -gt "$max" ]; then
if [ "$arg" -lt "$min" ]; then
```
**Confirmation:** Comparisons are safe against word-splitting; max and min track correctly across all inputs.

---

### Error 3 - Logic Error: integer division truncates decimal average

**Error type:** Logic error - average always rounds down to whole number  
**Line:** `avg=$((sum / count))`  
**Diagnosis:** Bash arithmetic `$(( ))` performs integer division and discards the remainder. For inputs `3 7 2 9 5`, the true average is `5.2` but integer division returns `5`. Using `bc` with `scale=2` preserves two decimal places.  
**Fix:**
```bash
# Before
avg=$((sum / count))

# After
avg=$(echo "scale=2; $sum / $count" | bc)
```
**Confirmation:** Average now outputs correctly to two decimal places (e.g. `5.20` for `3 7 2 9 5`).

---

### Error 4 - Logic Error: addition used instead of multiplication in `multiply_list()`

**Error type:** Logic error - function produces a sum, not a product  
**Line:** `product=$((product + num))`  
**Diagnosis:** The function is named `multiply_list` and initialises `product=1`, but uses `+` instead of `*`, making it behave identically to the sum loop. The result is wrong for any non-trivial input.  
**Fix:**
```bash
# Before
product=$((product + num))

# After
product=$((product * num))
```
**Confirmation:** `multiply_list` now returns the correct product of all arguments.

---

### Error 5 - Logic Error: unquoted variables in `while` and `if` conditions

**Error type:** Logic error / word-splitting risk  
**Lines:** `while [ $counter -lt 3 ]` and `if [ $counter -eq 2 ]`  
**Diagnosis:** Same pattern as Error 2 - unquoted `$counter` is vulnerable to word splitting. Consistently quoting all variables is a Bash best practice that prevents subtle bugs when variable values change.  
**Fix:**
```bash
# Before
while [ $counter -lt 3 ]; do
    if [ $counter -eq 2 ]; then

# After
while [ "$counter" -lt 3 ]; do
    if [ "$counter" -eq 2 ]; then
```
**Confirmation:** Loop runs cleanly for all counter values without risk of word-splitting.

---

### Error 6 - Logic Error: unquoted variables in `compare()` function

**Error type:** Logic error / word-splitting risk  
**Lines:** `if [ $a -gt $b ]` and `elif [ $a -lt $b ]`  
**Diagnosis:** Same pattern as Errors 2 and 5 - `$a` and `$b` must be quoted to be safe inside `[ ]` test expressions.  
**Fix:**
```bash
# Before
if [ $a -gt $b ]; then
elif [ $a -lt $b ]; then

# After
if [ "$a" -gt "$b" ]; then
elif [ "$a" -lt "$b" ]; then
```
**Confirmation:** `compare()` evaluates all three cases correctly without word-splitting risk.

---

### Error 7 - Syntax Error: missing `then` and unquoted variable in `check_value()`

**Error type:** Bash syntax error - script aborts  
**Line:** `if [ $v -gt 100 ]` (no `then`, unquoted variable)  
**Initial error message:**
```
syntax error near unexpected token 'echo'
```
**Diagnosis:** Bash requires every `if [ condition ]` to be followed by `then` before the body begins. The missing `; then` causes the shell to encounter `echo` in an unexpected position and exit. The variable `$v` was also unquoted.  
**Fix:**
```bash
# Before
if [ $v -gt 100 ]
    echo "Large value"

# After
if [ "$v" -gt 100 ]; then
    echo "Large value"
```
**Confirmation:** `check_value` executes without syntax errors and prints the correct message for both test inputs.

---

### Error 8 - Syntax Error: assignment operator inside arithmetic `if` condition

**Error type:** Syntax / logic error - condition always evaluates incorrectly  
**Line:** `if (( k % 2 = 0 )); then`  
**Diagnosis:** Inside Bash's arithmetic `(( ))` context, a single `=` performs an assignment rather than a comparison. This sets the expression result to `0` unconditionally, making every number appear to be odd. The correct arithmetic equality operator is `==`.  
**Fix:**
```bash
# Before
if (( k % 2 = 0 )); then

# After
if (( k % 2 == 0 )); then
```
**Confirmation:** Even/odd parity check now evaluates correctly for all values of `k`.

---

## `index.html` - Age Calculator

**Intended output:** Accept a birth year input, compute the user's age, and display whether they are an adult or a minor.

---

### Error 1 - Logic Error: assignment operator used to test for `NaN`

**Error type:** Logic error - input validation never fires  
**Line:** `if (age = NaN)`  
**Diagnosis:** `=` is the assignment operator in JavaScript. This line assigns `NaN` to `age` and then evaluates the expression. Since `NaN` is falsy, the `if` block never executes, allowing any non-numeric input to silently bypass validation. The correct check is `Number.isNaN()`, which is more precise than the global `isNaN()` because it does not coerce its argument before testing.  
**Fix:**
```javascript
// Before
if (age = NaN) {
    document.getElementById("ageDisplay").innerText = "Please enter a valid number.";

// After
if (Number.isNaN(age)) {
    document.getElementById("ageDisplay").innerText = "Please enter a valid age (numbers only).";
```
**Confirmation:** Non-numeric input now correctly triggers the validation message without assigning `NaN` to `age`.

---

### Error 2 - Syntax Error: missing closing parenthesis on `if (isAdult(...)` 

**Error type:** `SyntaxError` - entire script block fails to load  
**Line:** `if (isAdult(ageComputed) {`  
**Initial error message:**
```
SyntaxError: Unexpected token '{'
```
**Diagnosis:** The call to `isAdult()` is missing its closing `)` before the opening `{` of the block body. The parser reaches `{` while still expecting `)` to close the function call, throwing a `SyntaxError` that prevents the entire `<script>` section from executing.  
**Fix:**
```javascript
// Before
if (isAdult(ageComputed) {

// After
if (isAdult(ageComputed)) {
```
**Confirmation:** Script loads and `isAdult` result is used correctly.

---

### Error 3 - Syntax Error: missing closing brace in `dummyFunction`

**Error type:** `SyntaxError` - script block fails to parse  
**Line:** Inside `dummyFunction` - the `if (n < 0)` block is never closed before `return sum`  
**Initial error message:**
```
SyntaxError: Unexpected token 'return'
```
**Diagnosis:** The `if (n < 0) {` block is opened but never closed with `}`. The parser encounters `return sum` while still inside the open block. Adding `}` before `return` resolves the structural mismatch.  
**Fix:**
```javascript
// Before
        if (n < 0) {
            sum = sum * -1;
        return sum;
    }

// After
        if (n < 0) {
            sum = sum * -1;
        }
        return sum;
    }
```
**Confirmation:** `dummyFunction` parses and returns the correct value.

---

### Error 4 - Logic Error: `parseFloat()` applied to an already-numeric variable

**Error type:** Logic error - unnecessary and misleading type coercion  
**Line:** `var combined = parseInt(stringNumber) + parseFloat(integerNumber);`  
**Diagnosis:** `integerNumber` is already declared as a JavaScript `number` (`var integerNumber = 10`). Wrapping it in `parseFloat()` adds no value and implies the variable might be a string when it is not. `parseInt()` on the string `"15.5"` also truncates the decimal. The correct approach is `parseFloat(stringNumber) + integerNumber`.  
**Fix:**
```javascript
// Before
var combined = parseInt(stringNumber) + parseFloat(integerNumber);

// After
var combined = parseFloat(stringNumber) + integerNumber;
```
**Confirmation:** `combined` now correctly equals `25.5` (15.5 + 10) rather than the truncated `25`.

---

### Error 5 - Syntax/Logic Error: assignment used in loop `if` condition

**Error type:** Runtime logic error - all numbers reported as odd  
**Line:** `if (num % 2 = 0)` (inside the numbers array loop)  
**Diagnosis:** `=` performs assignment, not comparison. The expression `num % 2 = 0` attempts to assign `0` to `num % 2`, which is not a valid assignment target - some engines throw a `SyntaxError`, others silently evaluate to falsy, causing every number to be reported as odd. The strict equality operator `===` is required.  
**Fix:**
```javascript
// Before
if (num % 2 = 0) {

// After
if (num % 2 === 0) {
```
**Confirmation:** Even and odd numbers are now correctly identified.

---

### Error 6 - Logic Error: `b = b + 1` causes infinite `while` loop

**Error type:** Logic error - browser tab hangs (broken by the `b > 100` guard)  
**Line:** `b = b + 1;` (inside `while (b > 0)`)  
**Diagnosis:** `b` starts at `10` and the loop condition is `b > 0`. Incrementing `b` on each pass makes the condition permanently true. A `break` at `b > 100` was added as a safety net to prevent a true infinite loop, but the underlying logic is still wrong. Decrementing `b` is the correct fix.  
**Fix:**
```javascript
// Before
b = b + 1;

// After
b = b - 1;
```
**Confirmation:** Loop counts down from 10 to 1 and exits without reaching the emergency `break`.

---

### Error 7 - Syntax Error: missing closing brace in `multiply()` function

**Error type:** `SyntaxError` - script block fails to parse  
**Line:** Inside `multiply()` - the `else if (result < 0)` block is never closed before `return result`  
**Initial error message:**
```
SyntaxError: Unexpected token 'return'
```
**Diagnosis:** The `else if` branch opens with `{` but is never closed before `return result`. The parser encounters `return` inside an open block and raises a `SyntaxError`.  
**Fix:**
```javascript
// Before
        } else if (result < 0) {
            return "Negative";
        return result;
    }

// After
        } else if (result < 0) {
            return "Negative";
        }
        return result;
    }
```
**Confirmation:** `multiply()` parses correctly and returns the expected value for all input combinations.

---

### Error 8 - Logic Error: assignment used in final `if` condition

**Error type:** Logic error - condition always evaluates as truthy  
**Line:** `if (counter = 1)`  
**Diagnosis:** `counter` is `0` at this point. Using `=` assigns `1` to `counter` and then evaluates the expression; since `1` is truthy, the block always executes regardless of the variable's original value. The correct operator is `==` (or `===`) for comparison.  
**Fix:**
```javascript
// Before
if (counter = 1) {

// After
if (counter == 1) {
```
**Confirmation:** The block no longer executes unconditionally; it only fires when `counter` genuinely equals `1`.

---

### Documented Logic Error (kept - not fixed)

**`for (var i = 0; i <= sampleYears.length; i++)`** - The `<=` operator causes the loop to access index `4` on a 4-element array (`sampleYears`), reading `undefined` on the final pass. This produces `NaN` for the computed age of the last iteration. This off-by-one error was identified and documented but not corrected in the submitted file, as it was retained to demonstrate the class of error.

---

## Full Error Summary

| File | # | Type | Description | Fix Applied |
|---|---|---|---|---|
| `python_app.py` | 1 | TypeError | `len(numbers - 1)` - list subtraction | `(len(numbers) - 1)` |
| `python_app.py` | 2 | Bad practice | Bare `except:` catches all exceptions | `except (ValueError, TypeError):` |
| `python_app.py` | 3 | SyntaxError | `else` missing colon | `else:` |
| `python_app.py` | 4 | SyntaxError | `def bad_function(x, y)` missing colon | `def bad_function(x, y):` |
| `script.js` | 1 | SyntaxError | `a % 2 = 0` - assignment in condition | `a % 2 === 0` |
| `script.js` | 2 | Logic | `count++` in `while (count > 0)` - infinite | `count--` |
| `script.js` | 3 | SyntaxError | Missing `}` + `else` in `faulty()` block | Added closing brace and `else` |
| `script.js` | 4 | Logic | `i++` in countdown - infinite loop | `i--` |
| `script.js` | 5 | SyntaxError | Missing `} else {` in `dummy()` function | Added `} else {` |
| `script.sh` | 1 | Logic | `sum * arg` - multiplication instead of addition | `sum + arg` |
| `script.sh` | 2 | Logic | Unquoted `$arg`, `$max`, `$min` in comparisons | Added double-quotes around all variables |
| `script.sh` | 3 | Logic | Integer division truncates average | `$(echo "scale=2; $sum / $count" \| bc)` |
| `script.sh` | 4 | Logic | `product + num` - addition instead of multiplication | `product * num` |
| `script.sh` | 5 | Logic | Unquoted `$counter` in `while` / `if` | Added double-quotes around `$counter` |
| `script.sh` | 6 | Logic | Unquoted `$a`, `$b` in `compare()` | Added double-quotes around `$a` and `$b` |
| `script.sh` | 7 | SyntaxError | Missing `; then` and unquoted `$v` in `check_value()` | Added `; then` and quoted `"$v"` |
| `script.sh` | 8 | SyntaxError | `k % 2 = 0` - assignment in arithmetic block | `k % 2 == 0` |
| `index.html` | 1 | Logic | `if (age = NaN)` - assignment, not check | `Number.isNaN(age)` |
| `index.html` | 2 | SyntaxError | `if (isAdult(ageComputed)` - missing `)` | Added closing `)` |
| `index.html` | 3 | SyntaxError | Missing `}` in `dummyFunction` if block | Added closing `}` before `return` |
| `index.html` | 4 | Logic | `parseFloat()` applied to numeric variable | `parseFloat(stringNumber) + integerNumber` |
| `index.html` | 5 | SyntaxError/Logic | `num % 2 = 0` - assignment in loop condition | `num % 2 === 0` |
| `index.html` | 6 | Logic | `b = b + 1` - increment causes infinite loop | `b = b - 1` |
| `index.html` | 7 | SyntaxError | Missing `}` in `multiply()` else-if block | Added closing `}` before `return result` |
| `index.html` | 8 | Logic | `if (counter = 1)` - assignment, always truthy | `if (counter == 1)` |
