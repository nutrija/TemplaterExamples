[documentation]: http://templater.info/

# Reporting for JVM and .NET

Templater is small reporting library with a minimal API.

These examples show various usages of the library.

Additional [documentation] is available from the official webpage.

## How Templater works

Templater works by analyzing provided docx/xlsx document for **tags**. Tags are snippets of text written in either **[[tag]]** or **{{tag}}** format. Tags can have metadata which can be used for various customization purposes, such as formatting.

When tags are placed in resizable part of the document Templater can repeat that part of the document when collection is provided as input. An example of this is a row in a table, list, page, sheet or entire document.

Templater doesn't have it's own editor, but rather uses documents prepaired in Word/Excel/some other editor. While this works great in most cases, this means Templater must infer intention from the structure of the document/tags, which doesn't contain any Templater specific extension.

Templater has minimal API for interaction:

 * high level *process* method
 * low level *replace*, *resize* and *clone*

Usage of Templater mostly consists from passing existing model to high level API.

## Data types

Most data types are processed by Templater with conversion to string. In Excel some types are converted into native Excel formats, such as numbers and dates.

Collections are usually processed by duplicating context which encapsulates all tags matching that collection.

Templater will use reflection to analyze classes, but it can work also on dynamic data types such as dictionary. 

Templater has special behavior for several data types, such as:

 * image - injects image into the document
 * nested array/DataTable - invokes dynamic resize feature of the Templater 
 * XML - can inject XML into Word as-is

and several others (DateTime, multiline strings, ...).

## Resizing

Low level resize(tags, count) API is used for duplicating part of the document. It can work for simple cases, such as single row in a table, but can be used for more complex ones:

 * multiple rows - tags can span multiple rows, which will cause multi-row copying
 * nested elements - tags can span multiple tables/lists which can be used for cloning complex document layouts
 * formula rewriting - as formulas are moved around and copied, their expressions will be adjusted accordingly
 * row/column styles - if possible styles will be maintained during horizontal or vertical resizing (horizontal resizing requires **horizontal-resize** metadata)
 * pushdown/pushright - elements bellow/right of tags area will be moved according to builtin rules
 * merge cells/named ranges/tables influence push rules by extending the affected region
  
## Extensibility

Templater has several extensibility points. Various plugins are embedded into library, while others can be registered during library initialization.