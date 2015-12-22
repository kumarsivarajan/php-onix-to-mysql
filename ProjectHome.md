This script reads an onix file in parts and processes it to insert it into several tables in mysql.

If the tables or collums do not exist, the script will create them. An empty database will suffice.

At the moment I have only one supplier that provides me with complete databases only.

So help on this project is more then welcome to add for example a script that will process updates, or simple help to make this script faster and better etc.<br>
<b>So at this moment updates do not work</b>, if alterate onix files need to be processed, make sure there are no records in it, that are in the database. If there are recoreds in the database that are also in the onix, they need to be (manually) removed from the database, or duplicates will be created.<br>
<br>
<b>To start using this script is rather simple.</b><br>
There are a few settings to adjust to your situation:<br>
<pre><code>$file = "~/onix.xml"; // Location of onix file<br>
$dbhost = "localhost"; // mysql host<br>
$dbuser = "my_username"; //mysql username<br>
$dbpw = "my_password"; // mysql user password<br>
$db = "my_database"; // mysql database name<br>
</code></pre>

"~/onix.xml" should point to your onix file, if it is in the same directory as the script then the name on the onix file wil be enough.<br>
dbhost generally is localhost so probably can remain as is.<br>
The script does need your username, password and database in order to insert things into your database.<br>
<br>
<h2>Say thank you to the devolper of this script</h2>
I'd like to know that people like this script.<br>I am not placing a donate button to donate money, but instead I'd like to ask you people to help fight malaria.<br>
Doing so is rather easy, the explanation can be found <a href='Donate_CPU_time.md'>here</a>.<br>
<br>
<h2>if you get some errors</h2>
<a href='PageName.md'>Too many redirects error</a><br>
<a href='PageName.md'>Minor speed improvement</a>

<br><a href='http://code.google.com/p/php-onix-to-mysql/issues/detail?id=2#c3'>Some fields are not imported</a><br>
However, the tag that breaks your xml might be different. In the linked example it are several things, <b>html within xml</b> tags and <b>variables assigned to tags</b>.<br>
<br>
HTML inside tags should always be turned into its html special chars equivalent.<br>
<br>
Variables assigned to tags should be either ignored or be turned into their own tag.<br>
<br>
This issue actually has everything to do with <a href='http://php.net/manual/en/function.simplexml-load-string.php'>simple_xml_load_string</a> breaking on the provided xml.<br>
If this script breaks on this function, then your xml is not supported by this function.<br>Now we can do two things, either post a feature request/bug report about this php function, or we can change our xml to be compatible. The last is probably the easiest to achieve.<br>Support on how to automatically prepare your xml to be compatible with this function can be aquired on most web development forums.<br>
You could for example post a question on <a href='http://www.phpfreaks.com/forums/index.php?board=1.0'>phpfreaks.com</a> like:<br>
<br>
<b>Help wanted to make xml work with simplexml_load_string</b>

<blockquote>This xml tag is not compatible:<br>
<pre><code>replace this with the tag not being (completely) imported<br>
&lt;MyTagNotImported myvariable="02"&gt;bla bla bla &lt;html tag&gt; bla &lt;/html tag&gt; bla bla &lt;/MyTagNotImported&gt;<br>
</code></pre></blockquote>

<blockquote>How can I use preg_replace to change this to be compatible with simplexml_load_string?<br>
html should be turned into its html_special_chars equivalent, and variables in xml tags should be (turned into their own xml tag, or be ignored) (up to you if you find the variable usefull or not).<br>
This tag is part of string stored as $p.</blockquote>

The answer can be inserted into <a href='http://code.google.com/p/php-onix-to-mysql/issues/detail?id=2#c3'>this answer</a>, by replacing:<br>
<pre><code>$p = preg_replace("!&lt;br /&gt;!", "&amp;#60;br /&amp;#62;", $p);<br>
$p = preg_replace('!&lt;(.*?) (.*?)="(.*?)"&gt;!', '&lt;\\2&gt;\\3&lt;/\\2&gt;&lt;\\1&gt;', $p);<br>
</code></pre>
with the answer given on the forum, something like:<br>
<pre><code>$p = preg_replace("!&lt;br /&gt;!", "&amp;#60;br /&amp;#62;", $p);<br>
$p = preg_replace('!&lt;(.*?) (.*?)="(.*?)"&gt;!', '&lt;\\2&gt;\\3&lt;/\\2&gt;&lt;\\1&gt;', $p);<br>
</code></pre>