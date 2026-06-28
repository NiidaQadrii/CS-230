# Draw It or Lose It — Software Design Document

**Author:** Nida Qadri
**Course:** CS 230 — Operating Platforms

## Project Summary

The Gaming Room is a game development company seeking to expand their Android-based 
drawing and guessing game, *Draw It or Lose It*, into a cross-platform web application. 
The game is modeled after the classic TV show *Win, Lose or Draw* and required support 
for multiple teams and players, unique name enforcement, a singleton game service, and 
timed rounds with progressively revealed images.

This software design document guided the full arc of that expansion — from requirements 
gathering and domain modeling through OS platform recommendations and distributed systems 
architecture.

## Reflection

### Interpreting User Needs and the Role of User Stories

Translating client needs into design decisions requires looking beyond what is explicitly 
stated to what the requirements *imply*. The Gaming Room specified that game names must be 
unique and that only one game instance should exist in memory at a time — but the deeper 
implication is that the application needs a centralized, authoritative state manager. That 
insight drove the recommendation of a singleton `GameService` class and, at the 
infrastructure level, a distributed session cache (Redis) to keep state consistent across 
server nodes.

User stories support this kind of thinking by anchoring every technical decision to a 
real person's goal. Rather than designing around features in the abstract, user stories 
ask: *who needs this, and why?* That framing makes it easier to spot the difference 
between what a client asks for and what would actually serve their users. It also creates 
a traceable thread from requirement to design choice to implementation — so that when 
tradeoffs arise, the team can evaluate options against user impact rather than personal 
preference.

Keeping user needs at the center of design decisions matters because software that is 
technically elegant but functionally misaligned with its users provides little real value. 
Every architectural choice — from the file system to the authentication model — has 
downstream effects on user experience. A design that ignores those needs may work in 
isolation but fail under real-world conditions: slow mobile connections, concurrent 
multi-team sessions, or the need to recover quickly from a dropped connection.

### Approaching Development and Agile Processes

My approach to this design was to move from the most concrete and specific (the client's 
explicit requirements) outward toward the more general (platform and infrastructure 
recommendations). Starting with requirements ensured that every subsequent decision had a 
traceable justification rather than being a preference or assumption.

Working through the design document before writing any code made the development process 
significantly more deliberate. Documenting requirements forced an early confrontation with 
constraints that might otherwise have surfaced late — for instance, the need to enforce 
unique game and team names, or the memory implications of a singleton service holding 
references to hundreds of active game objects. Having those constraints written down made 
it easier to evaluate design decisions as the code took shape.

For future projects, I intend to bring Agile principles more explicitly into my workflow 
— particularly iterative delivery, continuous feedback loops, and sprint-based planning. 
Agile's emphasis on working software over exhaustive documentation complements, rather 
than replaces, upfront design: a lightweight domain model and a clear set of user stories 
can do the work that a heavy specification document used to, while leaving room to adapt 
as understanding evolves. I would also place greater emphasis on failure mode analysis 
early in the process — asking not just "how does this component work?" but "what happens 
when it fails?" That discipline is especially critical in distributed systems, where 
component failures are expected conditions the design must accommodate from the start.

### Being a Good Team Member in Software Development

Good team membership in software development goes beyond writing clean code. It means 
making your reasoning visible to others — through documentation, clear commit messages, 
and design artifacts like UML diagrams — so that teammates can build on your work without 
having to reverse-engineer your decisions. It means being honest about uncertainty early 
rather than discovering problems late when they are expensive to fix.

It also means holding the shared goal of the product above individual preferences. The 
domain model in this project — particularly the `Entity` superclass that reduces 
duplication across `Game`, `Team`, and `Player` — was valuable not because it was 
clever, but because it made the codebase easier for anyone on the team to understand and 
extend. Decisions that optimize for team comprehension over individual expression tend to 
produce better software and stronger collaboration over time.

Finally, being a good team member means being a reliable communicator across roles. 
Software teams include people with different backgrounds — developers, clients, project 
managers, QA engineers — and the ability to translate between technical and non-technical 
language is as important as any coding skill. This document was written with that 
dual audience in mind, and developing that habit is something I intend to carry into 
every future project.
