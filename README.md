# CFML Tag to Script Conversions
#####A collection of examples demonstrating the conversion of CFML code blocks written in tags to CFScript.

**Currently a work in progress!**

> This is not the CFScript syntax documentation you're (possibly) looking for. That awesome piece of work, by Adam Cameron, can be found [here](https://github.com/adamcameron/cfscript) and is highly recommended as a reference to the content you'll find here.

The examples in this document are an attempt to demonstrate conversions of CFML tag-based code to script-based code as a way of learning / migrating to CFScript. It is assumed that you already have a relatively firm understanding of the ColdFusion Markup Language (at least on a tag-based level). For the sake of modernity, the converted examples will be written to comply (from an agnostic approach) with the most current versions of the various CFML engines at the time of writing - (ColdFusion 11, Railo 4.2 & Lucee 4.5).

## Table of Contents
1. [The Modern Implementation of Tags to Script](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#the-modern-implementation-of-tags-to-script)
2. [Comments](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#comments)
3.  [Variables](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#variables)
4. [Operators](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#operators)
 - [Decision](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#decision)
 - [Increment / Decrement](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#increment--decrement)
 - [Inline Assignment](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#inline-assignment)
 - [Boolean](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#boolean)
 - [Ternary & Null-Coalescing](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#ternary--null-coalescing)
5. [Conditionals](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#conditionals)
 - [if / else if / else](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#if--else-if--else)
 - [Switch](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#switch)
 - [Try / Catch / Finally](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#try--catch--finally)
6. [Iterations](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#iterations)
 - [Index Loop](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#index-loop)
 - [Array Loop](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#array-loop)
 - [Struct Loop](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#struct-loop)
 - [List Loop](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#list-loop)
 - [Query Loop](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#query-loop)
7. [Misc Tags to Script](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#misc-tags-to-script)
 - [cfinclude, cflocation, cfabort & cfexit](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#cfinclude-cflocation-cfabort--cfexit)
 - [cfquery](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#cfquery)
 - [cfsavecontent](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#cfsavecontent)
 - [cflock, cfthread & cftransaction](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#cflock-cfthread--cftransaction)
8. [Tags Implemented as Components](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#tags-implemented-as-components)
 - [cfquery / query.cfc](https://github.com/cfchef/cfml-tag-to-script-conversions/blob/master/README.md#cfquery--querycfc)

### The <em>Modern</em> Implementation of Tags to Script

I want to open with this before diving into the meat of conversions. Adobe ColdFusion 11, Railo 3.2(?) and Lucee 4.5 all offer some kind of full script syntax support (Railo & Lucee sharing the implementation). Adobe ColdFusion 11 rolled it's own variation but Railo and Lucee both support ACF's syntax as of versions 4.2 and 4.5 respectively.

As per [Adam Cameron's CFScript Documentation](https://github.com/adamcameron/cfscript/blob/master/cfscript.md#the-rest):
> To use any other functionality not listed here within CFScript, one needs to use the generalised syntax.

> On Railo/Lucee this is a matter of removing the "`<cf`" and the "`>`", and using normal block syntax (curly braces) where the tag-version is a block-oriented tag.

> On ColdFusion, replace the "`<cftagname`" with "`cftagname(`", and the "`>`" with "`)`", and comma-separate the attributes. Note that this will make the construct look like a function, but it actually is not, and cannot be used like a function, eg this is invalid syntax:

> `result = cfhttp(method="post", url="http://example.com");`

### Comments

_**Tags:**_
```coldfusion
<!--- I'm a single-line comment. --->

<!---
I'm a multi-
line comment.
--->
```

_**Script:**_
```coldfusion
<cfscript>

// I'm a single-line comment.
// Notice that CFScript code within a ".cfm" file must be wrapped in <cfscript> tags.

/*
I'm a multi-
line comment.
*/

</cfscript>
```

### Variables

_**Tags:**_
```coldfusion
<!--- Some simple variable statements in tags --->

<!--- Default variable declarations --->
<cfparam name="title" default="">
<cfparam name="views" type="numeric" default=0>

<!--- Regular assignment --->
<cfset title = "Tags";
<cfset views = 1;
<cfset page = "Title: #title#, Number: #views#">
```

_**Script:**_
```coldfusion
<cfscript>

// Some simple variable statements in script

// Default variable declarations
param name="title" default="";
param name="views" type="numeric" default=0;

// Regular assignment
title = "Script";
views = 2;
page = "Title: #title#, Number: #views#";
// or
page = "Title: " & title & ", " & "Number: " & views;

</cfscript>
```

### Operators

#### Decision

_**Tags:**_
```coldfusion
<cfset x = 1>
<cfset y = x EQ 1>
<cfset y = x NEQ 1>
<cfset y = x LT 1>
<cfset y = x GT 1>
<cfset y = x LTE 1>
<cfset y = x GTE 1>
```

_**Script:**_
```coldfusion
<cfscript>

// All tag based operators still work in script
// There are also these equivalents

x = 1;
y = x == 1;
y = x != 1;
y = x < 1;
y = x > 1;
y = x <= 1;
y = x >= 1;

</cfscript>
```

#### Increment / Decrement

_**Tags:**_
```coldfusion
<cfset x = x + 1>
<cfset x = x - 1>
```

_**Script:**_
```coldfusion
<cfscript>

x = x++; // x = x + 1;
x = x--; // x = x - 1;

</cfscript>
```

#### Inline Assignment

_**Tags:**_
```coldfusion
<cfset x = x + 3>
<cfset x = x - 3>
<cfset x = x / 3>
<cfset x = x * 3>
<cfset x = x % 1>
<!--- or --->
<cfset x = x MOD 1>
```

_**Script:**_
```coldfusion
<cfscript>

// x = x + 3;
x += 3;
// x = x - 3;
x -= 3;
// x = x / 3;
x /= 3;
// x = x * 3;
x *= 3;
//x = x % 3;
x % 3;

</cfscript>
```

#### Boolean

_**Tags:**_
```coldfusion
<cfset NOT x>
<cfset x EQ 1 AND y EQ 2>
<cfset x EQ 1 OR y EQ 2>
```

_**Script:**_
```coldfusion
<cfscript>

!x;
x == 1 && y == 2;
x == 1 || y == 1;

</cfscript>
```

#### Ternary & Null-Coalescing

_**Tags:**_
```coldfusion
<!--- Ternary --->
<cfset x = "">
<cfset y = len(x) ? x : "something else">

<!--- Null-Coalescing --->
<cfset y = z ?: "something else">
```

_**Script:**_
```coldfusion
<cfscript>

// Ternary
x = "";
y = len(x) ? x : "something else";

// Null-Coalescing
y = z ?: "something else";

</cfscript>
```

### Conditionals

#### if / else if / else

_**Tags:**_
```coldfusion
<cfset count = 10>
<cfif count GT 20>
	<cfoutput>#count#</cfoutput>
<cfelseif count EQ 8>
	<cfoutput>#count#</cfoutput>
<cfelse>
	<cfoutput>#count#</cfoutput>
</cfif>
```

_**Script:**_
```coldfusion
<cfscript>

count = 10;
if (count > 20) {
	writeOutput(count);
} else if (count == 8) {
	writeOutput(count);
} else {
	writeOutput(count);
}

</cfscript>
```

#### Switch

_**Tags:**_
```coldfusion
<cfset fruit = "">
<cfswitch expression="#fruit#">
    <cfcase value="Apple">
        <!--- Some apple stuff --->
    </cfcase>
    <cfcase value="Orange">
        <!--- Some orange stuff --->
    </cfcase>
    <cfcase value="Kiwi">
        <!--- Some kiwi stuff --->
    </cfcase>
    <cfdefaultcase>
        <!--- Some default stuff --->
    </cfdefaultcase>
</cfswitch>
```

_**Script:**_
```coldfusion
<cfscript>

fruit = "";
switch (fruit) {
	case "Apple":
		// Do apple stuff
	break;
	case "Orange":
		// Do apple stuff
	break;
	case "Kiwi":
		// Do kiwi stuff
	break;
	default:
		// Do default stuff
	break;
}

</cfscript>
```

#### Try / Catch / Finally

_**Tags:**_
```coldfusion
<cftry>
	<cfset x = y + z>
	<cfcatch type="any">
		<cfdump var="#cfcatch#">
	</cfcatch>
	<cffinally>
		<cfoutput>Finally at the end.</cfoutput>
	</cffinally>
</cftry>
```

_**Script:**_
```coldfusion
<cfscript>

try {
	x = y + z;
}
catch(any e) {
	writeDump(e);
}
finally {
	writeOutput("Finally at the end");
}

</cfscript>
```

### Iterations

#### Index Loop

_**Tags:**_
```coldfusion
<cfloop index="i" from="1" to="10">
	<cfoutput>#i#</cfoutput>
</cfloop>
```

_**Script:**_
```coldfusion
<cfscript>

for (i = 1; i <= 10; i++) {
	writeOutput(i);
}

</cfscript>
```

#### Array Loop

_**Tags:**_
```coldfusion
<!--- Define our array --->
<cfset myArray = ["a", "b", "c"]>

<!--- By index --->
<cfloop index="i" from="1" to="#arrayLen(myArray)#">
	<cfoutput>#myArray[i]#</cfoutput>
</cfloop>

<!--- By array --->
<cfloop index="currentIndex" array="#myArray#">
	<cfoutput>#currentIndex#</cfoutput>
</cfloop>

<!--- By arrayEach() --->
<cfset arrayEach(myArray, function(element, index) {
	<cfoutput>#element# : #index#</cfoutput>
})>
```

_**Script:**_
```coldfusion
<cfscript>

// Define our array
myArray = ["a", "b", "c"];

// By index
// Note the use of the newer member function syntax for arrayLen()
for (i = 1; i <= myArray.len(); i++) {
	writeOutput(myArray[i]);
}

// By array
for (currentIndex in myArray) {
	writeOutput(currentIndex);
}

// By arrayEach()
myArray.each(function(element, index) {
	writeOuput(element & " : " & index);
});

</cfscript>
```

#### Struct Loop

_**Tags:**_
```coldfusion
<!--- Define our struct --->
<cfset myStruct = {name: "Tony", state: "Florida"}>

<!--- By struct --->
<cfloop item="currentKey" array="#myStruct#">
	<cfoutput><li>#currentKey# : #myStruct[currentKey]#</li></cfoutput>
</cfloop>

<!--- By structEach() --->
<cfset structEach(myStruct, function(key, value) {
	<cfoutput><li>#key# : #value#</li></cfoutput>
})>
```

_**Script:**_
```coldfusion
<cfscript>

// Define our struct
myStruct = {name: "Tony", state: "Florida"};

// By struct
for (currentKey in myStruct) {
	writeOutput("<li>#currentKey# : #myStruct[currentKey]#</li>");
}

// By structEach()
myStruct.each(function(key, value) {
	writeOutput("<li>#key# : #value#</li>");
});

</cfscript>
```

#### List Loop

_**Tags:**_
```coldfusion
<!--- Define our list --->
<cfset myList = "a, b, c">

<!--- By list --->
<cfloop index="item" list="#myList#">
	<cfoutput>#item#</cfoutput>
</cfloop>

<!--- By array --->
<cfloop index="currentIndex" array="#listToArray(myList, ",")#">
	<cfoutput>#currentIndex#</cfoutput>
</cfloop>

<!--- By listEach() --->
<cfset listEach(myList, function(element, index) {
	<cfoutput>#element# : #index#</cfoutput>
}, ",")>
```

_**Script:**_
```coldfusion
<cfscript>

// Define our list
myList = "a, b, c";

// By array
for (item in listToArray(myList, ",")) {
	writeOutput(item);
}

// By listEach()
myList.each(function(element, index) {
	writeOuput(element & " : " & index);
}, ",");

</cfscript>
```

#### Query Loop

_**Tags:**_
```coldfusion
<!--- Define our query --->
<cfset platform = ["Adobe ColdFusion", "Railo", "Lucee"]>
<cfset myQuery = queryNew("")>
<cfset column = queryAddColumn(myQuery, "platform", "CF_SQL_VARCHAR", platform)>

<!--- By row index --->
<cfloop index="i" from="1" to="#myQuery.recordCount#">
	<cfoutput><li>#myQuery["platform"][i]#</li></cfoutput>
</cfloop>

<!--- By group --->
<cfloop query="myQuery" group="platform">
	<cfoutput><li>#platform#</li></cfoutput>
</cfloop>
```

_**Script:**_
```coldfusion
<cfscript>

// Define our query
platform = ["Adobe ColdFusion", "Railo", "Lucee"];
myQuery = queryNew("");
column = queryAddColumn(myQuery, "platform", "CF_SQL_VARCHAR", platform);

// By row index
for (i = 1; i <= myQuery.recordCount; i++) {
    writeOutput("<li>#myQuery["platform"][i]#</li>");
}

// By query
for (row in myQuery) {
    writeOutput("<li>#row.platform#</li>");
}

</cfscript>
```

### Misc Tags to Script

#### cfinclude, cflocation, cfabort & cfexit

_**Tags:**_
```coldfusion
<!--- <cfinclude> --->
<cfinclude template="mypage.cfm">

<!--- <cflocation> --->
<cflocation url="mypage.cfm" addToken="false" statusCode="301">

<!--- <cfabort> --->
<cfabort statusError="My error message">

<!--- <cfexit> --->
<cfexit method="method">
```

_**Script:**_
```coldfusion
<cfscript>

// <cfinclude>
include "mypage.cfm";

// <cflocation>
location("mypage.cfm", "false", "301");

// <cfabort>
abort "My error message";

// <cfexit>
exit "method";

</cfscript>
```

#### cfquery

_**Tags:**_
```coldfusion
<cfquery name="myQuery" datasource="myDSN">
	SELECT myCol1, myCol2 FROM myTable
	WHERE myCol1=<cfqueryparam value="#myId#" cfsqlype="cf_sql_integer">
	ORDER BY myCol1 ASC
</cfquery>
```

_**Script:**_
```coldfusion
<cfscript>

// Also see the "Tags Implemented as Components" section for another method of using <cfquery> in script

myQuery = queryExecute(
	"SELECT myCol1, myCol2 FROM myTable
	WHERE myCol1=?
	ORDER BY myCol1 ASC",
	{myid: 5},
	{datasource = "myDSN"}
);

</cfscript>
```

#### cfsavecontent

_**Tags:**_
```coldfusion
<cfsavecontent variable="myContent">
	<cfoutput>Some content.</cfoutput>
</cfsavecontent>
```

_**Script:**_
```coldfusion
<cfscript>

savecontent variable="myContent" {
	writeOutput("Some content.");
}

</cfscript>
```

#### cflock, cfthread & cftransaction

_**Tags:**_
```coldfusion
<!--- <cflock> --->
<cflock timeout="60" scope="session" type="exclusive">
	<cfset session.myVar = "Hello">
</cflock>

<!--- <cfthread> --->
<cfthread action="run" name="myThread">
	<!--- Do single thread stuff --->
</cfthread>
<cfthread action="join" name="myThread,myOtherThread" />

<!--- <cftransaction> --->
<cftransaction>
<cftry>
	<!--- code to run --->
	<cftransaction action="commit" />
	<cfcatch type="any">
		<cftransaction action="rollback" />
	</cfcatch>
</cftry>
</cftransaction>
```

_**Script:**_
```coldfusion
<cfscript>

// <cflock>
lock timeout="60" scope="session" type="exclusive" {
	session.myVar = "Hello";
}

// <cfthread>
thread action="run" name="myThread" {
	// do single thread stuff
}
thread action="join" name="myThread,myOtherThread";

// <cftransaction>
transaction {
	try {
		// code to run
	        transaction action="commit";
	}
	catch(any e) {
		transaction action="rollback";
	}
}

</cfscript>
```

### Tags Implemented as Components

#### cfquery / query.cfc

_**Tags:**_
```coldfusion
<cfquery name="myQuery" datasource="myDSN">
	SELECT myCol1, myCol2 FROM myTable
	WHERE myCol1=<cfqueryparam value="#myId#" cfsqlype="cf_sql_integer">
	ORDER BY myCol1 ASC
</cfquery>
```

_**Script:**_
```coldfusion
<cfscript>

qry = new Query().setSQL("
	SELECT myCol1, myCol2 FROM myTable
	WHERE myCol1=:myId
	ORDER BY myCol1 ASC
");
qry.addParam(name: "id", value: "#myId#", cfsqltype: "cf_sql_integer");
qry = qry.execute().getResult();

</cfscript>
```

## LICENSE
> <a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">CFML Tag to Script Conversions</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://tonyjunkes.com/leave-your-tags-at-the-door-cfml-tag-to-script-conversions" property="cc:attributionName" rel="cc:attributionURL">Tony Junkes</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.
