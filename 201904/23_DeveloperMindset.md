# Developer Mindset

[Home](../README.md)

## Background

There are many small factors that are slowly and gradually harming the developers’ projects. They are not immediately destructive. Most of them only do long-term damage. Something you won’t see the damage for a year or more. So when somebody proposes them, often they sound harmless. Even when you start implementing them, they may seem fine. But as time goes on — and particularly as more and more of these stacks up — the complexity becomes more apparent and grows until you’re another victim of that ever-so-common horror story.

To avoid being one of the victims, you should embrace the fundamental laws of software. You should develop a mindset that every developer should have. This mindset will help you make better decisions in your daily programming journey. You can keep your software as simple as possible. You can protect it from being an unmanageable and complex system.

## Mindsets

### The purpose of software

The purpose of the software is not to show off how intelligent you are, but to help people.

### The goals of software design

The goals of software design is to design systems that can be created and maintained as easily as possible by their developers, so that they can be — and continue to be — as helpful as possible.

One of the best ways to improve your design skills is to be sure that you fully understand the systems and tools you are working with.

### Simplicity

Programming is the act of reducing complexity to simplicity. A “bad developer” is just somebody who fails to reduce complexity. A “good developer” is doing everything in their power to make the code as simple as possible for other programmers.

A good developer creates things that are easy to understand so that it’s really easy to shake out all the bugs.

Now, developers are generally intelligent people and none of them likes to be treated like they are idiots. Ironically, this leads them sometimes to create things that are a bit complicated. They basically think like this:

```text
"Oh, other developers will understand everything I’ve done here. I should write some clever code that is hard to understand so that they can think that I am very smart."
```

A mistake caused by a wrong mindset-not necessarily by a lack of programming skills. Most of the programming failures happen because of that mentality.

```text
"Complexity has nothing to do with intelligence, simplicity does." — Larry Bossidy
```

### Complexity

```text
"Controlling complexity is the essence of computer programming." — Brian Kernighan
```

Your main purpose is to control complexity, not to create it. The source of many software failures is complexity. Things get very complex because you expand your software purpose for no reason.

How could you avoid being a victim?

- Know your software purpose and its definition.
- Be as simple as possible in every piece of code you write.
- Evaluate new features or change requests based on your software purpose and question them.

As a developer, your first behavior should be resistance to (unnecessary) change. This will prevent you from adding unnecessary codes into your software. When you are convinced that this change is a need, then you can implement it.

### Maintenance

Quick coding and fast shipping look more important than code maintenance. This is the point where they make a mistake — ignorance of future code maintenance.

**All changes require maintenance.**

It is more important to reduce the effort of maintenance than it is to reduce the effort of implementation.

### Consistency

Code that isn’t consistent becomes harder to understand. Don’t keep forcing developers to relearn the way your system works every time they look at a new piece of it. ==> General guidlines or code conventions.

### Prioritizing

When you face many possible directions, how do you decide which option is the best? What to focus on and which features you should implement?

The desirability of any change is directly proportional to the value of the change and inversely proportional to the effort involved in making the change.

When you prioritize your work, you should follow this rule:

**The changes that will bring you a lot of value and require little effort are better than those that will bring little value and require a lot of effort.**

### Solving problems

The first step is understanding. Know exactly what is being asked. Most hard problems are hard because you don’t understand them. Write down your problem and try to explain it to someone else.

```text
"If you can’t explain something in simple terms, you don’t understand it." — Richard Feynman
```

The second step is planning. Don’t take action. Sleep on it. Give your brain some time to analyze the problem and process the information but don’t spend too much time on planning.

The third step is dividing. Don’t try to solve one big problem. When you look at the problem as a whole, it can scare you. Divide it into smaller tasks and solve each sub-problem one by one. Once you solve each sub-problem, you connect the dots.

### Good enough is fine

```text
"Perfect is the enemy of good." — Voltaire
```

Whether creating a new project or adding a feature to existing system developers tend to plan everything out in detail from the beginning. They want the first version to be perfect. They don’t focus on the problem they will solve and how their software will help people. They start by thinking every small detail they could think. Then assumptions and predictions come along followed by “What if” sentences. They have to predict the future because they were now so captivated by the imagination of the project in their mind and their project has to be as perfect as they imagined it.

