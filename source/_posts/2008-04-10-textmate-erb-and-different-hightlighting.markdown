---
layout: page
title: "Textmate ERB <%= and <% different hightlighting"
date: 2008-04-10 23:54
comments: true
sharing: true
footer: true
---
I can't be the only person that has run into the accidental no-output from erb tags.  Let's use textmate's syntax highlighting to solve this problem (or at least make it less obnoxious).
<!--more--> 
Ok, so I have been bitten several times when I do something like:

```
Your username is: <% username %>
```

Why is my username blank?  This is a simple example, but I have run into this a number of times.  It's like a missing semicolon problem, but ERB instead of PHP or C.  Strangely enough, even if you specify a puts method, you still get no output:

```
Your username is: <% puts username %>
```

(This also will give you nothing)

You need a <%= opening in order to print out anything.  So, it would be nice if Textmate colored them differently.  I went on a quest to make this happen... and failed.

Ultimately I ended up in the #textmate channel on freenode.  They were very helpful and pointed me to [this replacement grammar for textmate](http://rafb.net/p/DVN7Qh27.html), which for the sake of being complete, I've included Right here:  

```
{   scopeName = 'text.html.ruby';
    fileTypes = ( 'rhtml', 'erb', 'html.erb' );
    foldingStartMarker = '(?x)
        (<(?i:head|body|table|thead|tbody|tfoot|tr|div|select|fieldset|style|script|ul|ol|form|dl)\b.*?>
        |<!--(?!.*-->)
        |\{\s*($|\?>\s*$|//|/\*(.*\*/\s*$|(?!.*?\*/)))
        )';
    foldingStopMarker = '(?x)
        (</(?i:head|body|table|thead|tbody|tfoot|tr|div|select|fieldset|style|script|ul|ol|form|dl)>
        |^\s*-->
        |(^|\s)\}
        )';
    patterns = (
        {   name = 'comment.block.erb';
            begin = '<%+#';
            end = '%>';
            captures = { 0 = { name = 'punctuation.definition.comment.erb'; }; };
        },
        {   name = 'source.ruby.rails.embedded.substituted.html';
            begin = '<%+(?!>)=';
            end = '-?%>';
            captures = { 0 = { name = 'punctuation.section.embedded.ruby'; }; };
            patterns = (
                {   name = 'comment.line.number-sign.ruby';
                    match = '(#).*?(?=-?%>)';
                    captures = { 1 = { name = 'punctuation.definition.comment.ruby'; }; };
                },
                {   include = 'source.ruby.rails'; },
            );
        },
        {   name = 'source.ruby.rails.embedded.html';
            begin = '<%+(?!>)[-=]?';
            end = '-?%>';
            captures = { 0 = { name = 'punctuation.section.embedded.ruby'; }; };
            patterns = (
                {   name = 'comment.line.number-sign.ruby';
                    match = '(#).*?(?=-?%>)';
                    captures = { 1 = { name = 'punctuation.definition.comment.ruby'; }; };
                },
                {   include = 'source.ruby.rails'; },
            );
        },
        {   include = 'text.html.basic'; },
    );
}
```

So, here's how to do it:

1. In Textmate, go to Bundles > Bundle Editor > Edit Languages (or CTRL-OPT-CMD-L)
2. Click to the Ruby on Rails > HTML (Rails) Language grammar, the code will appear in the window to the right.
3. Replace the code with the code up above.
4. Click Test, if you like to make sure it is good.  Take note of the "name" of two of the rules: @source.ruby.rails.embedded.html@ and @source.ruby.rails.embedded.substituted.html@
5. Close the Bundle Editor
6. I think it's easiest to see what you are editing, so open up an erb or rhtml file so you can see what you are fiddling with.  After you have opened a file, continue on.
7. Go to TextMate > Preferences (or hit CMD - ,)
8. Click on "Fonts & Colors"
9. In the "Element" list, click the '+' to add a new element.  Call it something descriptive, I called it "Ruby HTML Output".  In the "Scope Selector", type or paste in @source.ruby.rails.embedded.substituted.html@.  Set the foreground and background color to be what you want.  Be careful, if there's no direction, it will use the default foreground and background.  If you do click and choose a color, I couldn't figure out how to un-choose and use default again. :(
10. Optionally, you can do the same thing and add another Element for the non-output tag (I made it less saturated than the other one).

And that's it!  Yay for solutions!
