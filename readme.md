Some Old Source Code
===

*Before Github, programmers would put links to code samples in their resumés. Here are some samples I used to provide. Back in the day. Before electricity. And yes, this is the actual text I used to introduce each snippet at the time.*

**Interview Answer**

Erich Finkelstein introduced me to the "balancing rocks" interview question. It's a math puzzle of the style you'll find at <a href="http://www.techinterview.org/">techinterview.org</a>. The nice thing about rock balancing is that it isn't an "aha" problem: there are several solutions of varying quality, so candidates have a great chance to show off their smarts by asking smart questions and considering various aproaches.

I'm not going to describe the math puzzle, but lately I've been asking candidates to check the correctness of their answers the nerdy way: by writing a program that checks it for them (ok, I usually don't have a lot of time, so I ask the candidate to write the algorithm out). In adherence with the Golden Rule, here're a few versions I whipped up in a few minutes (each) that do the job.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=2>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/rocks.java">rocks.java</A>
		</TD>
		<TD>
			Brute force answer checking in Java.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/rocks.groovy.txt">rocks.groovy</A>
		</TD>
		<TD>
			A much better algorithm, this time in the <a href="http://groovy.codehaus.org/">Groovy</a>
			scripting language.
		</TD>
	</TR>
</TABLE>


**Java Temporal Entity Relationship Modelling Framework**

These classes are part of a personal research project. I don't want to say too much about the project: the point is to create a working application, not a research thesis. I arbitrarily grabbed a few source code files that I happened to be editing when I decided
asynchronously to update this page. They aren't complete and I guarantee they will have changed by the time you read this.

Anyways, the idea exposited by this source code is that this application is event-centric: instead of investing 80 percent of the architcture in the domain model and 20 percent in the transactions that modify the domain, this application invests 80 percent of the architecture in the events that cause changes to the domain ("Domain Events") and only 20 percent in modelling the domain itself.

For more information on this subject, try doing a web search for &quot;<a href="http://www.google.com/search?q=temporal+database">Temporal Database</a>&quot;. I'm grateful to Camiel Wolsing of CriSys Limited for suggesting this area of interest.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=2>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/magicbook/TestDomainEvents.java">TestDomainEvents.java</A>
		</TD>
		<TD>
			Preliminary HttpUnit tests for Domain Events.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/magicbook/DomainEventCommand.java">DomainEventCommand.java</A>
		</TD>
		<TD>
			&quot;Commands&quot; are like little servlets that live inside the UI and perform actions, something
			like Struts actions. These are not Domain Events: this particular command generates a Domain
			Event and attempts to implement its writes.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/magicbook/DomainEventFactory.java">DomainEventFactory.java</A>
		</TD>
		<TD>
			A preliminary factory that builds Domain Events and uses reflection to populate them. There is some
			preliminary pattern matching available as pre-validation of domain event parameters.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/magicbook/TDomainEvent.java">TDomainEvent.java</A><BR>
			   <A HREF="source/magicbook/TAbout.java">TAbout.java</A>
		</TD>
		<TD>
			Some handy interfaces. TAbouts are used for answering whether operations are allowed,
			   like rich error messages. TDomainEvents are events that take place in the subject
			   domain (in an HR application, think &quot;Hire&quot; not &quot;Add Row to Employee
			   Table&quot;).
		</TD>
	</TR>
</TABLE>

**Adding Continuations to the Java Programming Language**

These classes are part of another personal research project. My interest was in adding continuations to the Java Programming Language so that web applications could be written in Java using a Cocoon or Seaside style of programming. The benefit is that much of the session state management in a modern web application simply goes away, making software development simpler and more reliable. And as an added bonus, the new window, back button, and bookmarks all work all of the time, eliminating my major irritation with treating web browsers like a desktop GUI.

This &quot;Big Idea&quot; is that in a standard web application we cannot obtain information from the user in the middle of a method: we therefore cannot model a series of UI steps in a single method. Although some folks feel that isn't a good idea, the fact is that lots of web applications do have a series of sequential steps: try checking out of almost any e-commerce site.

The basis of the system is to rewrite Java methods so that they become 're-entrant': they become a loop enclosing a big switch statement, with all state stored in a big vector of parameters. The method can be restarted arbitrarily by passing in the current state and a label. The sample classes handle some of the Abstract Syntax Tree transformations required to flatten the tree into one level so that it can be rewritten as a switch statement.

The basis for the transformation is that the Java byte codes are parsed and mechanically transformed into an intermediate language. Certain special forms that would be keywords are represented as special static methods of a dummy class: those methods are turned into special forms in the intermediate language. In a tradition dating back to 1957, the intermediate language has an ascii representation featuring Lots of Interesting and Special Parentheses. That representation appears often in the test cases: it also makes it easy to decouple testing of the transformations from testing of the byte code parsing.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=2>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/worseisbetter/LetCallExpander.java">LetCallExpander.java</A>
		</TD>
		<TD>
			Certain patterns in the AST are rewritten to simplify optimization and transformation.
			A let/call/cc form is a place where a method can be restarted. In a web application, every
			form presented to the user is a restartable place in the computation. The let/call/cc special form
			names the restartable place so that a form can be submitted back to this place in the computation.
			This class transforms the code into a sequence of steps featuring a label that can become the 
			target of the enclosing switch statement.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/worseisbetter/CallExpander.java">CallExpander.java</A>
		</TD>
		<TD>
			The inverse of let/call/cc is a call statement. This is where one piece of code invokes a named
			continuation. The sample code transforms this into a restartable computation: it looks just like
			let/call/cc without the name. That's because the model of computation is that of coroutines
			that yield to each other: the process is symmetric.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/worseisbetter/YieldExpander.java">YieldExpander.java</A>
		</TD>
		<TD>
			The yield special form actually transfers execution to the named continuation. It is also
			expanded into a simple sequence.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/worseisbetter/SetBangBeginFlattener.java">SetBangBeginFlattener.java</A><BR>
			   <A HREF="source/worseisbetter/IfFlattener.java">IfFlattener.java</A>
			   <A HREF="source/worseisbetter/CompositeEntryFlattener.java">CompositeEntryFlattener.java</A>
		</TD>
		<TD>
			Lots of language constructs add scope to the AST. Flatteners rewrite the code using GOTOs so that it has a single
			scope with all of its variables hoisted.
		</TD>
	</TR>
</TABLE>

**Code Generation for Struts Development**

When working with large Struts development projects, managing changes to the struts-config file and your action classes is essential. I developed a collection of code generators in XSL that transformed a struts config file into a collection of action classes, interfaces, and business logic abstract base classes. This is useful if you have a tool automatically building the struts-config file from a source like a spreadsheet. Automatic generation saves a little coding time up front, but its real benefit is when the design goes through the inevitable refactoring: the classes are automatically updated and strong typing catches any places where business logic classes have not been updated. Thus, the combination of code generation and type checking makes refactoring easier.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=24*>
	<COL WIDTH=232*>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/actions.xsl">actions.xsl</A>
		</TD>
		<TD>
			In conjunction with a build step that applies this XSL sheet to the struts-config.xml file,
    this generates a collection of Action java classes automatically. Each action class forwards to a Delegate class that actually contains
    the business logic. The idea is that a number of different actions might be handled by the same logic class. A
    special attribute added to each element in the structs-config file identifies the appropriate Delegate.
    
    The Action classes are responsible for forwarding action requests. A nice side effect for strong, declarative
    typing enthusiasts is that each forwarding call is strongly typed, documenting and enforcing the proper form bean's type in Java.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/delegate-interfaces.xsl">delegate-interfaces.xsl</A><BR/>
			<A HREF="source/default-delegate.xsl">default-delegate.xsl</A>
		</TD>
		<TD>
			Custom attributes in the struts-config file identify the business logic delegate that is to
			handle each action. These xsl style sheets generate a Java interface and an abstract base class
			for each delegate. There is a many-to-one relationship between actions and delegates, so each
			delegate gets a <tt>handleActionName</tt> method for each action it handles. The style sheets
			ensure that the handler method takes the correct form parameter (if one is needed) and returns the
			correct bean (if one is neede for viewing).
			
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/IEntityDelegate.java">IEntityDelegate.java</A><BR/>
			<A HREF="source/ActionEntityDetailsGeneralTab.java">ActionEntityDetailsGeneralTab.java</A>
		</TD>
		<TD>
			Sample files generated automatically.
			
		</TD>
	</TR>
</TABLE>

**Denumerables in Ruby**

I experimented with the Ruby programming language. While it doesn&#146;t have the power of Lisp&#146;s macros, it does have first class closures (you can even use the lambda keyword) and support for continuations. All this in a true object oriented language with support for operator overloading! 

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=24*>
	<COL WIDTH=232*>
	<TR VALIGN=TOP>
		<TD WIDTH=9%>
			<A HREF="source/denumerable.txt">denumerable</A>
		</TD>
		<TD WIDTH=91%>
			A module for call-by-need collections. Useful for modelling
			infinitely large collections.<BR>There&#146;s a full explanation
			of <A HREF="http://www.braithwaite-lee.com/tips/denumerables.html">Denumerables
			in Ruby</A> <A HREF="http://www.braithwaite-lee.com/tips/denumerables.html"></A>
			<IMG SRC="http://www.braithwaite-lee.com/image/bullet/text.gif" NAME="Graphic65" ALIGN=BOTTOM WIDTH=9 HEIGHT=11 BORDER=0><A HREF="http://www.braithwaite-lee.com/tips/denumerables.html"></A>
			on this site, plus an essay on <A HREF="http://www.braithwaite-lee.com/tips/closures.html"><SPAN STYLE="font-weight: medium">Closures
			in Ruby</A> <A HREF="http://www.braithwaite-lee.com/tips/closures.html"></A>
			<IMG SRC="http://www.braithwaite-lee.com/image/bullet/text.gif" NAME="Graphic1" ALIGN=BOTTOM WIDTH=9 HEIGHT=11 BORDER=0><A HREF="http://www.braithwaite-lee.com/tips/closures.html"></SPAN></A><SPAN STYLE="font-weight: medium">.</SPAN>
		</TD>
	</TR>
</TABLE>

**Wireshooter in PHP**

<B>Wireshooter</B> was a database backed web site that published news and sports photos for download by magazines. Photographers uploaded their photos using a standard web browser, and then Wireshooter periodically rebuilt a static web site from its database. Here is the complete and uncommented source code, now available via Gnu Public License.

Basically, there were two servers: a public server running on an ISP&#146;s high speed line, and a private administration server running on our own system. The public server served up static files, and the private server ran Apache with mod_php. The files you see here all were on the private server.

The architecture was loosely MVC. The PHP pages accessible from the admin page were the controllers, the production site was the view, and the PHP include files were the model, handling control of the SQL database.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=46*>
	<COL WIDTH=210*>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			<A HREF="source/private/admin/index.html.txt">/private/admin/index.html</A>
		</TD>
		<TD WIDTH=82%>
			The administration page. This is the page I used to manage the
			site. The site was<SPAN STYLE="font-weight: medium"> running on a
			public server, but there was no access for the public whatsoever.</SPAN>
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			<A HREF="source/private/admin/resetdb.php.txt">/private/admin/resetdb.php</A>
		</TD>
		<TD WIDTH=82%>
			Oh no!
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			<A HREF="source/private/upload-image/sports.php.txt">/private/upload-image/sports.php<BR></A><A HREF="source/private/upload-image/filmfest.php.txt">/private/upload-image/filmfest.php</A>
		</TD>
		<TD WIDTH=82%>
			The site was divided into two main &#147;databases,&#148; one
			for sports images and one for filmfest images. Separate pages were
			used for uploading sports and filmfest images. It&#146;s easy to
			build a generalized upload page that can upload to any &#147;database,&#148;
			but this was easier for the photographer to manage.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			<A HREF="source/private/grind/grind.php.txt">/private/grind/grind.php</A>
		</TD>
		<TD WIDTH=82%>
			On a daily basis, you could &#147;grind&#148; the database out
			to static HTML files. This would also create scaled down previews
			of the images with copyright banners attached. Static files were
			jillions of times faster than dynamic pages, and disk space is
			free, even on the ISP&#146;s server. The static files would not be
			public, however you could navigate them all and make sure
			everything was Ok. This also protected the public server in case
			of a crash while &#147;grinding.&#148;
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			<A HREF="source/private/xml/mirror.xml.txt">/private/xml/mirror.xml</A>
		</TD>
		<TD WIDTH=82%>
			The structure of the production site was defined in this XML
			file.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			<A HREF="source/private/grind/mirror.php.txt">/private/grind/mirror.php</A>
		</TD>
		<TD WIDTH=82%>
			When you were satisfied with the static site, you would
			&#147;mirror&#148; it to the production site using FTP. This was
			very loosely M/VC: The work was done by
			<A HREF="source/private/grind/mirror-action.php.txt">/private/grind/mirror-action.php</A>.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			The PHP files that handled uploading images:
		</TD>
		<TD WIDTH=82%>
			<A HREF="source/private/upload-image/upload-action.php.txt">/private/upload-image/upload-action.php</A>,
			<A HREF="source/private/upload-image/sports.php.txt">/private/upload-image/sports.php</A>,
			<A HREF="source/private/upload-image/filmfest.php.txt">/private/upload-image/filmfest.php</A>
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=18%>
			The include files for the site:
		</TD>
		<TD WIDTH=82%>
			<A HREF="source/private/required/admin.php.txt">/private/required/admin.php</A>,
			<A HREF="source/private/required/blackboard-protocol.php.txt">/private/required/blackboard-protocol.php</A>,
			<A HREF="source/private/required/blackboard.php.txt">/private/required/blackboard.php</A><BR><A HREF="source/private/required/class.php.txt">/private/required/class.php</A>,
			<A HREF="source/private/required/error.php.txt">/private/required/error.php</A>,
			<A HREF="source/private/required/image-schema.php.txt">/private/required/image-schema.php</A><BR><A HREF="source/private/required/simplify.php.txt">/private/required/simplify.php</A>,
			<A HREF="source/private/required/sql.php.txt">/private/required/sql.php</A>,
			<A HREF="source/private/required/table-schema.php.txt">/private/required/table-schema.php</A><BR><A HREF="source/private/required/TAbstractRootPage.php.txt">/private/required/TAbstractRootPage.php</A>,
			<A HREF="source/private/required/TContext.php.txt">/private/required/TContext.php</A>,
			<A HREF="source/private/required/thumbnails.php.txt">/private/required/thumbnails.php</A><BR><A HREF="source/private/required/TMirrorGrinder.php.txt">/private/required/TMirrorGrinder.php</A>,
			<A HREF="source/private/required/types.php.txt">/private/required/types.php</A>,
			<A HREF="source/private/required/upload.php.txt">/private/required/upload.php</A><BR><A HREF="source/private/required/wiregrinder.php.txt">/private/required/wiregrinder.php</A>,
			<A HREF="source/private/required/wireshooter.php.txt">/private/required/wireshooter.php</A>,
			<A HREF="source/private/required/wiretemplates.php.txt">/private/required/wiretemplates.php</A>
		</TD>
	</TR>
</TABLE>

**Wireshooter in Java**

<B>Wireshooter</B> was a database backed web site that publisheed news and sports photos for download by magazines. Photographers uploaded their photos using a standard web browser, and then Wireshooter periodically rebuilt a static web site from its database. I wrote a prototype in Java, then re-wrote it using PHP. Here is a class from the Java prototype:

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=2>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="ColumnGrinder.java">ColumnGrinder.java</A>
		</TD>
		<TD>
			This class creates indices for the different values in a column
			of a table. For example, if the schema includes a column named
			&quot;Sport,&quot; this class builds indices for each sport.
			Compare this to the <A HREF="#PHP">PHP</A> version.
		</TD>
	</TR>
</TABLE>

**</A>MetaCard Configuration Wizard**

On a large and very successful software tool, we needed a wizard for configuring JProbe's complex scripts. I wrote a fully functioning wizard using MetaCard, a cross-platform implementation of Apple's old HyperCard authoring tool. It took me three days or so to write and debug the complete wizard. We didn't end up shipping the MetaCard wizard: for 'strategic' reasons the wizard was re-written from scratch in Java. And no, it didn't take three days to build and test the re-write.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=43*>
	<COL WIDTH=213*>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/wizard.mc.txt">wizard.mc.txt</A>
		</TD>
		<TD>
			Excerpts of the MetaCard script for the Configuration Wizard.
		</TD>
	</TR>
</TABLE>

**C++ Source Code**

These are some of the C++ general purpose classes in my 'toolbox'. There's a heavy &quot;Effective C++&quot; bias. This set is everything you need to implement Hash Sets where each object has a primary key (unique in the set) and a secondary key (not necessarily unique). To convert this to a Map (in Java terminology) or Dictionary (SmallTalk terminology), an adapter is used (the equivalent of Java's Map.Entry).

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=43*>
	<COL WIDTH=213*>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/StrictPointerT.h">StrictPointerT.h</A>
		</TD>
		<TD WIDTH=83%>
			A template for enforcing strict single ownership of C++
			objects. Compare <B>StrictPointerT</B> to <B>auto_ptr</B> in the
			Standard Library.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/AutoArrayT.h">AutoArrayT.h</A>
		</TD>
		<TD WIDTH=83%>
			A version of <B>auto_ptr</B> designed for arrays.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/DoubleHashTableT.h">DoubleHashTableT.h</A>
		</TD>
		<TD WIDTH=83%>
			A template for making hash tables out of objects with two keys.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/AbstractDoubleHashTable.h">AbstractDoubleHashTable.h</A>
			<BR><A HREF="source/AbstractDoubleHashTable.cpp">AbstractDoubleHashTable.cpp</A>
		</TD>
		<TD WIDTH=83%>
			The superclass for DoubleHashTables.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/DoubleHashable.h">DoubleHashable.h</A>
			<BR><A HREF="source/DoubleHashable.cpp">DoubleHashable.cpp</A>
		</TD>
		<TD WIDTH=83%>
			A mixin for objects to be placed in <B>DoubleHashTables</B>.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/Hashable.h">Hashable.h</A>
			<BR><A HREF="source/Hashable.cpp">Hashable.cpp</A>
		</TD>
		<TD WIDTH=83%>
			Mixin for objects to be placed in hash tables. I've got other
			code which implements single key hash tables and use this mixin as
			well.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/PrimaryAdapterT.h">PrimaryAdapterT.h</A>
			<BR><A HREF="source/PrimaryKeyAdapterT.h">PrimaryKeyAdapterT.h</A>
		</TD>
		<TD WIDTH=83%>
			Adapters which can take any object and manage it as a
			<B>DoubleHashable</B>.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/HashUtil.h">HashUtil.h</A>
			<BR><A HREF="source/HashUtil.cpp">HashUtil.cpp</A>
		</TD>
		<TD WIDTH=83%>
			Utility class implementing a string to hash code operation.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/DoubleHashableIteratorT.h">DoubleHashableIteratorT.h</A>
			<BR><A HREF="source/DoubleHashableIterator.h">DoubleHashableIterator.h</A>
			<BR><A HREF="source/DoubleHashableIterator.cpp">DoubleHashableIterator.cpp</A>
		</TD>
		<TD WIDTH=83%>
			Iterator template for <B>DoubleHashTable</B>. Iterates over
			elements with a given key. In the C++ style. Not MT safe, but safe
			for a mutable hash table.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/DoubleHashableAllIteratorT.h">DoubleHashableAllIteratorT.h</A>
			<BR><A HREF="source/DoubleHashableAllIterator.h">DoubleHashableAllIterator.h</A>
			<BR><A HREF="source/DoubleHashableAllIterator.cpp">DoubleHashableAllIterator.cpp</A>
		</TD>
		<TD WIDTH=83%>
			Iterator template for <B>DoubleHashTable</B>. Iterates over all
			elements. In the C++ style. Not MT safe, but safe for a mutable
			hash table.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/DeallocDoubleHashTableT.h">DeallocDoubleHashTableT.h</A>
			<BR><A HREF="source/DeallocDoubleHashTable.h">DeallocDoubleHashTable.h</A>
			<BR><A HREF="source/DeallocDoubleHashTable.cpp">DeallocDoubleHashTable.cpp</A>
		</TD>
		<TD WIDTH=83%>
			Template for DoubleHashTables which use pooled memory
			management.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/TaObjectPool.h">TaObjectPool.h</A>
			<BR><A HREF="source/TaObjectPool.cpp">TaObjectPool.cpp</A>
		</TD>
		<TD WIDTH=83%>
			Class which pools memory for objects of a certain type. Good
			for objects which are recycled often, and which maintain a
			relatively constant number of live objects.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="source/MemoryBlock.h">MemoryBlock.h</A>
			<BR><A HREF="source/MemoryBlock.cpp">MemoryBlock.cpp</A>
		</TD>
		<TD WIDTH=83%>
			<B>TaObjectPool</B> allocates blocks of 32 objects at a time,
			which are represented by <B>MemoryBlocks</B>.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=17%>
			<A HREF="http://www.braithwaite-lee.com/tips/fooT.html"><SPAN STYLE="font-weight: medium">FooT.h</SPAN></A>
		</TD>
		<TD WIDTH=83%>
			</A>Mildly obfuscated source code for a C++
			template. I give this to C++ developers and ask them for their
			comments.
		</TD>
	</TR>
