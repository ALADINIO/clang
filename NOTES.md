
# Random Notes


To time GCC preprocessing speed without output, use:
   "time gcc -MM file"
This is similar to -Eonly.


Creating and using a PTH file for performance measurement (use a release build).

```
$ clang -ccc-pch-is-pth -x objective-c-header INPUTS/Cocoa_h.m -o /tmp/tokencache
$ clang -cc1 -token-cache /tmp/tokencache INPUTS/Cocoa_h.m
```


  C++ Template Instantiation benchmark:
     http://users.rcn.com/abrahams/instantiation_speed/index.html


## TODO: File Manager Speedup:
<a name='speedup'></a>

 We currently do a lot of stat'ing for files that don't exist, particularly
 when lots of -I paths exist (e.g. see the <iostream> example, check for
 failures in stat in FileManager::getFile).  It would be far better to make
 the following changes:
   1. FileEntry contains a sys::Path instead of a std::string for Name.
   2. sys::Path contains timestamp and size, lazily computed.  Eliminate from
      FileEntry.
   3. File UIDs are created on request, not when files are opened.
 These changes make it possible to efficiently have FileEntry objects for
 files that exist on the file system, but have not been used yet.

 Once this is done:
   1. DirectoryEntry gets a boolean value "has read entries".  When false, not
      all entries in the directory are in the file mgr, when true, they are.
   2. Instead of starting the file in FileManager::getFile, check to see if
      the dir has been read.  If so, fail immediately, if not, read the dir,
      then retry.
   3. Reading the dir uses the getdirentries syscall, creating a FileEntry
      for all files found.

### Specifying targets:  -triple and -arch

The clang supports "-triple" and "-arch" options. At most one -triple and one
-arch option may be specified.  Both are optional.

The "selection of target" behavior is defined as follows:

(1) If the user does not specify -triple, we default to the host triple.
(2) If the user specifies a -arch, that overrides the arch in the host or
    specified triple.



``verifyInputConstraint`` and ``verifyOutputConstraint`` should not return ``bool``.

Instead we should return something like:
```
enum VerifyConstraintResult {
  Valid,

  // Output only
  OutputOperandConstraintLacksEqualsCharacter,
  MatchingConstraintNotValidInOutputOperand,

  // Input only
  InputOperandConstraintContainsEqualsCharacter,
  MatchingConstraintReferencesInvalidOperandNumber,

  // Both
  PercentConstraintUsedWithLastOperand
};
```

Blocks should not capture variables that are only used in dead code.

The rule that we came up with is that blocks are required to capture
variables if they're referenced in evaluated code, even if that code
doesn't actually rely on the value of the captured variable.

For example, this requires a capture:
  ```(void) var;```
But this does not:
  ``if (false) puts(var);``

Summary of <rdar://problem/9851835>: if we implement this, we should
warn about non-POD variables that are referenced but not captured, but
only if the non-reachability is not due to macro or template
metaprogramming.


We can still apply a modified version of the constructor/destructor
delegation optimization in cases of virtual inheritance where:
  - there is no function-try-block,
  - the constructor signature is not variadic, and
  - the parameter variables can safely be copied and repassed
    to the base constructor because either
    - they have not had their addresses taken by the vbase initializers or
    - they were passed indirectly.

