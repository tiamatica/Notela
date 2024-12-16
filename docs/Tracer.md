# Tracer

The tracer is the controller for creating spans. It has the following methods.

## r ← GetActiveSpan

Returns the active span if there is one, otherwise `⍬`.

## r ← GetContext

Returns the current context in the form of a matrix of HTTP headers.

## span ← {context} StartSpan name

Creates a span and sets its `name`.

If `context` is provided it should be in the form of a matrix of HTTP headers. The span is then created in that context.

## span ← {context} StartActiveSpan name

As `StartSpan`, but in addition, sets the span as active in the current context.

