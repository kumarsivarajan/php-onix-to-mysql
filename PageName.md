one question so far:
# Too many redirects error #

Upon trying to process large onix files you can get this error.
For executing this script from a browser you can replace the serverside redirect with a javascript redirect.

**replace:**
```
header("Location: ".$uri."?start=".($end+$start-($deleted+1))."&st=".$st."&totaal=".$totaal);
```
**with:**
```
header("Refresh:0;url=".$uri."?start=".($end+$start-($deleted+1))."&st=".$st."&totaal=".$totaal);
echo 'Progress: '.number_format((($end+$start-($deleted+1))/$size)*100, 1) . '%';
```

If you wish to run this script from a cron then try using a tool that allows you to specify the maximum number of redirects.<br>
A tool that works on all platforms is <a href='http://curl.haxx.se/download.html'>curl</a>. (from command prompt / console / batch / bash)<br>
<pre><code>curl.exe -L --max-redirs 99999 http://url_to_your_server/path/to/script/onix.php<br>
</code></pre>

<b>or as an alternative you could increase the $mem value.</b>


one tweak:<br>
<h1>Minor speed improvement</h1>
This script as is runs with 393 records per second on my server, the here mentioned tweak in my case improves this to 409 records per second. A 4.1% increase in performance.<br>
It is achieved by setting the unique record id directly instead of looping through each productidentifier. In my case I get a field "RecordReference" that contains the ISBN. There is only one such tag, at highest level, that I can assign directly, where there can be many productidentifiers that I cannot assign directly.<br>
<b>For direct assigning replace:</b>

<pre><code>if(isset($produc-&gt;productidentifier)){<br>
   foreach($produc-&gt;productidentifier as $value) { <br>
      if($value-&gt;b221=='03' || $value-&gt;b221=='15') $id = mysql_real_escape_string($value-&gt;b244);<br>
   }<br>
} else {<br>
   foreach($produc-&gt;ProductIdentifier as $value) { <br>
      if($value-&gt;ProductIDType=='03' || $value-&gt;ProductIDType=='15') $id = mysql_real_escape_string($value-&gt;IDValue);<br>
   }<br>
}<br>
</code></pre>

<b>with:</b>
<pre><code>$id = mysql_real_escape_string($produc-&gt;RecordReference);<br>
</code></pre>

where <code>$produc-&gt;RecordReference</code> should point to a unique value per record