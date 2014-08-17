---
layout: page
title: About
permalink: /about/
---

```python
#!/usr/bin/env python

class Me(Developer) :

    @property
    def education(self):
        return {
            "Undergraduate" : ["Saftey Engineering","Shenyang Aerospace University"],
            "Graduate"      : ["Soft Engineering","Liaoning University"]}

    @property
    def characteristics(self):
        return set(["Teamwork", "Devoted", "Creative"])

    @property
    def skills(self) :
        return {
            "Language"  : ["Python", "Java", "HTML/CSS", "C/C++"],
            "OS"        : ["Linux"] ,
            "Tool"      : ["Vim", "Sublime"],
            "Framework" : ["Django"],
            "Network"   : ["TCP/IP"]}

    @property
    def is_geek(self):
        return True
```
