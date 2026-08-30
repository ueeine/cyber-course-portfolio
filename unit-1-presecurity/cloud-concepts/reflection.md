
# Entry: Cloud Concepts Reflection (U1-04b)

## Goal
I went through the U1-04a Cloud Concepts lecture and this is my reflection on it, connecting it to stuff I might actually run into in an entry-level IT job.

## Findings

**1. Cloud in your own words**
Basically instead of buying your own computer and keeping it at home or at work, you're renting space and processing power on someone else's computers, and you only pay for how much you actually use. It's like renting a storage unit instead of building your own garage - you get access whenever you want, you can ask for more space or less space at any time, and you never have to see or touch the actual machines. The main difference from just buying a computer is speed and flexibility, you don't have to buy new hardware and wait for it to arrive if you suddenly need more power.

**2. Traditional → Cloud → Containers**
The way I see it, every step in this chain is just about using hardware more efficiently and getting things running faster. First everyone had one computer running one operating system, which wasted a lot of the machine's power. Then virtualization let you split one machine into several pretend computers, cloud took that idea and let a company rent it out to anyone over the internet, and containers went even further by not needing a full operating system for every single app. Each step also added something new to worry about though - virtualization added more complexity to manage, cloud made you depend on a vendor, and containers need tools like Kubernetes just to keep track of everything.

**3. Deployment vs Service models**
These two things sound similar but they answer different questions. Deployment model is about where the servers physically are and who else is using them (on your own site, private, public, hybrid, or spread across multiple providers), while service model is about how much of the setup you have to manage yourself versus the vendor doing it (IaaS, PaaS, SaaS). A normal example most people use every day is Gmail or Google Docs - it's public cloud because the hardware is shared with a ton of other users, and it's SaaS because you just open it and use it, you never touch anything underneath.

**4. The Shared Responsibility Model**
This one is basically saying that even when you move everything to the cloud, security doesn't just become the vendor's problem. The vendor takes care of the physical stuff like the datacenter, the servers, and the hypervisor, but you as the customer always have to take care of your own data and identity/access no matter which service model you're using. A real example of this going wrong is the Capital One breach in 2019, where the actual cause was a misconfigured firewall that let an attacker get into an S3 storage bucket - AWS itself wasn't hacked, it was Capital One's own setup mistake. So the point is, moving to cloud changes what you're responsible for, it doesn't make the responsibility disappear.

**5. Why organisations still hesitate**
One reason is regulation, especially GDPR here in the EU/Finland, since some data legally has to stay in certain regions and that limits what cloud setup a company can even use. A completely different reason is that some companies have already spent a lot of money on their own servers and don't want to throw that away, plus they don't want to get stuck depending on one cloud vendor for everything (vendor lock-in). These two reasons are not really related to each other, one is a legal thing and the other is more of a money/practical thing.

**6. Cloud in an entry-level tech role**
If I ended up doing helpdesk work, a normal ticket would be something like a user saying they can't log into their Microsoft 365 account, and that's basically a cloud identity problem, not something wrong with their PC. As a junior sysadmin type role, you might get an alert saying there was a weird sign-in attempt on someone's cloud account from a strange location, and knowing that identity is always the customer's job (from the Shared Responsibility thing) is what makes that alert actually make sense instead of just being confusing.

**7. Your personal takeaway**
Honestly the biggest thing for me is realizing that "the cloud" isn't some magic floating thing, it's just a real building somewhere full of servers, which makes stuff like GDPR and data residency actually make sense instead of just being rules on paper. I'm also thinking I might look into doing the AZ-900 cert at some point this year since it doesn't need any experience to start and a lot of what I'd be doing at a helpdesk or junior sysadmin job seems to be about cloud identity stuff anyway.
