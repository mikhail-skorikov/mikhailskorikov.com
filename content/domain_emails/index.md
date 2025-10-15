+++
title = "How to set up email addresses for your own domain for cheap"
date = 2025-09-18
authors = ["Mikhail Skorikov"]
description = "Easy (and cheap!) setup guide to your own domain email addresses."
[taxonomies]
tags = ["DNS", "Email", "Migadu"]
[extra]
toc_sidebar = true
+++


Recently I have noticed that some clients who require emails for their own domain become aghast at the cost of it. 

While most email hosts provide a variety of features to justify the costs and even give some free mailboxes or free trial duration deals to reel clients in, there are some providers which are actually quite worth the costs. The one I am going to talk about is [Migadu](https://migadu.com). 

This hidden gem of a provider has free trials and for a small price of $19 per year, with reasonable limits, you get unlimited (practically) domains and mailboxes for them, with 5 GB storage which is enough for a moderate number of domains and mailboxes. The other pricing plans are even more liberal in how much you can do with it but for a basic setup, $19 per year is pocket change compared to most popular options.

To set up your own email addresses for your domain, you need to sign up for Migadu and have access to manage your domain's DNS records.

## Step 1: Create domain in Migadu
Once you have signed up for Migadu, log in to your account and under Email Domains, go to New Domain


{{ image(url="new_domain.png", alt="This is an image", no_hover=true) }}



Add in your domain name, selecting external nameservers as the default. 

Accept the default addresses (admin, postmaster, abuse) creation as they are part of the email standards, although they're not required.


{{ image(url="domain_form.png", alt="This is an image", no_hover=true) }}



## Step 2: Add DNS records to your domain

The setup instructions are provided by Migadu when you add your domain to Migadu with values automatically set for you to copy-paste. Go to your domain registrar, and open up its DNS management page.

To give an example of how easy it is to configure your DNS records based on Migadu instructions is:

- First is a verification TXT record for uniquely identifying your domain: 
	```
	Record Type | Name/Address | Value
	     TXT    |      @       | hosted-email-verify=<verification_code>
	```

- Then add MX records to route addresses to Migadu host:
	```
	Record Type | Name/Address | Value             | Priority
	    MX      |      @       | aspmx1.migadu.com | 10
	    MX      |      @       | aspmx2.migadu.com | 20
	```

- Add DKIM + ARC key records for authenticating the email's integrity:
	```
	Record Type | Name/Address    | Value
	   CNAME    | key1._domainkey | key1.<domain_name>._domainkey.migadu.com.
	   CNAME    | key2._domainkey | key2.<domain_name>._domainkey.migadu.com.
	   CNAME    | key3._domainkey | key3.<domain_name>._domainkey.migadu.com.
	```

- Add SPF Record of type TXT and DMARC record which is designed to prevent email forging in combination with DMARC:
	```
	Record Type | Name/Address    | Value
	   TXT      |      @          | v=spf1 include:spf.migadu.com -all
	   TXT      |   _dmarc        | v=DMARC1;
				      | p=quarantine; 

	```

There are some optional features for some clients and service discovery which you can set up similarly following the Migadu instructions if you need those. 

## Step 3: Verify!

Once the records are added, click on the Check Configuration button near the top of the instructions page in Migadu, which will probably send you to the login page due to session timeout. That is okay. After logging in again, you'll either be told it's already up, because Migadu checks the domain records after accessing domain setup page again, or you'll see that some of the records are updated while others are not. That is also fine, if you have added the records correctly. DNS record propagation takes time, so check back in a few minutes, or hours if it is taking longer - it varies how long it takes but can take up to a day or more. 

## Step 4: Add email addresses

Once your domain is configured though, you can go to Mailboxes > New Mailbox and create an email address as you want to call it, and as you want to authenticate it. Set an initial password, or send password setting link to an email, it's up to you!


{{ image(url="domain_form.png", alt="This is an image", no_hover=true) }}



## That's it!

And that's it. You can log into the email address from [Migadu Webmail](webmail.migadu.com). That part is not explicitly stated if you set your initial password, but email invitations have instructions to do so.


You can also use your own email clients to set up email receiving and sending as an alternative to Migadu Webmail. Migadu has guides on how to set those up [here](https://migadu.com/guides/).


