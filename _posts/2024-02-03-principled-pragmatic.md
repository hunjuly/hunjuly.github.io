---
layout: post
title: "The Ideological War in Software Development: Principled vs. Pragmatic"
---

Just like communism and liberalism during the Cold War era, two irreconcilable ideologies exist in the field of software development: principled and pragmatic approaches. Software development is an extremely complex process, and these two ideologies approach problems in different ways. Therefore, the principled and pragmatic approaches can be considered important factors that determine the success or failure of software development.

Principled developers believe that development should proceed based on systematic software development theories. They emphasize the importance of maintainability and argue that code should be carefully written from a long-term perspective. On the other hand, pragmatists focus on solving problems as simply and quickly as possible. They believe that no matter how carefully code is written, it is ineffective because software is complex. So rather than investing a lot of time upfront, they think it is more efficient to respond immediately whenever a problem occurs.

However, it is difficult to clearly classify developers as principled or pragmatic. Most developers will likely fall somewhere in the middle of the spectrum between these two ideologies. Furthermore, as a developer's tendencies can change depending on the situation, this distinction becomes even more ambiguous.

In this essay, I want to focus on extreme principled developers and pragmatic developers. I will examine their contrasting perspectives and approaches, as well as the problems that arise in the collaboration process.

## 1. Principled Team Members

If a junior developer is passionate and eager to grow, there is a high probability that they will become a principled team member. If such a principled team member meets a principled team leader, they will be happy, but in reality, the combination of a principled team member and a pragmatic team leader is more common.

A principled team member under a pragmatic team leader may find themselves in a difficult position within the development team. It is obvious that they cannot expect code reviews from the team leader, and they have no room to pay attention to code quality because the team leader is rushing the schedule. Even if there is time for refactoring, they have to be mindful of the team leader's watchful eye. When a problem occurs, the principled team member strives to identify the cause and take appropriate action. However, the pragmatic team leader may ignore the cause and force an easy way to solve the problem.

When this happens repeatedly, the principled team member feels frustrated. It seems that not only is the team leader's competence lacking, but they are also handling work carelessly without a sense of responsibility. Believing that there is nothing to learn from the team leader and that their work attitude is not respectable, the principled team member may even show disrespectful words and actions towards the team leader. The team leader also senses this and treats the principled team member emotionally. The principled team member becomes mentally exhausted and eventually resigns. However, even if they change companies, they are likely to encounter a pragmatic team leader again, so the difficulties of principled team members are not easily resolved.

As much as principled team members have a strong desire for growth, they may become impatient if they feel that the company's work is not helpful for their growth. Work that is not helpful for growth includes maintaining code with a lot of technical debt, or even new development projects that use old or unpopular technologies.

In this situation, if the team leader is pragmatic, they will have a strong tendency to maintain the current situation, so they will try to avoid large-scale refactoring or the introduction of new technologies as much as possible. They will not accept alternative suggestions from principled team members. Just as the first button must be fastened correctly, the projects during the junior period can have a significant impact on a developer's growth trajectory. If it seems mismatched, it may not be a bad idea to boldly leave the team.

However, if the team leader has a principled tendency, I recommend waiting. If the current project has many technical issues, there may be an opportunity to create a new one. In that case, since you have already grasped the domain and requirements while maintaining the existing project, it will be an environment where you can focus more on technology. Of course, the principled team member themselves must be prepared to take on such a project.

I want to say this to principled team members:

**"The company is not an academy."**

A company is a place where you sell your time and skills and get paid. You can't always do what you want to do at the company.

> While frequently changing jobs and encountering various team leaders, I met some who respected my intentions, but two of them were extremely pragmatic team leaders. These extremely pragmatic team leaders seemed to dislike young team members who were full of confidence and passion, and they sometimes openly kept them in check. Still, I think I showed maximum respect from the perspective of a team member. When I was having conflicts with the team leader, I asked other team members several times if I had done something wrong, but most of the answers were that we simply didn't get along.
>
> Looking back now, I wonder if the pragmatic team leader's own development competency was falling behind, and while feeling anxious about it, the principled team member's open talk about design patterns or refactoring further stimulated that anxiety. So in the eyes of the pragmatic team leader, the team member who was overconfident and lacked awareness may not have been viewed favorably.

## 2. Principled Team Leaders

If a principled team member's passion continues, they generally become a principled team leader. Although I said team leader, this applies to anyone in a position to lead development with authority over the project.

### 2.1. Over-Engineering

Principled team leaders tend to be excessively obsessed with preparing for the future. This manifests as over-engineering, such as excessive abstraction and unnecessary application of design patterns. This actually increases code complexity and decreases development efficiency. It becomes a tragedy where the principled approach for maintainability ironically harms maintainability.

