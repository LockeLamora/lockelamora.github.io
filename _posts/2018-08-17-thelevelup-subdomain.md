---
layout: post
title: Thelevelup.com subdomain takeover
tags: Live
---
## Rescuing neglected subdomains and giving them a new home

I was looking at  [Thelevelup's](https://www.thelevelup.com/security-response) responsible disclosure policy and it looks good. No unreasonable restrictions that only ties up whitehats and lets blackhats go free, and even a hall of fame and a bit of swag as reward. yay! (FYI: they don't update the hall of fame and there's no swag, but for legal practice and something to pad out a blog with while we wait for bigger finds to be resolved by other companies it suits us)

I was cruisin' for a bruisin' around their subdomains and one in particular looked abandoned; facebook.thelevelup.com was pointing to a cloudfront default page.

This occurs when you point one of your domains to a cloudfront instance by CNAME. Then you forget about it and let the cloudfront account die, what I can do now is re-register that cloudfront domain and I now have part of your site pointing to a page I control. Normally what someone would do here is put in fake logon boxes etc to start stealing your customer credentials in phishing expeditions with a perfectly legitimate URL, especially one attached to another recognisable brand like facebook, Personally I would even have a fake facebook login popup here and get two sets of credentials for the price of one.

So let's make our first check; is it pointing to cloudfront via a CNAME entry? let's check using dig:

[<img src="{{ site.baseurl }}/images/thelevelup/1.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/thelevelup/1.png)

success! Now here isn't the time to get complacent; Sometimes when you try to register the cloudfront domain it's still taken by the original company, so it's still abandoned but safely out of our grasp, so we go to AWS and see if we can get it up or if we get an error message:

[<img src="{{ site.baseurl }}/images/thelevelup/2.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/thelevelup/2.png)

Once that goes without a hitch we can point it to wherever we want, in my case I just pointed it to this blog but for a bigger company I'd probably aim it at a non-descript message that can't be found accidentally so I can submit a very specific link to the company.

[<img src="{{ site.baseurl }}/images/thelevelup/3.png"
   style="width: 800px;"/>]({{site.baseurl }}/images/thelevelup/3.png)

As you can see the address bar always contains the proper website URL, so even the more savvy users can be caught out by something fake being hosted here.

So that's always an issue company's need to keep an eye out for, and something a lot let slide.
