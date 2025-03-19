Proposed Replacement Official NestedText Test Suite
===================================================


Issue with Previous Test Suite
------------------------------

The previous test suite had the following issues.

1. It contained symbolic links, which caused problems on Windows.

2. The tests contain invisible characters in the form of tabs, end-of-line 
   spaces, unicode white spaces, control characters, and line termination 
   charactors.  These invisible characters were often confusing and could easily 
   be lost.

3. The tests were too tailored to the specific behavior of the Python NestedText 
   implementation.

4. The tests were categorized and numbers.  Both the categorization and 
   numbering were problematic.  Each test often fit into many categories but was 
   placed in one.  The numbering implied an order, but that ordering was 
   arbitrary.


The New Test Suite
------------------

These test cases are focused on assuring that valid NestedText input is 
properly read and invalid NestedText is properly identified by an 
implementation of NestedText.

No attempt is made to assure that an implementation produces valid NestedText.  
There is considerably flexibility in the way that NestedText may be generated.  
In light of this flexibility the best way to test a NestedText generator is to 
couple it with a NestedText reader and perform a round trip through both, which 
can be performed with these test cases.

The test cases are contained in tests.nt.  The convert command converts these 
test cases into JSON.  It may also add an additional field, types, which is 
a dictionary that contains the count of each possible line type that may be 
found in a NestedText document.  The line types are:

    blank
    comment
    dict item
    inline dict
    inline list
    key item
    list item
    string item
    unrecognized

These counts can be used to filter the tests to be run if desired.

It is expected that the JSON file (tests.json) will be used to test NestedText 
implementations).  The NestedText file of test cases (tests.nt) is used to 
generate tests.json, and it only needed if you plan to add or modify tests.  
Do not modify the JSON file directly, as any changes will be overridden 
whenever convert is run.

Each test case in tests.nt is a dictionary entry.  The key is used as the name 
of the test.  The keys must be unique and are largely chosen at random, but 
any words that are expected to be found within the test cases are rejected.  
This allows test cases to be quickly found by searching for their name.

The fields that may be specified are:

description (str):
    Simply describes the test.  Is optional and unused.

load_in (str):
    This is a string that will be fed into a NestedText load function.  This 
    string should be NestedText, though it may contain errors.

load_out (None | str | list | dict):
    The expected output from the NestedText load function if no errors are 
    expected.  If a string is given and if the first character in the string 
    is ‘!’, then the mark is removed and what remains is evaluated by Python, 
    with the result being the expected output.

load_err (dict):
    Details about the error that is expected to be found and reported by the 
    NestedText load function.  It consists of 4 fields:

    message (str):
        The error message that is emitted by the Python implementation of 
        NestedText.

    line (str):
        The text of the line in the NestedText input from load_in that 
        contains the error.

    lineno (None, int):
        The line number of the line that contains the error.  The first line 
        is line 0.

    colno (None, int):
        The number of the column where the error is likely to be.  The first 
        column is column 0.

Control characters and Unicode white space characters are expected to be 
backslash escaped in load_in, load_out, and load_err.lines.
