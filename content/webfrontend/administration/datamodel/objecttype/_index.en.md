---
title: "165 - Object Types"
menu:
  main:
    name: "Object Types"
    identifier: "webfrontend/administration/datamodel/objecttype"
    parent: "webfrontend/administration/datamodel"
---
# Object types

## Object type

![*View in the Tab Object Type*](objekttyp_en.png)

| Setting | Option | Explanation |
| --- | --- | --- |
| Name | | Name of the object type in the database. Not many characters are allowed, since this name may be used for exports, etc., and therefore can not contain complex characters [(see rules)](#objecttype_name_rules)
| Designation | | Is the display name of the object type. This is where the name of the object type appears in the application. This field is multilingual
| Comment (internal) | | A free comment, which is only displayed here |
| Database options | Pool management | easydb manages this object type in the pool hierarchy. That The records of this object type are always assigned exactly to one pool. Tag management, rights management and transition management can be operated via pools. You should organize your main records into pools
| | Hierarchical | easydb manages this object type as a hierarchy. That Exactly one father can be assigned to the same object type. Records without a father are called top-level records
| | Editable in linking (Reverse Edit) | With hierarchical object types, all children (and child children) of a father can be displayed directly and changed. For the child levels, a different field visibility can be set via masks than for the father's level. The data record indexing is always performed with this setting, including all subordinate children
| Search options | Search | The object type is thus available in the main search (Search). This allows the object type to be managed in folders
| Permissions | Individual permissions per record | If active, permissions can also be set for this object type on the individual record. Without this setting, rule management for the records of this object type is only possible via pools (or object types if the object type does not support pool management), tags (if the object type supports tag management), and folders
| | Assignment of tags | With tag management activated, tags can be assigned to each record of this object type. You can use tag management and workflow management

<a name="objecttype_name_rules"></a>
### Rules for names of objecttypes

To avoid internal conflicts with the names of objecttypes, the following rules apply to each objecttype name:

- No objecttype name can have the prefix `ez_` (case insensitive check)

- An objecttype can must match this regex: `^[a-z][a-z0-9_]{0,61}[a-z0-9]$`
    - only lowercase letters (`a` - `z`), numbers or `_` allowed
    - minimal length of 2 characters, maximal length of 63 characters
    - must start with a letter

- The objecttype name **must not be any** of the following system tables:
    - `pool`
    - `collection`
    - `user`
    - `group`
    - `message`

## Fields

![*View in the Fields tab*](felder_en.png)

| Setting | Option | Explanation
| --- | --- | --- |
| Field | Field | Name for the database field. The fields of object types are created directly in the database as a table field. Not all characters can be used, since this name is also used in exports, etc.
| | Multi-field (Nested Table) | Multi-field name. For multiple fields, easydb creates a separate table in the database. In this way, multiple fields can again define their own fields. Multiple fields can be interleaved
| Designation | | Ad names of the field as seen by users in ads and editors. Description is multilingual. Note that names can only be specified in object types. Masks define only the visibility of the fields, but no own ad names
| Data type | One-line text | One-line text allows the data entry in one line, thus texts without line breaks. The length is not limited. Texts are indexed word-by-word in the index and can be searched for right
| | One-line text (multilingual) | The multilingual *one-line text* allows data entry in several languages. When the data record is displayed, the language of the user is preferably used. If this language is not filled, the system automatically checks whether another language is filled. According to which languages the user searches, he can set himself
| | Multiple-line text | Like *Single text* only the input can be made with line breaks. Larger input fields can be seen in the editor
| | Multiline text (multilingual) | The multi-lingual one-line text allows data entry in several languages. When the data record is displayed, the language of the user is preferably used. If this language is not filled, the system automatically checks whether another language is filled. The languages the user is looking for can be set by the user. |
| | Simple text (string) | A string is used for names or reference IDs or anything that can be searched left and right
| Date + Time | Date | The date type supports pure year numbers, year + month and year + month + day. Depending on the front-end language, the input and output formats differ. When searching, the broader date and year + month ranges are treated as a period running from 1 January to 31 December and from month 1 to month to month
| | Date (Range) | This type can be used to specify a lower and upper date boundary (From+To). From or To is optional when entered and means that either the start or end point is unclear. Like Date, only years, year+month or year+month+day can be entered. In the editor, when a date is entered in the "from" field, the same value is automatically entered in the "to" field as well (if this field was empty before). Of course, this value can be changed or removed manually.  |
| | Date + Time | This type also allows you to enter a time with hours and minutes during the date input. The API can also store seconds and time zones (as UTC offset). When saving, the current time zone of the user's browser is saved |
| Numeric | Number (integer) | This type stores numbers without decimal places. Figures can be searched on both sides truncated in the fulltext search
| | Number (digits) | This type can be used for money amounts and stores a number with two decimal places
| Other | File | In this type, easydb saves files. Each file can be stored in different versions, so that a list of files is maintained internally. A file must be set as a preferred file. Easydb generates preview images for files and offers format conversions for formats such as audio and video that allow direct listening and viewing in the browser
| | Yes / No field (boolean) | This type stores the state set or not set. In the database, the default value is set to not set. This data type is displayed as a checkbox in the frontend
| Object Types | Existing Object Types | This type creates so-called forward links to other data records. The easydb record ID of the linked data record is saved internally. Through the forward linking, the linked records in the masks for input and output as well as for the search can be taken into account. Linked records are entered and stored independently of the linked records. However, linked records can not be deleted
| Additional data types | | Further data types can be integrated and selected here via plugins
| Review | | A check can be stored for each database field and is carried out at every storage. The options for checks can differ depending on the data type
| | No Verification | No verification is performed with this setting. Easydb accepts all values. On a technical page, text fields contain no empty string (""), but a database *null*. Text fields are trimmed automatically, i. Empty the beginning and the end are automatically removed |
| | Not empty | It is a *mandatory field*. Something has to be entered in this field, can not be saved before. If the mandatory field consists of several fields for multilingualism, only one language must be filled with an entry |
| | Email | It is checked whether the input is an e-mail address. This check is a simple verification of characters and does not guarantee that it is a valid and assignable e-mail address
| | Regular expression (regexp) | Regular expression checks the input for specific rules: [Tcl Advanced Regular Expression](https://www.regular-expressions.info/tcl.html). The regular expression consists of *Regexp* and *Regexp Flags* |
| | Date Range | For date fields, a date range can be stored, each with an *Exclusive* checkbox |
| | No blank entries (*NOT NULL*) | This is used to avoid empty fields in the database. See the [detailed explanation below](#notnull) |
| | No duplicate entries | With this check, easydb ensures that there are no duplicate entries for this field. Exceptions are empty fields which are not taken into account during this check. With *`UNIQUE (A)`* to *`UNIQUE (C)`*, you can define UNIQUE checks that work across multiple fields (in the same object type). It is checked that the combination of inputs is unique over all fields involved |
| Editable in linking (Reverse Edit) | | See the [detailed explanation below](#reverse-edit) |
| Bidirectional | | See the [detailed explanation below](#bidirectional) |
| Bidirectional Reverse | | See the [detailed explanation below](#bidirectional-reverse) |

### Changing the type of existing fields {#typechange}

The type of existing fields can only be changed between certain types. Most changes can not be handled by the database and are therefore not allowed. The changes are checked when the new datamodel version is saved, and an error is thrown if there are any invalid type changes. In these cases a new field must be created and the data must be migrated.

*Only* the following changes are possible:

| Old type | Allowed new types | Additional changes to existing data |
|---|---|---|
| Any of One-line text, One-line text (multilingual), Multiple-line text, Multiline text (multilingual), Simple text (string) | Any of One-line text, One-line text (multilingual), Multiple-line text, Multiline text (multilingual), Simple text (string) | Changes from non-multilingual texts to multilingual texts: the value of the old text field is used for all database languages in the multilingual text field; Changes from multilingual texts to non-multilingual texts: the first non-empty text in the order of the database languages is used as the new value of the text field |
| Any of Date, Date (Range), Date + Time | Any of One-line text, One-line text (multilingual), Multiple-line text, Multiline text (multilingual), Simple text (string) | Internal database cast to text. For multilingual fields the text value will be used for all database languages |
| Date | Date + Time | The time part of the new date time value will be `00:00:00` |
| Date | Date (Range) | The date value is used as the start and end date of the new range |
| Date + Time | Date | The time part of the old value will be lost |
| Date + Time | Date (Range) | The date time value is used as the start and end date of the new range |
| Number (integer) | Any of One-line text, One-line text (multilingual), Multiple-line text, Multiline text (multilingual), Simple text (string) | Internal database cast to text. For multilingual fields the text value will be used for all database languages |
| Number (digits) | Any of One-line text, One-line text (multilingual), Multiple-line text, Multiline text (multilingual), Simple text (string) | This datatype stores the currency value in the smaller currency, which means that the value is stored as multiplied with `100` in the database. An old value of `1.56` is saved as `156` in the database, and the resulting string will be `'156'`. For multilingual fields the text value will be used for all database languages |
| Number (integer) | Number (digits) | The value in the database is multiplied by `100` and represented with additional zeros after the comma. An old value of `156` is saved as `15600` and represented as `156.00` |
| Number (digits) | Number (integer) | There is no internal cast. The value is kept, only in the representation will include a comma before the last 2 digits. An old value of `156` will be represented as `1.56` |

### NOT NULL Constraint {#notnull}

A *NOT NULL* constraint can be set for simple fields (on top level and in nested fields), as well as for nested tables. *NOT NULL* is implemented in different ways.

For simple fields, the *NOT NULL* constraint is set in the database. This means this field can never be empty. This can cause a datamodel update to fail, if a new *NOT NULL* constraint is set, and there are already objects of this objecttype that would break the constraint.

The interpretation and implementation of the *NOT NULL* constraint is different for nested tables: for each object, there must be at least one row in the nested table. This is only checked by the Import API when objects are saved. This applies for the detail editor, the group edit mode and the JSON Importer.

The constraint is **not** checked in the following cases, so that empty nested tables are possible:

* Datamodel updates
* Deleting of linked objects in nested tables, which can cause empty nested tables

### Editable in link {#reverse-edit}

In contrast to the forward linking, where an easydb ID of the linked data record is stored on the main data record (as a reference to this data record), a data record is stored as part of another data record during editing in the linking, ie during backlinking.

```
Objecttype: Subject
Subject-ID    134
Title         Viele Bäume

  ^ Objekttyp: Term
  ^ Term-ID             89
  ^ Term-Subject-ID     134
  ^ Term-Title:         Wald

  ^ Objekttyp: Term
  ^ Term-ID             90
  ^ Term-Subject-ID     134
  ^ Term-Title:         Forest
```

In this simple example, we have created two object types: Subject and Term. A user can save several terms in the subject editor on a single subject. In the Term Editor, the forward link to the subject can be used to link exactly one subject. In backward linking, the linked records (2 terms) are fixed to the main record, ie. If the subject is re-stored with only one term, the no longer linked term is deleted. In the case of forward linking, only the linking would be solved, but not the data record itself. Backlinked objects behave like multiple fields which are fixed to the data record, but can also be edited and searched separately.

### Bidirectional {#bidirectional}

When storing links between two data sets, a direction of linking (forward or backward) is created. The direction is decisive for the visibility of the data records, depending on which side the record is viewed. In order to clarify an example, Fig. 198 is to be linked with Fig. 166 and Fig. Example 1: the linking is realized via a multiple field *Other images*. Example 2: the linking is realized with a separate object type *picture-to-picture*.

In both cases, the linking of image 166 or 168 to image 198 should now be visible.

#### Ex. 1: In a multiple field

For this purpose, the field *Other picture ID* is marked as *Bidirectional* in the first example. So easydb knows that a link should work in both directions. In the database, entries are generated for each direction, i. When storing image 198, the multi-field entries are stored automatically in the images 166 and 168, respectively.

##### Object Type: Picture

| Field | Data type | Editable in link | Bidirectional |
| ------ | ------- | ------ | ---- |
| Title | Text | | |
| Other images | Multifield | | |
| ↦ Other Picture ID | Picture | | X |

##### Data:

| Image ID | Title | Other image ID | |
| --- | --- | --- | --- |
| 198 | Test image 1 | 168 | |
| | | 166 | |
| 168 | Test image 1 | 198 | Link in the other direction (automatically generated) |
| 166 | Test image 1 | 198 | Link in the other direction (automatically generated) |

#### Ex. 2: In a separate object type

In the second example, the separate object type automatically creates records that link the other direction. By setting *Editable in Link*, the picture-to-picture entries of the linked image are edited in the editor of the main data record. If two links are added to the image 168 and 166, two records are automatically written for the link in the other direction. If you call the editor for image 166 or 168, the link to image 198 appears as an image-to-image entry.

##### Object Type: Picture

| Field | data type |
| --- | --- |
| Title | Text |

##### Object Type: Picture-to-Picture

| Field | Data type | Editable in link | Bidirectional |
| ------ | ------- | ------ | ---- |
| Image-From-ID | Image | X | X |
| Image-To-ID | Image | X | X |

##### Data:

**Object type: Picture**

| Image ID | Title |
| --- | --- |
| 198 | Test image 1 |
| 168 | Test image 2 |
| 166 | Test image 3 |

**Object Type: Picture-to-Picture**

| Picture-to-picture ID | Picture-From-ID | Picture-To-ID | Remarks |
| ---- | --- | --- | ---- |
| 9898078 | 198 | 166 | |
| 9898079 | 198 | 168 | |
| 9898080 | 166 | 198 | Link in the other direction (automatically generated)
| 9898080 | 168 | 198 | Link in the other direction (automatically generated)

### Bidirectional Reverse {#bidirectional-reverse}

The marking of a field as *Bidirectional Reverse* ensures that in a bidirectional context forward linking is provided with different attributes and thus typed bidirectional links arise. In the case of bidirectional, the link 1 is copied to 1, and in the case of bidirectional reverse, the link receives a distinctive attribute.

The device in the data model is the same for bidirectional and bi-directional reverse. In both cases, reference is made to the same table when linking.

> NOTE: For simple (top-level) fields, there must be two forward links; in the case of multiple fields (nested), a link to the top-level object type is sufficient.

| Objecttype: thing | type | example t1 | example t2 |
| --- | --- | --- | --- |
| Id: pkey | primary key, internal | 1 | 2 |

| Objecttype: thing_thing | type | example tt1 | example tt2 |
| --- | --- | --- | --- |
| Id: pkey | primary key, internal | 12 | 13 |
| Thing_from | forward link to thing, bidirectional | 1 | 2 |
| Thing_to | forward link to thing, bidirectional | 2 | 1 |
| Linktype | forwardlink to linktype | 8 | 9 |

| Objecttype: linktype | type | example l1 | example l2 |
| --- | --- | --- | --- |
| Id: pkey | primary key, internal | 8 | 9 |
| Linktype_reverse | forward link to linktype, bidirectional_reverse | 9 | 8 |
| Description | text | son of | parent of |

When *tt1* is inserted, *tt2* is generated automatically. *Tt1* contains as line type *l1*, easydb write the backlink as *l2* in *tt2*.
