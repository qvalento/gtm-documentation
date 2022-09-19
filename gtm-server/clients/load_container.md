# Custom loading of container
This client is used to load a GTM container from your website without doing it the Google way. Why would we want to do this? Google is already providing an alternative with the server container to send to your own host intead of googletagmanager.com. Well there are actually a very simple explanation. No matter if you implement GTM the standard way or with the Google Server way, you will still be blocked by common ad blockers. They can easily see that a request is being sent with a GTM ID in the payload. 

## How our solution works
Our solution solves this by sending the request completely without identifiers for containers. This will be set up by you in the server container instead. You can add multiple container IDs on the server and reference them by adding an index in the GTM script as a query parameter. The first container on the server will be referenced by 0. 

## Script modifications
To make this work, you will have to modify the standard GTM script a bit. 

### Remove the `<noscript>` tag in body
To be honest, this probably doesn't do anything good on your website anyways. It only allows a very few tags, since it only kicks in if the user has disabled JavaScript.

### Rewrite the script in `<head>` to the following:
```
<script>
    (function(q, v, a, l, e, n, t, o) {
        q[l] = q[l] || [];
        q[l].push({
            'gtm.start': new Date().getTime(),
            event: 'gtm.js'
        });
        var f = v.getElementsByTagName(a)[0],
            j = v.createElement(a),
            n = '?i=' + e,
            t = l != 'dataLayer' ? '&l=' + l : '',
            o = n + t;
        j.async = true;
        j.src =
            'https://[YOUR_CUSTOM_SERVER_HOST]/tm' + o;
        f.parentNode.insertBefore(j, f);
    })(window, document, 'script', 'dataLayer', 0);
</script>
```
  
  
## Debugging
When debugging the container, a request will be sent to googletagmanager.com so make sure to turn off ad blockers in those cases.

## Are you interested in getting it installed?
Reach out to linus.larsson@qvalento.com. 
