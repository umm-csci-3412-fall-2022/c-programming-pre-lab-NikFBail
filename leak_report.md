# Leak report

Looking through check_whitespace.c, we see that there are two methods,
each of which assign a pointer to a value in memory. The first method,
strip() can either return the variable result, or "". The method
is_cleaned returns a variable result, but calls on strip(), and
therefore is dependent on the outcome of strip(). To free the value
of the pointer result in strip(), we have to free the result variable
from the test file, check_whitespace_test.cpp by creating a variable
in each of the test functions and using that variable to run the test,
freeing it right after.

To free the storage used by is_cleaned, a check is needed. Since
strip() doesn't always allocate to memory, sometimes it returns ""
instead of a allocation to memory, a check is needed to determine
that the string is not empty. Once the check is in place, freeing the
variable from memory runs smoothly.