Such over-engineering makes managers feel frustrated and anxious. The principled team leader emphasizes the importance of design and is working hard, but since there are no visible results, the schedule is very worrisome. In the early stages of the project, there is time, so they wait and see with trust, but when a limit is reached, they actively manage the schedule.

The more thoroughly a principled team leader prepares for the future, the weaker they may be at overseeing the entire project. If requirements change significantly in the middle of the project after spending a lot of time on design and technical research, or if unforeseen technical issues emerge, what will happen?

The principled team leader may think that the cause of these problems lies outside the development team, so it is not their responsibility. And since preparing for such problems is force majeure, they may think it is reasonable to extend the project period or reduce functionality. However, if they cannot easily respond to such changes or problems, the manager will not be able to understand why so much time was devoted to design.

In this case, from the manager's point of view, regardless of maintainability, the pragmatic team leader's development method, which at least has higher immediate productivity, may be better. The principled team leader does not understand such an evaluation from the manager. They feel frustrated that their efforts for good quality are being denied.

> This is partly my story. Whenever I started a new project, I always set a technical challenge. For example, I would try applying TDD or DDD in this project. How much and how long this challenge lasted depended on the manager's feedback. Usually, if more than a month has passed since the start of the project and product development is slow with only technical research being done, the manager gives a warning that development progress is slow.

### 2.2. Over-Processing

Over-processing refers to excessively formal and inefficient development processes.

If over-engineering torments managers, over-processing torments team members, mainly in the following ways:

1. Requiring excessively formal and detailed development documents
2. Applying excessively strict coding rules
3. Obsession with test coverage

Principled team leaders often demand excessive test writing from team members to achieve high test coverage. Of course, testing is important, but if you focus only on raising the coverage number, code flexibility decreases and test code itself becomes complicated, making maintenance difficult.

Team members may have high expectations for the principled team leader's claims of the correct development methodology. However, it won't be long before the team members become exhausted by the team leader's overly systematic approach and demands for meticulous planning.

If the principled team leader's management is loose, team members will gradually start to ignore instructions. On the other hand, if management is thorough, not only will the team leader themselves have less time for coding, but they will also point out minor mistakes of team members. Team members will feel pressured by trivial remarks, and the principled team leader will feel frustrated by the team members' shortcomings.

The principled team leader's excessive requirements may sometimes be tasks that even they themselves have difficulty completing perfectly. Especially since software design deals with complex and abstract concepts, it is not easy to clearly document one's ideas. Nevertheless, principled team leaders often demand designs from inexperienced team members without providing proper guidelines. This is as daunting for team members as an elementary school student having to write a book report.

> It was when I joined a new company as a team leader. Apparently, the previous team leader was a passionate principled developer and demanded 100% test code coverage from team members. Of course, maintaining 100% is a good goal. The problem is that by meticulously writing tests for each function, many tests break even with small changes. Functions do not run independently; they call other functions and are called. So tests should be written in appropriate units considering maintenance and productivity, but this aspect seems to have been overlooked.
>
> As a result, when the test code was reduced to 1/10, team members began writing test code more actively.

### 2.3. Causes of Problems

Principled team leaders experience the most trial and error because their competence and experience are lacking compared to their authority. For some individuals, overconfidence can further exacerbate the problem.

Passion and confidence are somewhat proportional, so it seems difficult for a passionate principled team leader to avoid high confidence.

High confidence tends to make one find the cause of problems in others and make strong self-assertions. Such words and actions become a great burden to managers and colleagues, but the person is unaware of it.

Lastly, I want to say this to principled team leaders:

**"Do what you have to do, not what you want to do."**

You need to take a cold, hard look at your thoughts and see if what you're trying to do is really necessary. Principled team leaders tend to rationalize what they want to do as what they have to do.

## 3. Pragmatic Team Members

While principled team members try to use company work as an opportunity for growth, pragmatic team members accept company work as an economic activity. So when there is a problem, principled team members put in excessive effort, while pragmatic team members try to solve the problem with minimal effort.

There are various types of pragmatic team members:

The first type is diligent but has low passion for development. This type can do well with tasks that principled team members find difficult, so low passion for development is not a disadvantage. With less ambition for development, they cause fewer problems and can have a smooth company life.

The second type is diligent and has high passion for development. When I first met this type of developer, I didn't understand. If they have a high passion for development, you'd think they would be a principled team member, but they have no interest in maintenance. Even if you point out problems and provide guidance a few times, they ultimately solve problems with minimal cost.

