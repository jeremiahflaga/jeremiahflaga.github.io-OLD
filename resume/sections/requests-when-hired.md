<div class="resume-section-content col-md-9" markdown="1">

<h2 class="mb-5">Some requests when hired</h2>

### Pair programming sessions

> Many consultants believe that pair programming is the universal solution to any problem that a software project can have.
> 
> - It spreads the technical skills
> - It spreads the functional knowledge
> - It builds the team
> - It's fun
>
> --- Victor Rentea, ["Brainstorming a Clean, Pragmatic Architecture"](https://www.youtube.com/watch?v=mBxpOvlbAow&ab_channel=JUG.ru)


Depending on the complexity of your software system, and depending on my familiarity with the technologies being used in your software system, I might ask to have pair programming sessions with one of your programmers, for 1-3 hours per day, during my first few weeks on the job. And maybe once every two weeks after that, _if still needed_. This will help me become familiar with the coding styles and standards of your programming team. This will also help in passing your team's values on to me.


-----


### Single architectural vision


> I will contend that conceptual integrity is the most important consideration in system design. **It is better to have a system omit certain anomalous features and improvements, but to reflect one set of design ideas, than to have one that contains many good but independent and uncoordinated ideas.**
>
> Conceptual integrity in turn dictates that the design **must proceed from one mind, or from a very small number of agreeing resonant minds**.
>
> ... I will certainly not contend that only the architects will have good architectural ideas. Often the fresh concept does come from an implementer or from a user. However, all my own experience convinces me, and I have tried to show, that **the conceptual integrity of a system determines its ease of use. Good features and ideas that do not integrate with a system's basic concepts are best left out.** If there appear many such important but incompatible ideas, one scraps the whole system and starts again on an integrated system with different basic concepts.
>
> Frederick P. Brooks, Jr.,  ["The Mythical Man-Month", chapter 4](http://ptgmedia.pearsoncmg.com/images/0201835959/samplechapter/chap4.html)

Fred Brooks, it seems to me, is suggesting that it's best if a codebase follows a single structure or architecture.

I would like to request that...

If the architecture of the codebase I'll be working on is obvious on first look, and if it was already decided by the team to keep following that architecture, then no request coming from me regarding this.

If the architecture of the codebase I'll be working on is not obvious on first look (because there might be two or three competing architectures), I would like to request for the team lead of software development to explain his architectural vision for the codebase. By "architectural vision" I mean the direction he/she wants the codebase to follow moving forward. This will help me see where the codebase is supposed to be going, and to have good knowledge on how I should write my code as to not deviate too much from his/her architectural vision. (I would like to note that when I use the words "team lead", what's in my mind is someone who is also working on the project hands-on, because in that case the team lead truly knows the codebase and its problems, and so it will be hard to argue against his/her architectural vision.)

If the codebase does not have an architecture (it's a mess because there are hundreds of competing architectural ideas within the same codebase), and no architectural vision is in place, then I would suggest that someone or sometwo be assigned to look for an architectural vision that will be followed from a set time in the future and forwards. If I am choosen to be the one who will set the architectural vision for the codebase, I would like to ask that it be made clear to the team why a single architectural idea is needed --- that is, to make the codebase easier to manage or maintain* --- so that they will not rebel against me :smile:. (Depending on the complexity and size of your codebase, it might take a few months of working on it before I can find an architectural vision which matches with the current state of the codebase --- that is, an architectural vision that will not require a big overhaul of the codebase, but still makes it maintainable. Also, because I know that someone out there is better than me at doing things, I am ready to step down from the position of being in charge of the codebase if someone more competent than me is needed to take charge of it in the future. Also, because I know that I'm mortal and might die anytime, I will do my best to make everyone in the team understand the architectural vision, so that the codebase will move in that direction even when I'm already dead before the goal of making the entire codebase maintainable is completed.)

(Please note that if there is a plan to scrap or replace the codebase after a year or two, there _might_ be no need for this architectural vision thing. It is only most needed when the system is expected to be maintained for 10 or 20 years more.)

-----

 \* <small>"The true cost (80%) of software lies in its maintenance, not in its initial development." - [time 03:17 of "The Art of Clean Code" by Victor Rentea](https://youtu.be/AeWbJ5LIFNg?t=197)</small>


<div class="d-none d-print-block">
    <br /><br />
</div>


</div>
