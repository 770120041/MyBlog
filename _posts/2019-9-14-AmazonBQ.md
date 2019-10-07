---
layout: post
title:  Amazon Interview
categories: Interview
---


# OA2: Bridges in a graph
[ArticulationPoints](https://www.geeksforgeeks.org/articulation-points-or-cut-vertices-in-a-graph/)
[All Bridge](https://www.geeksforgeeks.org/bridge-in-a-graph/)

# STAR
S – Situation - background info

T – Task - what your job was in this situation

A – Activity - what you did - this should be the longest part of the answer

R – Result - positive; quantifiable; what you learned; what you would do differently next time

## 1. Why Amazon
The reason why I’m willing to join Amazon is because Amazon has the best technology products available. It is really impressive when I was using AWS for my course project. I was workign with other classmates for setting a website, I found that AWS has all the tools I need, like database, elastic cache, Elastic Load Balancing and they are built so good that it is very easy to set them up and enjoy the convenience of using AWS. With AWS it is easy to build a scalable website that I can never imagine.

Also, in terms of impacting people’s lives, amazon has the e-commercial platform that facilites billions of people in the world. People can buy everything from Amazon, watch amazon prime videos and enjoying the amazon music. 

My career is young and I believe that Amazon offers an exceptional place for me to learn and gain experience while adding value to people’s lives each day.

## 2. Tell Me About A Time You Failed
C++ QT


## 3. Your strength
I think my strength is that I am hard workindg and devoted. For example, when I am doing my intern, I was very devoted because I think what I was doing was really interested, and also I think that my job will really be helpful to the team, so I would usually work till 7 or 8 every day so I was able to finish the project 2 weeks in advance.

## 4. Your Weakness
In the past, Procrastination was a huge challenge for me. I’d get jobs done in the last moment. Later, I realized this was hindering my progress when I am working with my homework or working in group projects.So I decided to trying to plan ahead and pushing myself to finishing the job before deadline. After I sticking to my plans and finishing all the jobs in advance, I would say that I am starting to enjoy finishing things earlier. 

Most recently, When I was working at Citrix as a summer intern, I was able to finish the project in only 8 weeks, much faster than every one expected before I plan everything in advance and try my best to finish it quickly. And my manager and mentor even said they were so surpised that I finished it so quickly that they can't believe that I havn't learned CI pipelines before.

## Citrix Intern Experience
When I was working at Citrix as a software engineer intern, I was working with them as a team, so I was doing the same projects they are doing. So the thing I was working was building a CI/CD pipeline.
The team I was working on was called Provisioning Service and they have a huge product with more than 1Gb of source code. And the they have dozens of building scripts and testing scripts, but they don't have a CI pipeline, so the first thing I did is was to decide what kind of CI tools to use. And finnaly I decided to use Jenkins and Docker as the CI tools. And the next thing I did was to 
set up Jenkins server, set up Jenkins Slaves and set up Webhook from GitLab to Jenkins. Then I try to trigger all the build scripts using Jenkins file, after the sequential build succeed. 
I used Docker and Virtual machines to set up environments for build varification test. The most challenging part of this intern was to speed up the building process.
Because according to the plan I shall finish it in 10 weeks, but it took me only 6 weeks to build a working pipeline. So I spent another 2 weeks workign on how to speed up the building process,
I tried seperate the building process and find ways to build the scripts in parallel, I using 5VMs each time to see if the parallel build pass the varification test. And after 2 weeks of trying, I was able to
build the product efficiently and correctly. And the building time can be reduced from 3 hours to about 45 minuts. 

#### What you would do differently?
The think I think I would do differently is to using a Powershell scirpt or Python scirpt rather than Jenkinsfile to call the build scirpt, the reason is that for jenkins, the Jenkinsfile is triggered and executed by the Jenkins Server, not the actually slave that building the product, so it is very tricky to query the environments of the building slaves. The reason why I would prefer not using Jenkinsfile is because the requirement was to build different versions of the product according to the .Net version and Visual Studio version, so they can use same code to build different versions. So I need to qeury the environment variables to decided to target version.

## 4. Tell Me About A Time When You Overcame An Obstacle
When I was taking the course computer networks, the professor told us to implement a reliable file transmission protocol, which shares similiar properties to TCP. The requirements was that it needs to be 30% as fast as the origin TCP implementation.

But after I tried to implemented the TCP sliding windows and simplest congestion control algorithm, it still didn't meets the requirement. So I tried to analyse why my implementation is still not efficient, finnaly I found it is because the network has more than 35% of  packet loss,
So the usual congestion control algorithm is not optimized for such kind of networks, so the thing I did next was to make the size of the sliding window larger, so it can have more packets
send at the same time thus when a packet loss is detected, it is faster to resend, also, I changed the threshold such that it will be resending more actively to overcome such a high packet loss.

And finnaly, I was able to make it much faster than before and I even ranked 4th in the competition in the class, and there are more than 20 teams in total. I was very happy that I would be able
to solve this by my analysis.


## Tell me about a goal you had and now you achieved it 
When I was working at Citrix as a software engineer intern, I was working on a CI/CD pipeline and my mentor told me that I shall finish it in 10 weeks, but because I have learned
quickly and I worked hard, I was able to build a working pipeline in 6 weeks. So I set a goal that what I shall do next is to reduce the building time becasue initially it take about 3 hours
to build which wastes people's time. 

There are many ways to increase the building time. The simplest way is to increase the CPU and RAM size, so I tried to give the VM more cores and more RAM. I spent a couple of days exploring and testing 
it the RAM, CPU and ram cache would improve the performance. But turns out that the building time is dependent on CPU performance, even if I double the RAM size and using a lot of RAM cache, the performance
improvement is only 10%. 

Then I tried to talk to a senior software engineer and learned how this product is built. Then I realized that actually this project is modulaized, so probably I can build different 
modules in parallel. So next thing i tried was to analyse the building process and try to split it and make it build in paralle so that I can utilize the cpu cores. And after a week of test, I finnaly find a way to 
build it in paralle and also pass all the build validation test. And I was proud that the build time was reduced to about 45 minutes, which is a huge improvement compared to 3 hours. And my manager and 
mentor was really happy about that and I am proud that I have did my best to finish the work I was assigned


## Tell me about a time when you had to work with limited time or resources.
So when once I attended the hackerthon in my school, it requries us to developed a stratey for a 
self driving car, and there are more 30 teams in the competition. The duration of this hackerton is 48 hours, but the problem is that if we want to test the result, it takes around 10 minutes for training and there are many variables in this contest, like the angle of each turn, and if the car goes out of the road it will be penaltied for 10s. So actually the time is really limited for a good algorithm.

So the idea I have is that I wrote a python script to sumbit for testing with different parameters. And I have figured out all the possible combinations of the parameters and I write a script to submit the parameters for testing all the time. So I can save the time for mannual submitting and waiting and be focusing on the best parameters. Also, I can submit the test even when I go sleep so I can make the best use of the time. And finally with such method, I found the optimum parameters and finally ranked 1st in this competition. I think throguh this experience I learned how to make the best use of time and work under stress and limited time.

## Save money for the company
Citrix Pipeline story

## how will you manage projects with no budget and no resources?



## Where do you want to be in five years?
I'd like to be a very senior and excellent software engineer, and I want to be an expert in a certain area. For example like a certain part of a huge distributed system,
or a certain area of a CICD pipeline. I'd like to stay in a area related tightly with chanllenging problems and I can use my knowledge to solve it efficiently and quickly.

## Tell me a time when you delegated a project effectively
## Tell me a time when you coached someone
When I was doing my undergraduate. I was a course TA in the course data structures so I was answering questions from students about some concepts about the course and their questions about homework. And there is a student who hasn't learned C before and this course requires using C++ to do the assignment. So I teach him the concepts of pointers and structure, so he can finish the assignment for implementing linked list. Also, I teach him some data structures like hash map so 
he understands how to use it to finish some assignment.

I think that helping him give me some change to review some basic concepts, and I would be able to practice my skills of expressing a complex concept easily and help others understand my idea. I think this is a precious experience for me.

## Tell me a time when you motived others
Course project, using C++, he is experienced with Python not C++

## Tell me a time when you showed initiative
//占领先机





## Tell me a time when you disgreed with your leader and you succeed
## Tell me a time when you take a risk and succeed
Citrix Using teamcity or Jenkins

More plugins, free, people are using it so for all the questions I met, maybe people are asking questions about it so it is very each to find some solutions online.

## Tell me a time when you disgreed with you leader and you failed
## Tell me a time when you take a risk and failed
C++ QT front end

## How do you deal with a difficult person or customer
I think that in most cases, 90% of the issues I have encountered with "difficult people" can be attributed to a miscommunication or misunderstadning. I tried to identify 
where the miscommunication/understanding occured and then create a solution from there.

The first thing i would do is to let them speak their idea and opinions.
And I think the key to solve this difficult situation is needs to follow a couple of ideas
1. The first and most important thing is that we shall never tell the client/people that they are wrong, no matter how riduculous the situation is.
2. The second thing is that we shall asking them questions and listen to them carefully, let them know that we are tring to understand their problem and want to solve their problem together
3. The thrid thing is that always staying calm and trying to be helpful

And if we can do those things, I believe that we can solve most of those situations. For example, I used to have a course projects and I was the group leader, one of my team
member always can't finish his part on time and this has hindered the my group's progress. So I tried to talk with him about it and finnaly I find that he don't quite understand 
the background knowledge of the project we are doing so he takes a lot of time trying to learn the basic concetps, which takes him a lot of time. So I understand that 
he is not intentionally procrastinating and he is actively tring to finish his job. So I told him to ask me anything whenever he meets some problem, and 
finally he and I become good friends and he can finish his part quickly and everyone in my group are very happy and we all get a A in the course.


#### Different opinions
I have been thinking which tools to choose for a couple of days, the choice I got was TeamCity, Jenkins or GitLab CICD. My mentor wants to use Gitlab CICD because he has some Experience with Gitlab CICD,
and he think it is a very popular solution. The reason why I choose jenkins is because Jenkins has a lot of necessary plugs we need to use later, like the Dashboard for Big screens, the Slack notification plugin,
the Perforce plugin.

#### Why not CD
The reason why it can't be a CICD pipeline is because it is a Type1 Hypervisor thus it can't be deployed directly because it don't build upon a OS and it has drivers code.


## Customer obsession.
When I was working as a intern, my team member received a call from a big customer and they reported that the reconecction after failure is not working as promised. One of senior software engineer in my team was respnsible for
this part, and he has a deadline in 1 week. At that time I still have plenty of time for my projects, and also the problem was because thread race condtion and I understand this concepts, so I think maybe I can help him with this.
So I helped the senior software engineer setting up different test cases and dumping the log to find out where the bug was, and finally we were able to locate that the bug is because a lock is released too early and we were able to solve 
it before the deadline, and my manager was extremely happy about it and customer was quiet after the quick fix. 


## Ownership
When I was building the pipeline, I used Jenkinsfile to set up the pipeline, and I was using a innner tool recently deveoped by Citrix call PacMan, it can integrated some inner tools like set up the authentication for Git, and also set up 
the place to put artifact. But it has a bug for cloning the git repo, I found that when it use it to clone the repo, it always faield half way. And nobody has encountered this problem when
they are using this tool. Because I think it is not a very hard problem to solve, so I read their source code carefully and finally I found it is because they have a default time out equals 150s, but the product I was using is too big to be cloned in 2 minuts becasue it has 1GB of code and dependencies,
so I told my mentor about it and he forward this issue to their team, and they have a quick fix about this issue.


 