This type of pragmatic team member seems to have a high passion for work rather than a pure passion for development, and they take an interest in development out of necessity. As a result, they tend to prioritize learning technologies that will help them get a job at a better company. If a pragmatic team member studies design patterns, it's not because they're curious, but because it might come up as an interview question. So they don't study it in depth. They also study algorithms diligently because it is necessary for coding tests.

The third type has low passion for work. In this case, it is usually a principled team member who is not motivated because they don't like the company's work. Or they simply have no interest in work itself.

Even with low passion, if they just do their job properly, there won't be much conflict with the team leader. The problem is that since they don't want to work, they gradually neglect responsibility. For example, when changing code, they should carefully test it, but they don't even build and report completion. Or they only passively perform assigned tasks and don't proactively find work to do, so the team leader's management cost increases. If this is repeated, relationships with others will also deteriorate.

Regardless of the type, it is difficult for a pragmatic team member to grow as a developer. It is common for them to stay in one company for a long time and become a middle manager. However, while becoming a middle manager is easy, if they don't have differentiated strengths, it may be difficult to keep that position until the end. As someone who has seen many people regret becoming a manager, I advise carefully considering what direction you want.

I want to say this to pragmatic team members:

**"There is no easy and comfortable path. You grow as much as you ponder."**

Pragmatic team members prioritize efficiency, so they have fewer opportunities to ponder. However, whether in human relationships or development, you can grow as much as you ponder, and you can differentiate yourself as much as you grow.

## 4. Pragmatic Team Leaders

Many developers become pragmatic team leaders, and not only pragmatic team members but also principled team leaders can become pragmatic team leaders. It is natural for pragmatic team members to become pragmatic team leaders, but what does it mean for principled team leaders to become pragmatic team leaders?

Principled team leaders learn many development methodologies and architectures such as OOP, design patterns, TDD, DDD, and MSA, but it is not easy to apply them to actual projects. These methodologies and architectures are interconnected, so applying them to a project requires a great deal of effort and experience.

For example, to properly apply MSA (Microservices Architecture), one must be well-versed not only in OOP and design patterns, which are the foundation of modern development methods, but also in DDD (Domain-Driven Design). However, the reality is that it is common to not even properly grasp OOP and design patterns. TDD (Test-Driven Development) also requires not only OOP/design patterns but also an understanding of the domain and flexible design.

If you don't reach this level, it is difficult to apply the learned development methodologies to actual projects. After experiencing a few times of applying them to projects but ultimately only amplifying problems, you come to dismiss development methodologies as unrealistic ideals.

So in the eyes of a pragmatic team leader, a principled developer seems to be wasting time being absorbed in plausible theories without knowing reality. The pragmatic team leader has seen many colleagues around them proclaiming the importance of development methodologies, but they have never seen a case where quality actually improved, only delayed schedules. So they view the principled team member's efforts as a waste of time and find it pathetic.

Pragmatic team leaders, with little interest in technology, put more effort into requirements analysis and schedule management. In the process, they naturally transition to a managerial role.

I have never seen a pragmatic team leader actively lead technology, such as through code reviews. They usually just assign work to team members in module units and check the results. When technical problems arise that team members cannot solve, they sometimes solve them directly, but as the number of team members increases, they seem to gradually move closer to being a manager.

One of the reasons why pragmatic team leaders transition to managers is that development is not new and repetitive. Since pragmatic team leaders do not learn new technologies or methodologies, the development process does not differ much from the past. Tired of this repetitive work, they often become managers.

In my experience, pragmatic team leaders can achieve good results in projects that last less than a year, such as SI. However, if a project lasts more than a year, quality management becomes difficult, and managers will also start to sense problems.

When such a pragmatic team leader develops an in-house product, they sometimes adopt a strategy of constantly remaking it. In other words, instead of continuously improving the code, when the project reaches a certain level, they leave the existing code, which has become complex, as is and start making it from scratch. They take this strategy of gradually expanding functionality, and I think the repetition cycle is usually about 1-3 years.

There are also cases where a pragmatic team leader maintains a product for several years without remaking it. This is when the person in charge of development doesn't change, but if that person quits, maintenance capability plummets, and it eventually becomes a situation where it needs to be remade.

Lastly, I want to say this to pragmatic team leaders:

**"Do what you have to do, not what you can do."**

Sometimes you may have to go through a lot of research and trial and error due to bugs with unknown causes. If you write code just to cover up the problem, most of the time it will come back as a bigger problem in a different form.

## 5. Micromanagers

A micromanager is a manager who excessively interferes with and tries to control the work process of members within the organization.

