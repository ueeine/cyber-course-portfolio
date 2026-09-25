# Darknet Diaries: LinkedIn Breach Assignment

## 1. The incident in your own words

The hacker searched for a victim on LinkedIn because it's easy to find a target there. You can easily find someone with all their information, since most LinkedIn users put all their info: where they're working now, where they worked in the past, and where they're planning to work in the future.

The hacker found a victim, and that victim's friend had a website, and he used a file on the website, when you send a file to a website, the file goes to the server, basically like sending a virus to the server, and the server injects it. It was easy, and the hacker hacked into the home computer. He found the iMac and escaped the virtual machine, then brute forced the password of the iMac. He found the private key to the server just sitting there unprotected, along with the VPN profile to LinkedIn, and he found all the private information that normal people are not allowed to see, like address or emails.

He made a mistake, he used his own IP address, which was in Moscow, and they didn't know about this for over 3 months. It wasn't until 2016 that the full number came out: 117 million accounts, not just the 6.5 million first reported.

## 2. Who was affected, and how

Most of LinkedIn's users were affected. Most of the users' passwords got stolen, millions of passwords were stolen. LinkedIn said to be aware and told users to change their passwords to stay secure, because they might be affected. The hacker stole data in the breach, passwords and users' information.

## 3. The CIA principle

It's confidentiality. Private data got exposed, emails, usernames, and password hashes. Most users had bad passwords too, like "1234," because LinkedIn only required a short minimum length back then.

This also messed with people's trust in LinkedIn, not just their data. After finding out their password wasn't safe there, people probably trusted the site less. And since a lot of people use the same password on more than one site, this breach could've put their other accounts at risk too.

## 4. The technique and the weak hashing decision

Let's say I made a LinkedIn account and made my password "password1234." If it just went straight to the database and was saved as-is on the server that saves passwords, imagine if someone malicious gained access to it, he could steal it, use it, or even sell it. That's why hashing exists, it was hashing and something called SHA-1. If a hacker steals a database of hashed passwords, he can't just see the real passwords, he has to use a tool to try to crack them. In LinkedIn's case, the hacker already got the database breached, and the hashes let him crack passwords faster.

## 5. The slow surfacing of the data

Stolen data doesn't go straight to the public. Instead, it goes to people who buy it, and then they sell it to other people who use it, or maybe publish it, depending on what they're trying to do. Imagine you could sell the same stolen data to multiple people, that would make you rich, since you're selling one thing to hundreds of people instead of just one buyer.

That's basically what happened with LinkedIn. The hack happened in March 2012, but the full number, 117 million accounts, didn't come out until 2016. That's a four-year gap. During that time, the data was probably being sold around privately like that, to different buyers, before it ever became public.

## 6. What could have helped, defending the organization

LinkedIn didn't secure the web enough, and they didn't secure their servers well either. They should have used an encrypted system to secure passwords, so even if hackers stole the data, it wouldn't be easy to crack, or maybe even impossible to crack.

## 7. The broader lesson: credential reuse and downstream attacks

LinkedIn didn't secure the web enough, and they didn't secure their servers well either. They should have used an encrypted system to secure passwords, so even if hackers stole the data, it wouldn't be easy to crack, or maybe even impossible to crack.

What could have helped is if people didn't use stupid passwords, and instead used a password manager. I understand it's easy to log into your account with a simple password, but that makes you an easy target too.

## 8. Your personal takeaway

To be honest, I would use the same password if it's long and hard to crack, but I wouldn't use the same password if I use a password manager, since all I have to do is log into my main account, and whenever I want to log into a site, it automatically pastes the password for me. But the catch is, if I try to log into my account on a different device, I couldn't remember my passwords.
