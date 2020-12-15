# Traefik v2 use oauth2-proxy  on k3s demo



[TOC]

> we use traefik forwardAuth to implement this, 
I use traefik v2.3,  self modified oauth2-proxy version,  provider github oauth.

>
> 



##  forwardAuth behavior

>  The ForwardAuth middleware delegate the authentication to an external service. If the service response code is 2XX, access is granted and the original request is performed. Otherwise, the response from the authentication server is returned.

which means when we config forward address to /oauth2/auth,   any unauthentication request comming will return 401 directly,  but we expected to  redirect request to sign in page。

we can use  traefik  errors middleware to redirect 401 to /oauth/sign_in

I modified oauth2-proxy repo to add paht auth_sign_in to implement this。



## origin path redirect

 after auth success, we found the redirect path is '/'。

oauth2-proxy use header X-Auth-Request-Redirect to get origin redirect URL, but traefik use X-Forwarded-Uri to forwarded origin Uri,   so I modified oauth2-proxy repo to  get X-Forwarded-Uri。

 by default, traefik Forwarded header  X-Forwarded-Uri is disabled,   use --entryPoints.web.forwardedHeaders.insecure to enable。

a simple way is edit deploy traefik to add args on running。