And what will happen are:

- You will be writing code that isn’t needed
- You will increase complexity by adding unnecessary codes
- You will be too generic
- You will be missing deadlines
- You will be dealing with many bugs caused by the complexity

**Start small, improve it, then extend.**

### Predictions and Assumptions

When faced with the fact that their code will change in the future (new requirements, new requests, new features,...), some developers attempt to solve the problem by designing a solution so generic that (they believe) it will accommodate to every possible future situation. Code should be designed based on what you know now, not on what you think will happen in the future.

**You can’t predict the future, so no matter how generic your solution is, it will not be generic enough to satisfy the actual future requirements you will have. Being too generic involves a lot of code that isn’t needed.**

Code should be designed based on what you know now, not on what you think will happen in the future.

### Stop Reinventing

Don’t reinvent the wheel. The only times it’s okay to reinvent the wheel is when any of the following are true:

- You need something that doesn’t exist yet
- All of the existing "wheels" are bad technologies or incapable of handling your needs
- The existing "wheels" aren’t being properly maintained
- The existing "wheels" are too complicated for your usecases

### Resistance

Always resist adding more code, more features until you are convinced that they are required and there is a need to implement them. Because unnecessary changes will increase defects in your software.

### Automation

Don’t spend your time on repetitive tasks. Set them up and forget about them. They can work while you are sleeping. **If you can automate it, automate it.**

Code measurement

```text
"Measuring programming progress by lines of code is like measuring aircraft building progress by weight." — Bill Gates
```

The optimum code is a small bunch of code that is easy to understand, easy to read.

### Productivity

```text
"One of my most productive days was throwing away 1000 lines of code." — Ken Thompson
```

Your main goal should be keeping your code base as small as possible.

### Testing

You should be reliable. When other developers in your team see that you committed new code to source control, everyone should know that your code is tested, and works. **Untested code is the code that doesn’t work.**

### (Under)Estimation

Developers’ estimation sucks. Usually, they underestimate things rather than overestimate them. They underestimate the time and effort required to develop a small amount of code or a feature.

Try to break the big thing into smaller things. The smaller it is, the easier it is to estimate. **Everything takes longer than you think.**

### Running Away From Rewriting

Rewriting code is often a developer delusion, not the solution in most cases. Well, because _it’s harder to read code than to write it_. This is why it is so hard to reuse code. This is why our subconscious mind whispers to us `Throw it away and start over` when we read another developer’s code. But **refactoring should be the first option.**

### Documentation and Commenting

One of the common misconceptions about commenting is that developers add comments that say _what code is doing_. The real purpose of comments is to explain "WHY" you did something, not "WHAT" the code is doing.

It is important to have documentation to explain your software’s architecture and every module and components. This is required to see the high-level picture of your software. When a new developer joined your team, it would be easier to understand the software as a whole. When developers don’t have any clue about other parts of the software, they could easily make a mistake in their own part which can affect other parts also.

### Picking Technologies (Tools, Libraries, etc.)

**Don’t depend on external technologies or reduce your dependency on them as much as possible.** Because they are another common source of complexity.

It is so important to pick the right technologies for your project, you should consider before you start using some technology:

- Is there active development behind it?
- Will it continue to be maintained?
- How easy is it to switch away from?
- What does the community say about it?

### Self-Development

Keep learning. Try out different programming languages and tools, read books on software development. They will give you another perspective. Every day small improvements will make a real difference in your knowledge and skills.

Be open-minded. Don’t be obsessive about one technology. Use the required technology to solve a specific problem.

**Know that every specific problem has its own specific solution.**

### Don’t be a hero

A lot of times it’s better to be a quitter than a hero. Don’t be obsessive. Know when to quit. Don’t hesitate to ask for help.

### Don’t Ask Questions… Ask For Help

When you have something to implement and you are not sure about the solutions, don’t ask others how to so it …at least not immediately. Instead, try anything and everything you can think of.

But always seek advice. When you have tried everything, and preferably after you have a working solution, now is the best time to seek advice.

## References

<https://medium.freecodecamp.org/learn-the-fundamentals-of-a-good-developer-mindset-in-15-minutes-81321ab8a682>