</TABLE>

**Scheme/Lisp Web Application Platform written in Java**

Here's some Java I wrote when building NAVajo and Firestorm (both of which are 100% Pure Java). NAVajo was a system which built static web pages from HTML pages with an embedded scripting language called 'MendelScheme'.

Firestorm was a web application server which used a Scheme/Lisp dialect as a scripting language to build dynamic web applications.

To make it perfectly clear: I wrote the interpreters for both MendelScript and Firestorm in Java, and both of these were used to build production web applications. Today we have J2EE, JSPs, ASPs, and various other web application platforms. But in 1996 these applications were cutting edge.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=2>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="http://www.braithwaite-lee.com/opportunities/firestorm/DilutedInterpreter.java">DilutedInterpreter.java</A>
		</TD>
		<TD>
			A sparsely commented excerpt from Firestorm's source. This Java
			class is the core interpreter for the Scheme-based server side
			scripting environment at the heart of Firestorm. It is similar to
			<A HREF="http://www.cag.lcs.mit.edu/curl/wwwpaper.html">Curl</A>.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/AbstractMember.java">AbstractMember.java</A>
		</TD>
		<TD>
			An interface from NAVajo (Firestorm's predecessor). Glancing
			over it quickly, you can see that at the time I was enamored of:
			naming conventions; heavy use of the envelope idiom (thus the
			<B>yourself</B> method); container inheritance (analogous to event
			handling in a GUI); and dynamic typing (the <B>strategy </B>idiom).
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/MemberBuilder.java">MemberBuilder.java</A>
		</TD>
		<TD>
			An implementation of the <B>factory</B> pattern in NAVajo.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/TEnglish.html.txt">TEnglish.html</A>
		</TD>
		<TD>
			An early template page for NAVajo, showing the Smalltalk-like
			syntax. NAVajo renders template files like this into HTML. Compare
			to <A HREF="source/firescript/navajo-scheme.lisp">navajo-scheme.lisp</A>.
		</TD>
	</TR>
</TABLE>

**Scheme Source Code**

The following examples are Scheme used to test the core Firescript interpreter. They were displayed on a virtual command line and do not reflect any web server functionality:

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=2>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/bad-recursive.lisp">bad-recursive.lisp</A>
		</TD>
		<TD>
			A recursive implementation of <B>factorial</B> which is <B>not</B>
			tail recursive.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/factorial.lisp">factorial.lisp</A>
		</TD>
		<TD>
			A properly tail recursive implementation of <B>factorial</B>.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/no-tail-factorial.lisp">no-tail-factorial.lisp</A>
		</TD>
		<TD>
			An implementation of <B>factorial</B> which is <B>not</B> tail
			recursive.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/benchmark.lisp">benchmark.lisp</A>
		</TD>
		<TD>
			Something which chews up enough cycles to be worth timing.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/continuation.lisp">continuation.lisp</A>
		</TD>
		<TD>
			A test for <B>call-with-continuation</B>. Enough to convince me
			that a 30% performance hit wasn't enough to justify the awesome
			elegance of continuations. [Note from 2002: The problem was my
			implementation: I was not clever enough to make continuations as
			cheap as a Long Jump.]
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/freeze-and-thaw.lisp">freeze-and-thaw.lisp</A>
		</TD>
		<TD>
			Demonstrates the power of combining macros with raw lambdas to
			implement algol thunks.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/hello.lisp">hello.lisp</A>
		</TD>
		<TD>
			Disk space is cheap.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/let.lisp">let.lisp</A>
		</TD>
		<TD>
			Tests the <B>let</B> macro. Especially important, because the
			implementation changed several times. In proper Scheme, let has a
			specific macro definition. Originally, I had a custom handler for
			let because I hadn't implemented macros properly but I wanted to
			use let. Then, I implemented macros and made let a macro. Finally,
			I implemented a lot of optimizations to ensure that let wasn't a
			performance hit. 
			
			This is why I'm aggressive about regression testing. Over the
			course of a development cycle, implementations can change quite a
			bit, and it's crucial to know when and if you inadvertently break
			something.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/map.lisp">map.lisp</A>
		</TD>
		<TD>
			Another regression test. <B>map</B> changed from being a built
			in function to being a generalized variable.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/even.lisp">even.lisp</A>
		</TD>
		<TD>
			Determines whether an integer is even. Puzzle for people new to
			Lisp: is this tail 'recursive'?
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/no-tail-even.lisp">no-tail-even.lisp</A>
		</TD>
		<TD>
			An implementation of <B>even?</B> which is <B>not</B> tail
			recursive.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/stack-inversion-example.lisp">stack-inversion-example.lisp</A>
		</TD>
		<TD>
			Tests the <B>generalized variable</B> facility in Firescript.
			Borrowed from Common Lisp.
		</TD>
	</TR>
</TABLE>

Firescript Source Code
---

The following examples are from the Firestorm application server. Firestorm was a web application server I wrote in 1997 which used a Scheme-based scripting language to build dynamic, user-centric web applications.

**XML and User Preferences**
  
This is part of a small module that demonstrates how to track user preferences. In this case, users of an online application can indicate whether they prefer English or French text, and whether they prefer graphics or text only (the demonstration was composed when 56K modems were uncommon):

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=33*>
	<COL WIDTH=223*>
	<TR VALIGN=TOP>
		<TD WIDTH=13%>
			<A HREF="source/firescript/sartz.xml">sartz.xml</A>
			<BR><A HREF="source/firescript/about.dtd.txt">about.dtd</A>
		</TD>
		<TD WIDTH=87%>
			An XML file containing language attributes. This module
			rendered the XML as HTML.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=13%>
			<A HREF="source/firescript/display-preferences.lisp.txt">display-preferences.lisp</A>
		</TD>
		<TD WIDTH=87%>
			This displays a preferences page for the user. Notable features
			are: recursively embedding Scheme and HTML inside each other; use
			of <B>this-session</B> dictionary and generalized variables; and
			the ability to 'call' this page from any other page.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=13%>
			<A HREF="source/firescript/sartz.lisp">sartz.lisp</A>
		</TD>
		<TD WIDTH=87%>
			The Firescript page which handles the display of <B>sartz.xml</B>.
			This follows the &quot;MVC&quot; pattern: the logic is embedded in
			this page, while the model is embedded in the XML page. And of
			course, the XML could be 'repurposed' in other forms.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=13%>
			<A HREF="source/firescript/validate-preferences.lisp">validate-preferences.lisp</A>
		</TD>
		<TD WIDTH=87%>
			A helper function encapsulated in a separate file. Shows that
			you can structure Firescript applications on a server.
		</TD>
	</TR>
</TABLE>

**NAVajo update**

The NAVajo system was a predecessor to Firestorm. I reimplemented the application in Firescript to demonstrate the newer, more Scheme-like syntax. Of course, Firescript offered much more power than the original &quot;MendelScheme&quot; scripting language.

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=56*>
	<COL WIDTH=200*>
	<TR VALIGN=TOP>
		<TD WIDTH=22%>
			<A HREF="source/firescript/navajo-scheme.lisp">navajo-scheme.lisp</A>
		</TD>
		<TD WIDTH=78%>
			An implementation of NAVajo using Firescript. Compare to
			<A HREF="source/TEnglish.html.txt">TEnglish.html</A>.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=22%>
			<A HREF="source/firescript/navajo-indexed.lisp">navajo-indexed.lisp</A>
		</TD>
		<TD WIDTH=78%>
			An alternate implementation which uses indexed collections for
			performance reasons.
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=22%>
			<A HREF="source/firescript/navajo-dictionaries.lisp">navajo-dictionaries.lisp</A>
		</TD>
		<TD WIDTH=78%>
			Yet another alternate implementation, this one using
			dictionaries for collection access.
		</TD>
	</TR>
</TABLE>

**Displaying Dynamic Collections**

This is part of a small module that demonstrates how to display collections of things which can change. In this case, photos of Bryan Donald Bartle are dropped into a folder and Firestorm displays them in a crude photo book:

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=3>
	<COL WIDTH=14*>
	<COL WIDTH=242*>
	<TR VALIGN=TOP>
		<TD WIDTH=5%>
			<A HREF="source/firescript/List.lisp">List.lisp</A>
		</TD>
		<TD WIDTH=95%>
			The file which completely describes the list of images. Note
			that (unlike the previous example) display code is intermingled
			with hard coded information about where to find the photos and
			what they are about. This demonstrates that Firestorm supported
			hacking out quick solutions <I>as well as</I> structured designs
			:-)
		</TD>
	</TR>
	<TR VALIGN=TOP>
		<TD WIDTH=5%>
			<A HREF="source/firescript/detail.lisp">detail.lisp</A>
		</TD>
		<TD WIDTH=95%>
			This displays a single photo in a simple page.
		</TD>
	</TR>
</TABLE>

**SmallTalk Binary**

The only SmallTalk I can lay my hands on is an implementation of Streamized Programming for DigiTalk SmallTalk I wrote in 1983. Good luck filing it into your image!

<TABLE WIDTH=100% BORDER=1 CELLPADDING=2 CELLSPACING=2>
	<TR VALIGN=TOP>
		<TD>
			<A HREF="source/VSTRM.CLS">VSTRM.CLS</A>
		</TD>
		<TD>
			A class which implements <B>streamized programming</B>, which
			uses delayed evaluation to allow extremely high level abstraction
			of infinite streams such as 'integers', 'primes', &amp;tc. Save it
			to your system and file it in.
		</TD>
	</TR>
</TABLE>