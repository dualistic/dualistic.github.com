---
layout: page
title: "Mercurial, Apache2 and mod_rewrite"
date: 2008-04-01 18:57
comments: true
sharing: true
footer: true
---
So, "as previously mentioned":http://pol.llovet.name/2008/4/1/redmine-and-mercurial, I had gotten mercurial working in Redmine, but I was having trouble getting mercurial to work with apache2.  Part of the issue is that unlike most webserver environments, most RoR Apache2 -> Mongrel Cluster (Let's call this RAM from now on) setups are using mod_rewrite and mod_proxy to do URL jujitsu.  This can (and did) cause a bit of problem for me.

There are a bunch of tutorials online ("like this one":http://www.selenic.com/mercurial/wiki/index.cgi/PublishingRepositories) about how to publish your mercurial repos.  But if you are mod_rewriting all over the place, you need a couple extra steps.

h3. Tell mod_rewrite to ignore your hg directory

I had to add this directive at the beginning of my mod_rewrite declarations to ignore the hg directory:

```
        RewriteRule ^/hg(.*) - [PT]
```

My  apache conf blok looks like this (not dissimilar to the examples):

```
        # Setup HG (mercurial) location
        ScriptAliasMatch ^/hg(.*) /replicated/hg/hgwebdir.cgi$1
        <Directory /replicated/hg>
                Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
                AllowOverride None
                AuthUserFile /replicated/hg/.htpasswd
                AuthGroupFile /dev/null
                AuthName "MSU Forge Mercurial Repository"
                AuthType Basic
                <Limit POST PUT>
                        Require valid-user
                </Limit>
        </Directory>
```

So, it's working now, and that's lovely. :)  Now, i just wish I could tie the htaccess to the Redmine user tables and use that to Auth against...  Another day, another project...
