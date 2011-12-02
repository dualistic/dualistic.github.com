---
layout: post
title: "Case what?"
date: 2011-12-01 21:28
comments: true
categories: ['ruby','chef']
---

So, I was merging some changes (the postgres chef recipe), and I ran across the following:

``` ruby why would you do this?
case
when platform_version.to_f <= 5.0
  service_name "postgresql-#{node['postgresql']['version']}"
when platform_version =~ /squeeze/
  service_name "postgresql"
else
  service_name "postgresql"
end
```

I am confused.  Why would you do this?  What possible benefit does this have over just using an if/else/elsif block?  So... I'm going to just change it to:

``` ruby this makes more sense
if platform_version.to_f <= 5.0
  service_name "postgresql-#{node['postgresql']['version']}"
elsif platform_version =~ /squeeze/
  service_name "postgresql"
else
  service_name "postgresql"
end
```

Because that seems, you know, sensible...