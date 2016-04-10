---
layout: post
title: "history.pushState for Jekyll in a few lines of code"
description: ""
category: code-snippet
tags: [code-snippet, random-gif]
---
{% include JB/setup %}

You may have noticed the fast page loads here. This site uses `history.pushState` combined with AJAX, which I hacked on to Jekyll. In case you don't want to inspect element, here's the code:

```javascript
window.reparseLinks = function(){
    if(window.hpsEnabled){
        var as = document.getElementsByTagName("a");
        for(var i = 0; i < as.length; i++){
            if(as[i].hostname == "aputurk.tk"){
                as[i].onclick = window.onhpslink; // this makes all inter-page links use AJAX loading
                                                  // which overrides navigation
            }
        }
    }
}

window.postload = function(){
    document.getElementById("loader").style.opacity = 0;
    reparseLinks();
        
    // handle the search box that comes up on the 404 page
    // (that uses https://github.com/christian-fei/Simple-Jekyll-Search)
    // I use a MutationObserver there to handle any links that come up there
}

window.hpsEnabled = !!history.pushState; // can we use history.pushState?
                                         // if not, leave links as they are and use normal page loading

window.loadPage = function(pathname){
    var to = setTimeout(function(){
        document.getElementById("loader").style.opacity = 1;
    }, 200);
    var xhr = new XMLHttpRequest();
    xhr.onload = function() {
        clearTimeout(to);
        var title = this.responseXML.title;
        var res = this.responseXML.getElementById("page-wrap").innerHTML;
        document.title = title;
        history.replaceState({}, title, pathname);
        
        // handle analytics
        
        // here's where we actually insert the loaded content
        document.getElementById("page-wrap").innerHTML = res;
        
        window.postload();
        
        // handle disqus widget
    }
    xhr.open("GET", pathname);
    xhr.responseType = "document";
    xhr.send();
}

window.onhpslink = function(e){
    if(this.hostname == "aputurk.tk"){
        e.preventDefault();
        history.pushState({}, "", this.pathname + this.hash);
        window.loadPage(this.pathname + this.hash);
    }
}

if(window.hpsEnabled){
    window.onpopstate = function(){
        window.loadPage(location.pathname);
    }
}
window.postload();
```

Because we can't change what Jekyll serves, it's impossible to prevent loading the entire HTML for a page over AJAX. But using `history.pushState()` allows you not to have to reload all the random scripts and styles and background images that come with a normal page load. For me, the pages usually load so fast, they don't trigger the loading animation that displays after 200ms of loading.

---

On a more off-topic note: here's the GIF I made for Zro's April 1st "maymays" thread on the A(O)Ts, if anyone wants it:  
![Thisandagain-themed deal with it GIF](http://i63.tinypic.com/10ef1ad.gif)