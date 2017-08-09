# Caching

## Why

Originating from VOC from our Prime application, there was an issue regarding users’ my-account data still being visible from their browser’s cache even after the user has logged out and clicked back. A solution is live in production for Prime. One concern that is currently being monitored are the drawbacks of clearing the cache. This is important as the issue currently exists in the RA module integrated with Prime.

The challenge is to consider a similar solution for within the React side of my-account.


## What

The bottom-line is that the user should not see their private information after logging out should they click on the “Back” button of their browser. A solution is live in Prime (as of early July, 2017) and must be considered in RA. It is currently present in parts of RA (see team Nutella or details below).

The negative side-effects are currently being monitored in Prime (particularly performance). 


## How

The solution that is currently in production for Prime is to disable caching by adding “no-cache” and “no-store” directives in the headers from within our Prime_Controller.php. The 5 lines of code added at the head of Prime_Controller.php are:

header("Cache-Control: no-store, no-cache, must-revalidate");  
header("Cache-Control: post-check=0, pre-check=0", false);   
header("Expires: Sat, 26 Jul 1997 05:00:00 GMT");  
header("Pragma: no-cache");  
header("Last-Modified: " . gmdate("D, d M Y H:i:s") . " GMT");

In RA, team Nutella has implemented the solution:
          
// set headers to prevent browser caching  
res.set('Cache-Control', 'no-cache, max-age=0, must-revalidate, no-store');



## References

- [Architecture Forum Issue #67] (https://github.com/telusdigital/architecture-forum/issues/67)
- [Implementation Example in RA] (https://github.com/telusdigital/my-account-favourite-numbers/blob/master/ui/src/server.js#L180)