# Logger

The logger is the controller for creating logs. It has the following method.

## {severity} Log args

Creates a log record with log severity specified in `severity`.

`severity` must be a number defined in `Notela.LogSeverity` (default is `INFO`).

If `args` is a simple string or scalar, it is the body of the log message. Note that the body could be any of:

* simple string or number
* vector of items
* 2 column matrix of name item pairs

If `args` is a vector, the first element is the body of the message and the rest is attributes to attach to the log record.

Attributes are declared as one of:

* 2 column matrix of name value pairs
* vector of alternating name, value, name, value...
* vector of (name value) pairs

### Examples

```apl
logger.Log 'Hello world!'

Notela.LogSeverity.ERROR logger.Log 'Calculation error' 'exception.message' ⎕DMX.Message 'exception.type' ⎕DMX.EM

17 logger.Log 'Calculation error' ('exception.message' ⎕DMX.Message)('exception.type' ⎕DMX.EM)

attrs←0 2⍴''
attrs⍪←'exception.message'    ⎕DMX.Message
attrs⍪←'exception.type'       ⎕DMX.EM
attrs⍪←'exception.stacktrace' (∊⎕XSI,¨'[',¨(⍕¨⎕LC),¨']',¨⎕UCS 10)
20 logger.Log 'Calculation error' attrs
```

## {severity} LogError msg

Creates a log record with log severity specified in `severity`.

`severity` must be a number defined in `Notela.LogSeverity` (default is `ERROR`).

`msg` is the body of the log record (as for `Log`).

### Examples

```apl
 logger.LogError 'Calculation error'
```
