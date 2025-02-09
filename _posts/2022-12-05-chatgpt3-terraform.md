---
layout: post
title:  "Musings Around ChatGPT"
date:   2022-12-05 06:39:32 -0400

categories: Musings AI ChatGPT Automation
---

A lot of people in the internet are playing around with Chat GPT at the moment (myself included!). One of the more fun examples, is to ask it to create a python script to automate some task

![Chat GPT generating a python script to get financial data](/assets/chat-gpt-python-script.png)

You can even have it write pretty decent shell code

![Same script as above but in Bash](/assets/chap-gpt3-shell-code.png)

This is really cool stuff, but what if I ask it to write terraform?
Here, I ask Chat GPT to generate terraform to create an EC2 instance in AWS

![Request chat gpt to create an EC2 instance in terraform](/assets/chat-gpt3-terraform-ec2-instance.png)

This is fine on first glance, but let's take a closer look. Where does `ami-0b69ea66ff7391e80` come from?

A quick look in the AWS console clearly shows this isn't a public image - so what did it scrape to get this?
Was this something Chat GPT generated? Is this a private AMI on some github gist in its dataset?

![AWS AMI console](/assets/personal-aws-acct-ami.png)

We're lucky that this is a very specific string and can be easily googled with not too many [hits](https://www.google.com/search?q=%22ami-0b69ea66ff7391e80%22&source=hp&ei=C4mOY5XpEcGl5NoP2I60CA&iflsig=AJiK0e8AAAAAY46XGzuZUMRkrsQUsLmhru9R-Me5XtQ8&ved=0ahUKEwjVmoLV2uP7AhXBElkFHVgHDQEQ4dUDCAk&uact=5&oq=%22ami-0b69ea66ff7391e80%22&gs_lcp=Cgdnd3Mtd2l6EAMyBwgAEB4QogQyBQgAEKIEMgUIABCiBDIFCAAQogRQAFjECmDVDWgAcAB4AIABgAGIAY8CkgEDMi4xmAEAoAEBoAEC&sclient=gws-wiz).
If you scroll a bit further down on the Google results page, one comes across a `gitter.im` link for `aws-cli`    
Parsing through this [chat](https://gitter.im/aws/aws-cli?at=5d6fbde58a83ef34a318cedb) eventually leads you to . . .

![Comment that has the ami tag](/assets/aws-ami-doc.png)

Aha, so this was an old AMI that used to be referenced in the AWS documentation, but has since been removed. That's how Chat GPT knows about it.     
If you read the documentation, the data sets stop at around 2021, which is _probably_ when that image was removed from the public catalogue

The power behind Chat GPT (and AI, in general) for writing code is clear. It can build fantastic scaffolding and help you get "Good Enough" very quickly, but it seems to suffer from the same problems as googling answers does, which is parsing through old information. Still, this is something that could be rectified by updating the data sets

All told, this is an incredibly powerful tool and I can't wait to see the ~~horrible things that man creates with it~~ the fascinating ways AI is used and how the technology grows