Micromanagers are largely divided into two backgrounds. One is a manager who claims to have been a developer, and the other is a manager who has no development experience but thinks they know to some extent. What they have in common is that they mistakenly think they know enough.

Since micromanagers overestimate their own competence, they share some of the problems of principled team leaders, most notably demanding thorough design and documentation. The difference is that because they are managers, their impact on the project and the organization as a whole is greater.

Since micromanagers do not develop directly, they sometimes easily choose a principled approach. This is due to the tendency of managers to prefer an ideal approach over realistic constraints as they move away from the development field.

Here is the rest of the translation:

> It was when I was working as a team member at a security-related company.
>
> As the quality of the company's services continuously declined, the manager instructed each team leader to write design documents. At the time, the C++ source code was so complex that a single file exceeded 50k lines. It is basic to implement based on design, but the solution presented by the manager was to write design documents based on the implementation.
>
> In the end, the development team spent about 3 weeks writing design documents and then stopped. The tragedy is that the manager who gave this instruction thinks that the developers lack the skills to follow his instructions.
>
> If the situation required strengthening the development team's capabilities, the manager should have recruited competent developers instead of directly intervening.

Managers and team leaders have their respective roles. Just as team leaders should not become managers, managers should not try to become team leaders either. If a manager takes on the role of a team leader, the team leader cannot properly exercise their capabilities, and the manager cannot focus on management tasks.

In my experience, managers directly leading development is often a choice based on their own desire rather than an unavoidable situation. Simply put, development leadership seems to be treated as a manager's hobby.

A wise manager would replace the team leader or adjust the team's tasks.

There is a saying:

**"疑人不用 用人不疑"**

This is a saying from the Chinese historical book Song Shi, which means do not use a person you doubt, and do not doubt a person you use.

> It was when I was working as a team member at a company that provided specific information services. The development team at this company consisted of around 8 people, and there was a separate QA team. The CEO was a brilliant person, and the development team leader was a respected developer with passion and skills.
>
> When I was working at this company, the CEO actively intervened in the development process, but since it was the early stages of the startup, there was an atmosphere of understanding each other even when service failures occurred.
>
> A few years later, when I quit and visited again, the development organization had changed significantly. The existing developers were working on unclear tasks in separate spaces, and a new development team directly formed by the CEO was in charge of important tasks. When I asked the reason, it turned out that the CEO had continuously raised quality issues and eventually declared that he would directly lead the development and formed a new team.
>
> The CEO's gain from leading the development was just another big trial and error. In this case as well, the CEO should have hired developers who could meet his standards.

## 6. Conflict Between Principled and Pragmatic Team Leaders

If principled and pragmatic team leaders maintain a harmonious cooperative relationship in a project, it means that there is a significant threat that they don't have the capacity to fight each other. For example, there may be situations like severe bullying from customers or a high-pressure micro-manager.

If not, principled and pragmatic team leaders will have conflicting opinions on every little thing that requires cooperation, such as API design.

### 6.1. Perspective of the Principled Team Leader

The principled team leader thinks that the pragmatic team leader is holding them back.

When designing an API, the pragmatic team leader argues in a way that minimizes changes to their own code. As a result, the API becomes unintuitive and difficult to understand, and the principled team leader has to do more work.

As the project progresses, a new API is needed, but the pragmatic team leader sticks to the existing API, even if it's a bit inconvenient to use, as long as it can be used.

As such, the principled team leader thinks that the pragmatic team leader only pursues their own convenience and lacks overall development capabilities, including insight.

It is difficult to shake off the anxiety that if they continue to follow the pragmatic team leader's opinion, it will eventually cause big problems for the project.

The project's design is old and has structural problems, but the pragmatic team leader only opposes the large-scale refactoring plan to solve this.

Such a simple development strategy is not helpful for their own growth either.

### 6.2. Perspective of the Pragmatic Team Leader

The pragmatic team leader thinks that the principled team leader unnecessarily stirs up work.

The principled team leader forcibly introduces technologies that they themselves cannot fully handle, unnecessarily complicating the project.

They think that the principled team leader is obsessed only with personal growth and puts the project's schedule or success on the back burner.

Even after agreeing on an API with the principled team leader, the principled team leader may keep requesting small changes. The principled team leader's excessive obsession with details hinders the progress of the project.

They make claims about large-scale refactoring or remaking, but it doesn't seem like it will particularly improve things, and there is no guarantee that it will improve.

### 6.3. Mediation by the Micromanager

When a conflict of opinion occurs, the micromanager thinks that they can listen to both sides and make a logical judgment.

Although the micromanager will try to judge fairly and objectively, this situation already starts with the principled team leader at a disadvantage.

