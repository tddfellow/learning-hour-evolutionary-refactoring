# Learning Hour: Evolutionary Refactoring at a Larger Scale

_(reading: 10 min)_

It's often that the software engineers want to make large changes to the system that require weeks or months of
development time, and don't deliver tangible value to the business. These are usually required when the current
software design or architecture is no longer suited for what the team has been implementing recently, or for any
other non-functional reasons like security, debugging, performance, observability, etc.

The issue is that these changes are usually done as one large clump, take a lot of resources, and have high risks
of not working as expected, introducing bugs or downtime to the production system, when they are finally released.

## What's Considered a Small or Large Scale?

Refactoring on a small scale can be finished within one short focus session (e.g., no more than 30-60 minutes). 
Preferably, these can be automated and can be performed in several seconds, and maybe a few minutes. For example:
rename variable, move method, extract class, inject dependency explicitly, extract method, replace small algorithm,
etc.

Refactoring on a large scale is anything larger than that. Typical examples of a large-scale refactoring:

- Change the number of tiers of architecture.
- Replace direct references to the object, module, library or third-party with a Ports & Adapters approach.
- Extract common concern as a separate class out of major number of modules in the system.
- Move modules beyond boundaries.
- Make breaking API (or interface) changes across boundaries.
- "Revamp" architecture of how a particular problem is solved in the system (e.g., replace 2-tier architecture with
  3-tier one with DTOs at the boundaries).

## Deliver Refactoring Built Into Business Value

Delivering this kind of large-scale change is tricky, because the business will not be happy that they are not getting
any business value. Often, the business wants to get involved too deeply in this, because from outside it "does look
weird" that the engineers just want to do some work that has no visible impact. This could mean asking for a particular
number of weeks to perform this large-scale refactoring.

The problem is that, dragons may lie in the code, and will be discovered only during this change. Even if the business
authorizes this change with requested timeline, the likelihood of the timeline slipping is extremely high because these
changes don't deal with "simple" or "complicated". They are usually very complex, and unpredictable.

Additionally, there is always risk of having to rush your changes when you discover that there is much more work to do
than expected. Rushing these changes will only produce a system that is also bad quality. It can even be worse than
where you've started!

If you could deliver this type of change to the system **while delivering business value**, happening as part of the
valuable changes, then you avoid all of these issues. You also will have to deliver this change partially, and to
production. This means that the chance of bugs and outages is lower, and if they do happen, the debugging space is much
smaller.

You can fix the issue, and make another release. Frequently, very large changes cannot be effectively debugged, and you
would have to either live with bugs for a while, or have to revert. If you revert, and your time is up - then you're
unlikely to be given more time or another chance at a change like this.

## Deliver Often to Production

As already mentioned, delivering your incremental or partial changes to production as frequently as you can is very
important, simply because it reduces the risk of bugs and outages, and, on top of that, it makes it faster and cheaper
to fix these if they do happen.

## Divide & Conquer

Great principle to apply to such large-scale problems is Divide & Conquer. This is where you take the large problem as
a whole, and then split it down among several smaller items (but not too many at this stage, think 3-7 items).

Then you treat every item as a separate independent problem to solve. You may have to apply Divide & Conquer method to
it recursively.

Finally, when you make progress on your items, you can recompose them into the one big whole again in terms of a
solution. This can also, sometimes, be done incrementally - you can complete only few items, and then compose their
result, and deliver to production.

![](img/Divide%20And%20Conquer.drawio.png)

## Change in Stages or Slices

Another variation of this same principle is to slice the problem not into the hierarchical structure (where you 
potentially still need all its components to _actually deliver_ the change), but into incremental stages or slices
that can be delivered independently, on their own.

There are two types here:

In Stages - Instead of transitioning from the current software design directly to the new one, find an intermediate
design (or many design stages), that allow you to transition easier, in steps. Of course, these have to be fully
functional, deployable increments. For example:

![](img/Stages%20Example.drawio.png)

In Slices - Instead of applying the new design approach to the whole system, slice system into functional areas and
apply it one-by-one. Needless to say - fully functional and deployable increments. For example:

![](img/Slices%20Example.drawio.png)

As you can see, the choice of the order of implementation (and timing) of the slices is critical. We start with the
slice of the codebase within which we're currently are (and will be in predictable future) working on. Then we can
take the next most-frequently changing slice into account. And finally, we have a very stable part of the system that
doesn't change that often - here, we decide to make changes opportunistically, only if we have to touch it.

## Parallel Change

When you perform a large-scale refactoring, it's not uncommon to encounter breaking API and interface changes.
Sometimes, it's not possible to make a complete increment without breaking some dependencies or dependents.

You have a choice in these scenarios - not to deliver the refactoring slice (or many slices) to production environment
so that you don't break anything, or, as an alternative, apply Parallel Change Refactoring:

1. Create another version of the API
2. Make sure that you duplicate only the API, and not the logic behind it (extract duplication where necessary)
3. Mark old version of the API as deprecated
4. Use the new version in your new implementation code
5. Deploy to production
6. Each time you touch some code that uses deprecated API, replace with the new one, and deploy as an increment

## Choose Parts to Refactor at The Right Time

The major blocker to large-scale refactoring is the need to change the design in all the relevant places at once, in
pursuit of "consistency." In Evolutionary Design, such consistency isn't necessary, and, in fact, isn't required on
purpose. The reason is that the requirement for consistency makes developers afraid to make changes, and hence removes
the ability of the team to practice continuous improvement and continuous refactoring.

What you do instead is you prioritize the parts of the code that you should change now, next, later, and maybe never:

- Now - the code that you're working right now on very actively, and will be working on in foreseeable future.
- Next - the recent code, that you expect that you might be working on soon.
- Later - relatively stable code, that you don't expect to be actively working on. Maybe a small change or two there
  and there.
- Maybe never - very stable code, or stable legacy code, that you don't expect to be changing any time soon.

When you've defined the stages of your software design journey, then you can actually have different versions of 
approaches, that are documented, and every module of the system declares which version of the approach it is on.

Moreover, when you have to touch the code that is using the older version of the software design approach, then you
can refactor it either to the next stage, or advance it multiple stages. When can be done relatively quickly, feel 
free to update it directly to the latest version.

## Complete The Change in One Final Push

If you follow these incremental changes, and you realize that you have only 10-20% of the codebase that isn't yet
following the latest architectural approach - then you can make a final push as a separate work item just to finalize
this refactoring. This should ideally fit in one iteration. If it doesn't, then you're not ready to perform the final
push yet.

## Kata

_(practice: 45 min)_

As Ensemble, discuss the recent architectural change that the team had to apply, that was large-scale
and wasn't completed with an evolutionary approach.

Together, try to mentally return back to the beginning of that initiative and plan it anew with the
evolutionary approach.