It is the principled team leader who tries to change something or apply new technologies or methodologies. Therefore, the principled team leader has to explain their logic in a lengthy manner, mixing it with software development theories.

On the other hand, the pragmatic team leader prefers to minimize change as much as possible or choose simple solutions, so they can easily explain their logic.

So when a debate between the two begins, all the pragmatic team leader has to do is ask the principled team leader, "Why?" The principled team leader, even with considerable knowledge, will find it difficult to defend against the pragmatic team leader's "Why?" attack. At some point, they will reach a point that is difficult to explain. Moreover, since they have to make the micromanager, who lacks development knowledge, understand, the principled team leader ends up debating from a 2:1 position.

Even if the principled team leader doesn't fully understand the development theories themselves, trying to explain them to the micromanager will lack persuasiveness. The micromanager will only vaguely feel that it sounds plausible. In the end, the micromanager's initial idea of a logical judgment becomes difficult, and it is common to propose an appropriate compromise considering each other's feelings.

As this is repeated, the micromanager's opinion will lean towards the pragmatic team leader, whose opinion is easy to understand. Moreover, since results can be confirmed relatively quickly, there is no reason to refuse.

Or there are cases where the micromanager gives up mediation with instructions for the two to discuss it well, which is the worst choice. The conflict between the two team leaders, now without even the last brake, will reach an extreme and the project will not be completed safely.

### 6.4. Solution

It was the micromanager who created a situation where the two team leaders had to fiercely argue in the first place. The micromanager's delusion that they can mediate between the team leaders and their desire to lead development have likely combined to give them the final decision-making authority.

The only one who can mediate between the two is a superior technical leader with overwhelming experience and competence. More precisely, the superior technical leader should proactively lead the development team leaders. The fact that the development team leaders are having discussions means that there is no superior technical leader. The problem is that it is not easy to find a superior technical leader with overwhelming experience and competence that general development team leaders can accept.

In a situation where there is no superior technical leader, it is difficult for the manager to directly mediate between the two, and it is burdensome to take sides with one as the other will object. However, it would be better in the long run to clearly assign responsibility and authority to one side depending on the nature of the project, even if it means accepting losses. If it is a short-term outsourcing project, the pragmatic team leader should lead, and if it is an in-house product that requires continuous improvement, the principled team leader should lead.

> I had an experience working at a company with its headquarters in Europe.
>
> This company was developing a quite large online service. There were several development teams creating one service, and there was a separate QA team. What was noteworthy was that there was an architect team, and there was even a top architect.
>
> With the organization divided into design, implementation, and testing like this, before starting development, the architect, development team, and QA team gather to discuss the design and testing methods.
>
> At this company, decisions about design, including interfaces, were made among the architects, so it was difficult to find discord between development teams. There could be differences of opinion between the development team leader and the architect regarding the design, but since the design authority and responsibility lay with the architect, it never became a problem.

## 7. Conclusion

The difference in ideologies is not the cause of the conflict between principled and pragmatic approaches. Even if there are ideological differences, as long as there is sufficient trust in each other, they can respect each other and proceed with the project well.

Conflicts arise due to personal emotions and desires that humans have. When interests collide due to personal emotions and desires, cracks are bound to form, and different ideologies only widen these cracks.

From a manager's perspective, the arguments of pragmatic developers may seem useful. However, you need to carefully consider what the company will look like if pragmatic development continues.

The principled team leader's development method does not immediately produce results, they have strong self-assertions, and costs increase due to over-engineering. If the company cannot endure these trials and errors and flows towards pragmatism, only pragmatic developers will remain in the end. In a situation where pragmatic developers make up the majority, even if the manager senses quality issues and tries to hire a principled developer, it will be difficult for them to endure the resistance from the established pragmatic team leaders.

On the other hand, if principled developers make up the majority, they may become dedicated to research or introduce experimental technologies, causing productivity to plummet. The quality of the software does not greatly improve either because their understanding of development methodologies is not complete.

A manager who values software quality thinks that the trial and error of principled developers has some value. However, the growth of principled developers may be slower than expected.

To solve these problems, there must be at least one competent superior technical leader. A superior technical leader can greatly reduce the trial and error of principled developers and place pragmatic developers in the right positions.

If a superior technical leader cannot be secured, the company will eventually have to choose the developers' tendencies according to the nature of the product they are developing. If it is an in-house product, pragmatic developers should be minimized. If it is a short-term outsourcing project, principled developers should be minimized. Just as maintenance becomes difficult as technical debt accumulates, if you accumulate hiring debt for the reason that developers are lacking at the moment, it will become difficult to maintain the development organization.
